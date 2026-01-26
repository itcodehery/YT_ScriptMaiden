# Building a Borrow Checker for C

---
## TITLE

## Building a Borrow Checker for C! (kinda) | Rust to the Rescue
### Description: Porting Rust's Ownership Model to C using Static Analysis
#### Quirk: Fast-paced, heavy on the "Why", Technical deep dive into ASTs

Hi guys, my name is Hari and this is Project Directory, where we'll be focusing on technical videos related to projects that I and the OSS community have built.

So it's been a while since my last video and I have been keeping myself busy with college. But today we are attempting something that I've been working on for a while at college that might also be slightly controversial. We are going to try and fix the C programming language.

Before we dive into today's project, recently I've been interested on how a Programming Language is created. Let's talk about how compilers actually work. We often think of them as black boxes that turn code into binaries, but under the hood, they are complex pipelines. A compiler reads your text (which is called Lexing), understands its structure (which is Parsing), and then—crucially—analyzes its meaning (that is Semantic Analysis or the Grammar Check in simpler words).

This third step — Semantic Analysis — is where Rust truly separates itself from the pack. While most compilers just check if you missed a semicolon or used the wrong type, `rustc` runs its famous "Borrow Checker" here. It uses this phase to prove your code is memory-safe *before* it ever lets you run it.

If you've watched my previous videos, by now you know I love Rust. I love the safety guarantees, the borrow checker, the way it stops me from shooting myself in the foot. But the reality is, the world runs on C. The Linux Kernel, Git, Python's interpreter, your toaster—it's all C.

And C is dangerous. It lets you manage your own memory, which means it lets you mismanage your own memory. Double frees, use-after-frees, uninitialized memory usage—these are the bugs that cause 70% of security vulnerabilities in major software.

So then came the thought. What if we could take the "Brain" of Rust—the static analysis, the ownership tracking—and apply it to C? What if we could write a tool that reads C code but judges it with Rust's strict standards?

Enter **Caw**. A Borrow Checker for C, written in Rust.

This isn't a rewrite. It's a static analysis tool that parses your C code and yells at you if you violate ownership rules that C doesn't even natively have. We are enforcing Rust's discipline on C's chaos.

Let's build it.

---
## AGENDA
Here is the plan for this 15-minute speedrun:
1. **The Architecture:** How do we even read C code? We'll talk about Lexers and Parsers.
2. **The AST:** How Rust's Enums make representing language syntax incredibly easy.
3. **The Brain:** Implementing the Symbol Table and the Ownership Logic. This is where the magic happens.
4. **The Demo:** We'll run it against some "valid" C code that is actually a memory safety nightmare.

---
## THE FOUNDATION: PARSING C
To analyze code, we first need to understand it. We can't just regex our way through a C file; that's just madness. We need to build a proper Parser.

I didn't know about this before I started this project but the way I go about doing this is by building something called a custom recursive top-down parser here sometimes also known as a Recursive Descent Parser. It starts with a **Lexer**.
What the Lexer essentially does is that it takes a stream of raw text characters and chops them into meaningful chunks called Tokens.

```rust
pub enum Token {
    Int, Char, Float, Return, If, While,
    Identifier(String),
    Number(i32),
    LeftBrace, RightBrace, 
    // ... and so on
}
```

If you look at `src/c_parser/lexer.rs`, you'll see it's just a state machine. It eats whitespace, identifies keywords like `int` or `return`, and bundles everything else into Identifiers or Literals.

Once we have tokens, we feed them into the **Parser**.
The Parser turns a flat list of tokens into a hierarchical structure called an **Abstract Syntax Tree** or AST. This is where Rust really shines.

### Algebraic Data Types for ASTs

An Abstract Syntax Tree (AST) is a tree-like representation of the source code's structure, think of the AST as the skeleton of your code. It takes that flat list of tokens and organizes them into a tree structure that actually shows how the code fits together. In Java or C++, an AST node might be a base class with a dozen subclasses. In Rust, we use Enums.

Look at how clean this data modeling is in `src/c_parser/ast.rs`:

```rust
#[derive(Debug, PartialEq, Clone)]
pub enum Statement {
    Return(Expr),
    If(Expr, Box<Statement>, Option<Box<Statement>>),
    While(Expr, Box<Statement>),
    VarDecl(Type, String, Option<Expr>),
    Block(Vec<Statement>),
    Expr(Expr),
}
```

A `Statement` can *be* a `Return`, or an `If`, or a `VarDecl`. We don't need null checks or dynamic casts. We just pattern match.
The `If` variant holds the condition (an `Expr`), the "then" branch (a `Statement`), and optionally an "else" branch.
Notice the `Box<Statement>`. Since `Statement` is recursive (an If statement contains other statements), we have to box it so the compiler knows the memory size.

This structure allows us to represent the entire C program as a tree of data that we can traverse.

---
## THE BRAIN: STATIC ANALYSIS
Now that we have the tree, we need to traverse it and look for crimes.
This logic lives in `src/analysis/mod.rs`.

We need to track the "State" of every variable. In C, a variable is just bytes. But in our Borrow Checker, each variable needs to have a state.
I've defined this state in `src/analysis/scope.rs`:

