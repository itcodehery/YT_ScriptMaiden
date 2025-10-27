# Rust Programming Language

Hi guys, my name is Hari and this is Project Directory, where we'll be focusing on
technical videos related to projects that I and the OSS community have built.

Today, I'm going to tell you why Rust is not just another programming language, its
a paradigm shift in how we think about building reliable software. The structure
of this video is inspired from one of the most helpful Rust YT channels called
NoBoilerplate.

That being said, this talk moves fast. Everything you see here will be available as
an open-source markdown document on my Github. Link in the description.

---
## AGENDA
Here's what we'll be covering today. We'll learn more about what exactly it means to
be a low level language. We'll have a lot of comparisons with other languages. I'll then
try my best to explain how Rust differs from traditional programming. We'll talk about what
and where Rust is used? We'll look at basic code snippets and finally talk about how you
can start learning Rust. Consider this a speedrun of Rust's language features.

---
## CONTEXT
Currently I'm at college doing my Masters on Computer Applications, and my fellow classmates
and colleagues at internships do not have much experience shipping code to production. Even so,
there are things that eventually every software developer needs to learn - how to write clean,
maintainable code, how to write test cases for each level of abstraction and so on. I have questions
for the general software engineer who already has a bit of experience.

- How many of you have debugged a segmentation fault at 2 AM?
- How many have traced a NullPointerException through five layers of abstractions?
- How many have shipped code, confident it works, only to watch it crash in production?

We accept this. We build monitoring systems, write extensive tests, and pray our error handling catches everything.

What if I told you there's a language where an entire class of these bugs simply cannot exist?

---
## Question: Is this code correct?
I have a simple question here. Is this code correct? This is just a basic snippet of a function
in Javascript that takes a number as a parameter and returns a +1 value of it. Is this code correct?

```javascript
function increment(n) {
  return n + 1;
}
```

As software developers, we often need to think about times at which a particular part of the
software that we ship might fail. That is why we think about edge cases and write test cases to ensure
every possible input is handled carefully. 
So my rephrased question would be - How can this function fail? or What is the scenario(s) that this function
might fail?

- Is n always a number? What if I pass in a string?
- How large is n? Can I give it a value so big, it breaks my database layer at 2am?
- Could n be modified while we are reading its value?
- What about negative numbers? or floating values? Does this function accept all of those
values?

Although as software engineers, we would think of all of these and write test cases for the function
to ensure the increment function always works as intended, and that would be right, but even so, with
programming languages similar to Javascript, there is one thing common among them - Uncertainty.

---
## What is Rust?
- Rust is a systems programming language built by the Mozilla team that places a huge bet on memory safety without a garbage collector.
- For rookie devs, think of it as C++ performance meets Python-like ergonomics.
- It has a powerful type system that makes invalid states unrepresentable.
- Here are some features of Rust - which we'll be talking about briefly later.
- And it has the best compiler error messages you've ever seen. The compiler doesn't just tell you what's wrong—it tells you how to fix it.

---
## Hello World
Here's the code snippet for a hello world program in Rust.
Familiar syntax. C-like. Nothing scary here.
But watch what happens when we get more complex.

```rust
fn main() {
  println!("Hello, World!");
}
```

## Defining Variables
Rust has a well-defined strongly typed system similar to C, but Rust doesn't always
explicitly ask the programmer about the type. Rust's compiler tries to infer the type on assignment and
holds you to that rule. Once a variable binding is defined with a value of a type, it cannot be assigned
a value of another type.

```rust
fn main() {
  let x: i32 = 10;
  println!("{}",x);
}
```

## Mutability - 1
Here's another program with seemingly simple syntax again. All we do here is read a number from the user,
print it to the default terminal, ask the user for another number and assign it to the same variable, and
print it again to the console. I have used the external crate text_io for ease of console input here, but
apart from the necessary abstraction there, this code looks valid and should run.

```rust
use text_io::read;

fn main() {
  println!("Enter a number: ");
  let x: i32 = read!();
  println!("The number is {}",x);

  println!("Enter another number: ");
  x = read!();
  println!("The number is {} now!");
}
```

That is until you hear the compiler throwing an error at compile time. This code will not compile.

Let's look at the exact compile error.

## Mutability - 2
Here we have the very-detailed compile time error for the program from Cargo, Rust's build tool and compiler.
It tells us that we tried to assign a new value twice for a variable that is immutable, and it asks us,
the programmer, to explicitly define the variable as mutable. 

Variables are immutable by default in Rust. You opt into mutability.
This single design decision prevents countless bugs.


---
## Benchmarks
Rust is fast. Really fast.
In the Computer Language Benchmarks Game:

Rust matches C++ performance,
is 10-100x faster than Python,
is 2-5x faster than Java,
and uses a fraction of the memory,

And you get this speed without sacrificing safety.
Let's compare some of those languages directly with Rust.

---
## vs. Python
Python is beautiful. We all love it.
It's the language of data science, machine learning, scripting. The syntax is clean. The ecosystem is massive.
But Python is slow. Really slow.
Your API handles 100 requests per second. A Rust version? 10,000 requests per second.
Your data processing script takes 10 minutes. The Rust equivalent? 10 seconds.
And memory usage? Python's interpreter overhead means even a simple script uses 50MB. Rust? 2MB.

## vs. Java
Java dominated enterprise development for decades.
It's fast enough. The JVM is battle-tested. The ecosystem is enormous.
But Java has problems.
- Memory: Your Spring Boot microservice uses 500MB at idle. A comparable Rust service? 5MB.
- Startup time: Java needs to warm up the JVM. Your serverless function has 2 seconds of cold start. Rust? 50 milliseconds.
- Null pointer exceptions: Java's billion-dollar mistake. Every reference might be null.
In Rust, there is no null. If something might not exist, it's wrapped in an Option<T> type. The compiler forces you to handle it.

## vs. C++
C++ is the king of systems programming.
It's what operating systems, game engines, and browsers are built with.
It gives you complete control. Zero overhead. Maximum performance.
But that control comes at a cost.
- Memory safety: In C++, memory bugs are your responsibility.
- Data races: C++ has threads. You can share data between them. Good luck debugging when two threads modify the same data.
- Modern tooling: C++ has CMake, Make, dozens of build systems. Package management is a nightmare.

Rust has Cargo. One tool. Builds, tests, documentation, dependencies—all handled.
Rust is what C++ would be if we redesigned it today with 40 years of hindsight.

---
## FEATURES OF RUST
Memory Safety (With a Safety Net)

Rust’s number one job is preventing the most common bugs of languages like C and C++.
It stops **use-after-free**, **buffer overflows**, and those **null pointer errors**—all at **compile time**.
If the code has a memory error, it simply won't build. 

Zero-cost abstractions mean you get all the convenience of a powerful, high-level language, but the
resulting machine code runs just as fast as if you had meticulously handwritten the most efficient
assembly possible. No trade-offs here—just performance.

A Type System That Knows Your Business
Rust’s type system is incredibly powerful. It uses Algebraic Data Types (ADTs) to help you model
your problem domain so precisely that you can literally make invalid states unrepresentable.
If it compiles, it's correct by design.

Explicit Error Handling: No More Nulls. The big one: Rust has no `null`. That eliminates an entire class of painful runtime errors.
Instead of guessing if a value exists, the compiler forces you to deal with uncertainty head-on
using two types:

* `Option<T>` for values that might be missing.
* `Result<T, E>` for operations that might fail.

You must handle every success and failure case. The compiler is your quality control,
making sure nothing slips through.

Composition with Traits. Rust approaches Object-Oriented Programming without the complex inheritance hierarchies. It’s built on composition:
* Structs hold your data.
* Traits define your behavior.

You mix and match traits onto structs to build complex systems from simple, focused pieces.
It gives you the benefits of OOP (clear structure and defined behavior) but keeps the codebase flexible and maintainable.

Writing concurrent, multi-threaded code is notoriously difficult—except in Rust.
The language's famous borrow checker prevents the most dangerous kind of concurrency bug: data races.

You simply cannot accidentally share mutable state between threads.
If your concurrent code passes the compiler, you can be absolutely confident that it is thread-safe.

---
## WHAT CAN YOU BUILD USING RUST
- Systems programming: Operating systems, file systems, device drivers, but that's not all.
  Rust is a mature programming language, we wouldn't be talking about it if it was a
  niche product. So, the possibilities are endless. 

- You can build Desktop apps using native crates for each platform.

- Or you can use one of the amazing cross-platform libraries we have, to build across platforms.

- Rust can help you build apps for Mobile as well using the Tauri framework.

- Write blazingly fast APIs and microservices using the Rocket.rs crate.

- Write scalable frontends for your web apps using the Yew or Dioxus frameworks.

- Maybe you are a game developer looking for more control - look no further.
  The Bevy engine makes it easy for new developers to build on top their own game engines.
  If you don't want to write a game engine yourself, the Godot Engine now has full
  support for scripting using Rust alongside Godotscript.

- Maybe AI/ML is your jam, in that case various crates like the cuda_std crate allows
  devs to take advantage of GPU hardware for parallel processing.