```rust
#[derive(Debug, PartialEq, Clone, Copy)]
pub enum VariableState {
    Uninitialized,
    Owned,
    Moved,
    Dropped,
}
```

This is the core of the project. The Parser should now recognize these states of variables.
- **Uninitialized**: You declared `int x;` but gave it no value. Reading this is a bug.
- **Owned**: You have the data. You are responsible for it.
- **Moved**: You gave the data to someone else. You can no longer use it.
- **Dropped**: The scope ended, and the variable is gone.

### The Symbol Table
Now about the Symbol Table. A Symbol Table is basically the compiler's address book. It's where it writes down every variable name you invent and keeps tabs on them.
Whatever variable we create, we track its state across scopes. When you enter a `{` block in C, you enter a new scope. When you hit `}`, that scope dies.
I implemented a `SymbolTable` struct that essentially manages a Stack of HashMaps.

```rust
pub struct SymbolTable {
    scopes: Vec<HashMap<String, (VariableState, bool)>>,
}
```

Think of it like a stack of transparent papers. Every time you enter a new block of code (a new scope), we put a fresh sheet of paper on top.
When we `enter_scope()`, we push a new HashMap (that fresh sheet) onto our stack. When we `declare()` a variable, we write it on that top sheet. When we `lookup()` a variable, we look at the top sheet first; if it's not there, we peek through to the sheet below it. This is exactly how shadowing works—a variable on a newer sheet "hides" one with the same name on an older sheet.

### Enforcing Ownership
Now, the controversial part. C doesn't have ownership. If I do:

```c
int x = 10;
int y = x;
return x;
```

In C, this is valid. `x` is copied to `y`. Both exist.
But Caw enforces **Move Semantics**. We are essentially pretending C is Rust to catch logic errors.
In `src/analysis/mod.rs`, look at how we handle assignment:

```rust
Expr::Binary(left, op, right) => {
    if let BinaryOp::Assign = op {
         // Check the Right side
         if let Expr::Variable(rhs_name) = &**right {
             // If the right side is OWNED, we MOVE it.
             if self.symbols.lookup(rhs_name) == Some(VariableState::Owned) {
                  self.symbols.update(rhs_name, VariableState::Moved);
             }
         }
         // ...
    }
}
```

If you assign `y = x`, our analyzer marks `x` as **Moved**.
If you try to use `x` afterwards, the analyzer sees `VariableState::Moved` and throws an error:
`"Error: Use of moved variable 'x'"`

We basically implemented a "Use After Move" checker for a language that doesn't strictly support moves. Why? Because if `x` was a pointer to malloc'd memory, and you copied that pointer to `y`, and then both `x` and `y` freed that memory... you have a double-free vulnerability. By enforcing moves, we prevent multiple variables from thinking they own the same resource.

---
## RUNNING THE BEAST
Let's see it in action.
I have a simple CLI built with `clap` in `src/main.rs`. It takes a file, parses it, and runs the analyzer.

Let's take a look at `test_ownership.c`.

```c
int main() {
  int x = 10;
  int y = x;
  return x;
}
```

If I compile this with `gcc`, it's fine. It runs.
But let's run it through Caw.

```bash
cargo run -- --file test_ownership.c
```

**Output:**
```
Parsing C file: test_ownership.c...
Successfully parsed C file!
Running Static Analysis...
Static Analysis Failed:
  - Error: Use of moved variable 'x'
```

It caught it! We assigned `x` to `y`, so `x` is moved. The return statement tries to read `x`, and Caw says "Absolutely not."

Let's try another one. Uninitialized memory.

```c
int main() {
    int x;
    return x;
}
```

The analyzer checks the Symbol Table for `x`. It finds it, but the state is `VariableState::Uninitialized`. Boom. Error.

---
## WHY DOES THIS MATTER?
You might ask, "Hari, isn't this overkill? C integers copy by default, they don't move."
Yes, for integers, it is overkill. But think about pointers. Think about file handles. Think about sockets.
In large C codebases, passing a pointer around without knowing who "owns" it (i.e., who is responsible for freeing it) is the #1 cause of leaks and crashes.

By forcing a programmer to be explicit—or by at least visualizing where ownership is transferred—we can catch logic bugs before they become security patches.

Currently, Caw is in its early stages. It treats everything as a move, which is aggressive. A future version would differentiate between `Copy` types (like int) and `Move` types (like pointers), similar to how Rust does it.

## CONCLUSION
So, what did we learn? Building a compiler frontend isn't black magic. It's just data structures.
We used:
1. **Enums** to represent the Syntax Tree.
2. **HashMaps** to represent Scope.
3. **Pattern Matching** to traverse the tree and apply rules.

This might honestly be the most fun I've had in a while writing code and Rust made this tooling incredibly easy to write. The entire project—Lexer, Parser, Analyzer, CLI—is less than 1000 lines of code.
If you're interested in compilers, don't start by trying to build LLVM. Start by building a linter. Start by building a syntax highlighter. Start by building Caw.

The source code for Caw, initially called Borcom, is available in the description. Clone it, break it, add support for pointers, and let me know how it goes.

Thank you for watching, and stay curious!