- If you are building software for Embedded Systems, Rust's rich ecosystem of crates
  allow devs to maximize the use out of the hardware using crates like esp-rs.

---
## MOST LOVED LANGUAGE
This is one of the reasons why Rust has become the most loved language for developers to write code
in, since its 1.0 release, and it all relies on one huge promise that Rust makes...

## PROMISE: YOUR CODE WILL NOT CRASH
Your code will not crash. Rust ensures this by means of its main two principles - The Ownership and
Borrowing pattern and the strong and rich Type System.
Rust ensures that if you satisfy the compiler, your code will not crash at runtime because of memory errors.
This means no more dangling pointers, use after move errors, memory leaks etc. 

Let's look at an example to demonstrate the Ownership and Borrowing Model. Here's where Rust gets interesting.

```rust
fn main() {
    let x = String::from("Hello World!");
    println!("{}", x);
    let y = x;
    println!("{}", x);
}
```

Here's a simple program that has two variables x and y and x is defined as a String type, binding the value
"Hello World!". It is printed once before the variable is passed on to y, and then I try to print the value
of x. This code will not compile. The Rust compiler tells us that this is a case of 'use after move'.

What essentially is happening here is, x and y are variable bindings which are owners of their particular
values. Variable x is the owner of the value "Hello World!". When one variable is assigned to another, the
ownership of that value gets transferred to that another variable, which in this case is y, and that means,
x is no longer valid and cannot be used.

This is the crux of the Ownership system. It is a simple design change that helps developers avoid many memory
bugs at compile time instead of runtime. You cannot use a value that has been already transferred to another
part of the program. Now this presents another problem, how do we fix that error?

This is where the Borrowing Model comes into play. 
 
```rust
fn main() {
    let x = String::from("Hello World!");
    println!("{}", x);
    let y = &x;
    println!("{}", x);
}
```

This is a simple change but a meaningful one. Now instead of transferring ownership of the value over to y, I'm
telling the compiler that you can borrow the value of x to read it, but the original owner of the value is going
to be still x. That is what the ampersand (&) symbol means. It means that the value of x is borrowed by y, but x
still retains the ownership of the value. This code now compiles with no error.

Veteran developers might recognize this as an explicit way to define pass by reference and pass by value by code.
Rust's smart design makes you think before making each choice because at the very least, for programs that care about
their runtime performance, small things like these make a large difference.


```rust
fn main() {
    let x = String::from("Hello World!");
    println!("{}", x);
    let y = x.clone();
    println!("{}", x);
}
```

This is how we pass by value in Rust. Any value can be literally cloned and passed as a value. Now x and y
hold different strings with the same characters and are owners of each value respectively.

---
## OBEY THE COMPILER
Rust isn't difficult, it's unfamiliar. It is a very smartly designed language that relies on limiting the developer's freedom to
achieve that greatness. This can seem counterintuitive for people moving from other high level languages like Python and Javascript
to understand. When they code in JS, they make a change and boom, the hot reload immediately makes the change
and shows the change on the UI, regardless of whether it's functioning correctly or showing an error.

---
## ERROR-DRIVEN DEVELOPMENT
Tris from NoBoilerplate put it correctly when he named this approach "Error-driven development". Not that there is anything wrong
with that, but compiled languages have to make the sacrifice between quick iteration and maximal performance as the overhead that
features like hot reload bring is simply not worth the performance decline.

In Rust, if you iterate quickly without thinking about your actions firsthand, the compiler will shout at you.
There is only one rule to writing good rust code - Obey the Compiler. Do what the compiler says, be nice to it and it will be nice to you.
Tris calls this 'Compiler-driven development'.

Let's do a fun example to show how helpful a compiler can be. We are going to refer a previous snippet of code that we had - the increment
function in Javascript and simply paste it onto a fresh rust file.

```rust
function increment(n) {
  return n + 1;
}
```

The compiler immediately panics when we run it with cargo run, with an error:

```
error: expected one of '!' or '::', found 'increment'
|
help: write 'fn' instead of 'function' to declare a function
```

Not only is the error clearly given with the line and character address, the
compiler is generous enough to help us with the error first-hand. It asks us
to correct the syntax by providing us the correct syntax. Onwards...

```
error: expected one of `:`, `@`, or `|`, found `)`
 --> src\main.rs:1:15
  |
1 | fn increment(n) {
  |               ^ expected one of `:`, `@`, or `|`
  |
  = note: anonymous parameters are removed in the 2018 edition (see RFC 1685)
help: if this is a `self` type, give it a parameter name
  |
1 | fn increment(self: n) {
  |              +++++
help: if this is a parameter name, give it a type
  |
1 | fn increment(n: TypeName) {
  |               ++++++++++
help: if this is a type, explicitly ignore the parameter name
  |
1 | fn increment(_: n) {
  |              ++
```

Rust is explicitly typed wherever the compiler cannot infer types on its own. It
asks us to declare its type by giving us all the methods to do so in this helpful
panic message.
Let's add the type as int for an Integer.

```
error[E0412]: cannot find type `int` in this scope
 --> src\main.rs:1:17
  |
1 | fn increment(n: int) {
  |                 ^^^
  |                 |
  |                 not found in this scope
  |                 help: perhaps you intended to use this type: `i32`
```

Primitive types like integers and floats are declared with their bit sizes so the
programmer knows the exactly size each integer value would take up in memory.

```
error[E0308]: mismatched types
 --> src\main.rs:2:12
  |
1 | fn increment(n: i32) {
  |                     - help: try adding a return type: `-> i32`
2 |     return n + 1;
  |            ^^^^^ expected `()`, found `i32`    
```

Ah. We haven't specified the return type. Rust tells us how to do so in its syntax as well.

Finally we'll need a main function to test this out.

```rust
fn main() {
  println!(increment(1));
}
```

This code again doesn't compile.

```
error: format argument must be a string literal
 --> src\main.rs:6:14
  |
6 |     println!(increment(1));
  |              ^^^^^^^^^^^^
  |
help: you might be missing a string literal to format with
  |
6 |     println!("{}", increment(1));
  |              +++++
```

Rust finally informs us the syntax of the println! macro, and now we're done.

```
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.02s
     Running `target\debug\sometest.exe`
2
```

With none of the uncertainties that we had earlier too, all thanks to Rust's
robust type system and the smartest compiler in the business. 

---
## POPULARITY
Nine years.
Stack Overflow runs a survey every year asking developers what languages they love working with.
Rust has won nine years straight.
Not "most used." Most loved.
The people writing Rust? They're not going back to C++. They're not going back to Java.

And it's not just hobbyists voting in surveys.
Microsoft is rewriting parts of Windows in Rust.
The Linux kernel—thirty years of pure C—now accepts Rust code.
Amazon's running critical AWS infrastructure on it.
These aren't experiments. This is production code handling billions of requests.

When Discord rewrote their service and saw latency drop from seconds to milliseconds—that got people's attention.
When Dropbox rewrote their sync engine and it just worked better—people noticed.
When Cloudflare built their entire edge platform on Rust—the industry took note.

Some of my favorite open-source projects are written in Rust like the Helix Editor which
I use to code Rust and also write this script in. Wezterm, my Windows Terminal replacement.
UV, a faster, simpler pip replacement for Python, and Terminal based coding tool called Codex,
developed by OpenAI. All tools developed using Rust.

---
## HOW TO GET STARTED

You want to learn Rust. Where do you start?
First: Install Rust.

Go to rustup.rs. One command installs everything.

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Five minutes later you have Rust, Cargo, and the entire toolchain. Mac, Linux, Windows—doesn't matter.

Second: Read The Book.
Which is the recommended way for developers to start learning Rust. It is available at
doc.rust-lang.org/book. It's free. It's comprehensive. It's actually well-written.
You'll build a guessing game, a command-line tool, a web server. By the end you
understand ownership, borrowing, and lifetimes. The concepts that make Rust different.

Third: Rewire your brain.
Rust syntax will feel overwhelming at first. Especially coming from Python, JavaScript,
or dynamically typed languages. The compiler will fight you.
The Rust compiler catches bugs before you run the code.
Not at runtime. At compile time.

Rust has a learning curve. You'll spend your first week confused. Your second week frustrated.
Your third week you'll start to get it. And then you'll wonder how you ever wrote code without these guarantees.


Have an open mind.

Rust is different. It's not Python with types. It's not safer C++.
It's a fundamentally different way of thinking about memory and ownership.
If you approach it expecting it to work like other languages, you'll struggle.
If you approach it ready to learn, you'll discover something powerful.

The learning resources are excellent. The Book teaches concepts step by step.
Rust by Example shows working code for every pattern.
Rustlings gives you exercises that fail until you fix them—learning by doing.
The community is welcoming. The documentation is thorough.
The compiler error messages are the best you've ever seen.

---
## EXPLORE ON YOUR OWN
This is a comprehensive list of things that maybe you can explore on your own about
Rust and its different features. The more you explore, the more you learn.

## YT CHANNELS
Here are some YouTube channels to learn and stay curious on your Rust journey.

---
## CONCLUSION
You won't master Rust in a weekend.
But you don't need to.
Start small. Build one thing. Then another.
Thank you for watching!
