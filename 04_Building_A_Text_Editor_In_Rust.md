# Building a Text Editor (Part 1)

---
## TITLE

## How Does a Word Processor Even Work? | Building a Text Editor in Rust | Part 1: Representing State
### Description: Project Setup and Modeling our Data
#### Quirk: No Music For This One

Hi guys, my name is Hari and this is Project Directory, where we'll be focusing on technical videos related to projects that I and the OSS community have built.

I'm back with a bit of a technical video today and we're gonna be learning how to build our own text editor from scratch. Obviously if you have watched my previous videos, you know by now how much I value a good text editor, so here's a simple video on how the process of building a text editor goes through its stages. 

This is going to be a multi-part series and part one of this series which you are watching right now revolves around Project Setup, Data Modeling, and a cool design pattern of Rust that I found and how I repurposed it for a specific feature of the text editor, which is, Modality.

You should know that *we* are going to learn to do this together, because I am just starting out as well with this project, and you will get to see how I take a project myself through its paces and you'll see me struggle a bit too but hopefully it teaches you how to create anything by just having fun and being curious while doing it.

So let's begin.

## What is a Text Editor?
Well let's start with clearly defining what we're building, which is a text editor, not a Code editor though. That comes with its own set of complexities and unnecessary features that might take its own video. It's always good to narrow down the basics of what your software must do. In our case, it just has to behave like a notepad clone on the terminal. Maybe I'll give it different  modes like Vim and Helix 'cuz I seem to like those.

`Detour`
Real quick, for those who don't know what a Modal text editor is, we have different modes to do different things like Navigation, Editing, and Selection, and most of the common modal editors are centered around using your keyboard more. Vim for example just completely doesn't support the mouse at all because all the actions have a respective keybind on the keyboard to do the same. It might seem less intuitive at first, but eventually a person who knows most, if not all, of the different keybinds works faster than the average person who uses both a mouse and keyboard to maximize productivity.

But for now, we'll focus on three things -> creating/opening a document, writing/deleting characters, and saving the document. Later we'll model our data better to do more, but let's just start with this much. Also, I'm assuming atleast a basic level of understanding on the working of the Rust language, because I'm not going to go over the language and its syntax, but I'll try to explain what exactly that part of the program is supposed to do.

## The Bulb
I saw this project on GitHub about a text editor built in Rust called Hecto, where this person built a whole Vim-like text editor from scratch using Rust. It's not Helix with a whole suite of IDE tools, but it is something comparable to what I want to build on my own. Turns out, they had a whole guide written on how to do exactly the same. So I think we'll follow the same for a while until I see something that I don't understand and go raw from there.

## Terminal Configuration: Raw Mode
[Chapter 2](https://philippflenker.com/hecto-chapter-2/)
Starting with the setup, we can skip all that, I have a Rust project already setup.
Entering Raw mode, yes. Now I've put my terminal in Raw mode before in my project that eventually became the name of this channel. That project was about creating my own Powershell like terminal. How it works is that your terminal emulator by default handles many keybinds and mouse inputs of your input devices and leaves very few inputs from the user to apply to terminal based apps. Raw mode puts all the control of your terminal to the program that you are running.
We use Crossterm for this, its just a low-level implementation to remove control from the terminal emulator and place it onto your program. In Go Lang, you would use the GoKilo library.

```
cargo add crossterm
```

Right, so now that we have all that set up, the main reason we needed raw mode was to read a Key event, or a key input, in our case, the 'q' key so that should, when pressed, quit the program. Obviously Ctrl+C wouldn't work in Raw mode. Let's write the code for it.

```rust
use std::io::{self, Read};
use crossterm::terminal::enable_raw_mode;
use crossterm::terminal::disable_raw_mode;

fn main() {
    enable_raw_mode().unwrap();
    for b in io::stdin().bytes() {
        let c = b.unwrap() as char;
        println!("{}", c);
        if c == 'q' {
			disable_raw_mode().unwrap();
            break;
        }
    }
}
```

[Chapter 3](https://philippflenker.com/hecto-chapter-3/)
The next chapter again goes in detail about language specfic implementations. We're not gonna go into that right now, maybe later.

## Modeling our Data using Structs and Interfaces (aka Traits)
Now let's get into the interesting part -> Modeling our Business Logic.

To explore what business logic means in our case, we need to look at how real life data is modeled in any program.
Now the easiest case of this is to show a real life object and model it using any Object Oriented program. Here's a Java file and I'm gonna create a simple class to represent a Fruit.
Obviously this is one of the examples that school or college teaches you. They ask us to use any real-world object, but always give irrelevant or mundane examples like a Dog, Fruit, Animal etc. Let's take a more nuanced example.

Imagine we're representing a Pokemon. Weird example, I know, but bear with me here. This is how a simple class for a Pokemon would look like:

```java
class Pokemon {
    int id;
    String name;
    PokeType type1;
    PokeType type2;
    String[] abilities;
    BaseStats stats;
}

enum PokeType {
    FIRE, WATER, GRASS, FLYING, ELECTRIC
}

class BaseStats {
    int hp;
    int atk;
    int def;
    int spa;
    int spd;
    int spe;
}
```

And this... is how Pokemon are represented in Pokemon Essentials, an open-source project written in Javascript:

```javascript
class PokemonData {
    id: number = 0;
    name: string = "";
    species: string = "";
    nature: Natures;
    abilities: any = {};
    baseStats = { hp: 1, atk: 1, def: 1, spa: 1, spd: 1, spe: 1 };
    effortPoints = { hp: 0, atk: 0, def: 0, spa: 0, spd: 0, spe: 0 };
    types: string[] = [];
    learnset = {
      levelup: null,
      tm: null,
      tutor: null
    };
    eggGroups = [];
    heightm = 0;
    weightkg = 0;
    color = 0;
    rareness = 0;
    stepsToHatch = 0;
    growthRate: Experience;
    baseSpecies = null;
    forme = null;
    gender = null;
    otherFormes = null;
    pokedex = "";
    genderRatio = { M: 0.5, F: 0.5 };
}
```

This is great, pretty detailed but almost the same. This is how objects in real-life are represented within a program and we're going to do that for ours as well.
Since Rust isn't an Object Oriented Language, we represent data using its type system just like any other functional programming language using Structs and Enums. Using these, let me start with defining the most basic struct in our project, the Line Struct.

```rust
pub struct Line {
    text: String
}
```

Now why did I make a separate struct to just store a line? We know that in Object Oriented programming languages, data and their methods can be stored in one container called their class. Rust doesn't have classes, but it does have something similar to let us implement methods on its types. These are called Implementations. In many languages, this is called an Abstract Class or an Interface, but what it essentially allows us to do is to define functions that work on the variables defined in its particular type.
This will be very helpful later when we implement more functionality to our Line struct. For now though, let me just include a `new()` method. I can create an implementation using the `impl` keyword.

```rust
impl Line {
    pub fn new() -> Self {
        Self {
            text: String::new();
        }
    }
}
```

Next let's begin to model an open document. This essentially should store a list or an array of Lines and store meta data about the open document. Let's just name the struct as Document.

```rust
pub struct Document {
    pub lines: Vec<Line>,
    pub file_format: String,
}
```

A simple document that stores all the lines in a Vector and a simple file_format metadata about the open document. Now here's where it gets a bit interesting. I want to implement more methods on this struct to get specific data out of it. Let's do that.

I'm thinking I need methods to get the line_length of a specific line, the line_count, and the character count. Let me create an impl block and write those function signatures.

```rust
impl Document{
    pub fn new() -> Self {
        Self {
            lines: vec![],
            file_format: ".txt".to_string(),
        }
    }

    pub fn line_length(&self, line_idx: usize) -> usize {}

    pub fn line_count(&self) -> usize {}

    pub fn char_count(&self) -> usize {}
}
```

It's a good idea to reason yourself about what each function should do and how it is achieved using your programming language, so I'll leave that to you guys to figure out. Let's not discuss about the syntax, I'll just add the implementations.

```rust
impl Document{
    pub fn new() -> Self {
        Self {
            lines: vec![],
            file_format: ".txt".to_string(),
        }
    }

    pub fn line_length(&self, line_idx: usize) -> usize {
        self.lines.get(line_idx).map(|l| l.text.len()).unwrap_or(0)
    }

    pub fn line_count(&self) -> usize {
        self.lines.len()
    }

    pub fn char_count(&self) -> usize {
        self.lines.iter().map(|l| l.text.len()).sum()
    }
}

```

In any modern text editor, there is the concept of Tabs, or multiple open documents at the same time. In terminal text editors like Neovim and Helix, they're called Buffers, and we can have multiple buffers open at the same time. We also have an initial empty buffer to directly type into. I want to include this as well into our app, and that leads us into the Editor struct.

The Editor struct represents a running instance of our app which manages the multiple running Buffers or in our case, variables of the Document struct. So let's build that.

```rust
pub struct Editor {
    buffers: Vec<Document>,
    current_focused_idx: usize,
    is_quittable: bool,
}
```

We'll also implement a few methods to work with the Editor struct, starting with new:

```rust
impl Editor {
    pub fn new(buffers: Vec<Document>) -> Self {
        Self {
            buffers,
            current_focused_index: 0,
            is_quittable: true,
        }
    }
}
```

I need functions to switch between open buffers. This is as simple as adding two functions to switch buffers forward and backward by increasing and decreasing the current focused index value respectively.

```rust
impl Editor {
    pub fn new(buffers: Vec<Document>) -> Self {
        Self {
            buffers,
            current_focused_index: 0,
            is_quittable: true,
        }
    }

    pub fn buffer_switch_forward(&mut self) {
        if !(self.buffers.len() < 2) {
            if self.current_focused_index + 1 == self.buffers.len() {
                self.current_focused_index = 0;
            } else {
                self.current_focused_index += 1;
            }
        }
    }

    pub fn buffer_switch_backward(&mut self) {
        if self.buffers.len() < 2 {
            return;
        } else {
            if self.current_focused_index == 0 {
                self.current_focused_index = self.buffers.len();
            } else {
                self.current_focused_index -= 1;
            }
        }
    }
}
```

What essentially this code does is that it checks first if multiple buffers exist to switch between. If yes, it then checks if the user is already in the last open buffer, and if that's the case, it goes to the first buffer, and if not, it just moves to the next buffer index. The switch backward function does the opposite.
Notice that the new functions take a mutable reference to the variable that we're calling from, this is because we're modifying the variable itself. We require a mutable reference here.

## Modality

Now comes the concept of Modality. In my previous video about Vim, I explained that Vim is a modal text editor -> that means that it has different modes depending on what exact action we want to perform. Normal Mode for moving around the document and performing quick actions like cut, copy and paste. Insert mode for inserting text into the document. Visual Mode for performing actions on selections, and Command Mode for executing editor commands.

So I also want the Editor struct to store the state of the editor which can be one in four -> Navigate Mode, Edit Mode, Select Mode, and Command Mode. Let's add that in the form of an enum so that we can make invalid states unrepresentable.

```rust
pub enum EditorState {
    Navigate, Edit, Select, Command
}

pub struct Editor {
    buffers: Vec<Document>,
    current_focused_idx: usize,
    is_quittable: bool,
    state: EditorState,
}

```

Our new Editor struct looks like this, with a state variable to track its current state. Great, so now we can just include methods to switch between them, like so.

```rust
impl Editor {
    pub fn new(buffers: Vec<Document>) -> Self {
        Self {
            buffers,
            current_focused_index: 0,
            is_quittable: true,
            state: EditorState::Navigate, 
        }
    }

    pub fn to_navigate_mode(&mut self) {
        &self.state = EditorState::Navigate;
    }

    pub fn to_select_mode(&mut self) {
        &self.state = EditorState::Select;
    }

    pub fn to_command_mode(&mut self) {
        &self.state = EditorState::Command;
    }

    pub fn to_edit_mode(&mut self) {
        &self.state = EditorState::Edit;
    }
}
```

## Making Invalid States Unrepresentable: Raw State Design Pattern
Our current implementation for Modality is good, but now we run into a new problem. There are still absolutely invalid states that can be represented using our current implementation. The methods to switch between each mode is accessible from any mode, but for someone who, in the future, might make modifications to this API, I want to enforce that some modes cannot be accessed when I'm in a specific mode.

So how do I make it so that few methods are not available to use when the editor is in a specific state. 

One possible solution is to create separate Structs for each state and implement different traits for both.

```rust
/// Represents the Editor in Navigate Mode
pub struct NavigateEditor {
    pub buffers: Vec<Document>,
    pub current_focused_index: usize,
    pub cursor_col: usize,
    pub cursor_line: usize,
    pub is_quittable: bool,
    pub command_line: String,
    pub error_line: String,
}

/// Represents the Editor in Edit Mode
pub struct EditEditor {
    pub buffers: Vec<Document>,
    // ... same fields as NavigateEditor
}

/// Represents the Editor in Select Mode
pub struct SelectEditor {
    pub buffers: Vec<Document>,
    // ... same fields as NavigateEditor
}

/// Represents the Editor in Command Mode
pub struct CommandEditor {
    pub buffers: Vec<Document>,
    // ... same fields as NavigateEditor
}
```

And write their implementation like this: 

```rust
impl NavigateEditor {
    pub fn handle_input(&mut self, key: KeyEvent) -> EditorAction {
        unimplemented!()
    }

    pub fn enter_edit_mode(self) -> EditEditor {
        unimplemented!()
    }

    pub fn enter_command_mode(self) -> CommandEditor {
        unimplemented!()
    }

    pub fn enter_select_mode(self) -> SelectEditor {
        unimplemented!()
    }
}

// And so on for EditEditor, SelectEditor, and CommandEditor...
```

We can then do this to use them together like this.

```rust
fn main() {
    let editor: NavigateEditor = NavigateEditor::new();
    // Can only use Navigate editor Implementations
    let editor: EditEditor = editor.enter_edit_mode();
    // Can only use Edit editor implementations
    let editor: SelectEditor = editor.enter_select_mode();
    // Can only use Select editor implementations
    let editor: CommandEditor = editor.enter_command_mode();
}
```

You can already see that we have solved one solution just to introduce another. The problem with this approach is that reassignment leads to unnecessary cloning of the data and a lot of duplication of code. Fortunately for us, there is a better way to do this in Rust.

## Modeling our Data Better: Type State Pattern
[Let's Get Rusty example: Improve your Rust APIs with the Type State Pattern](https://youtu.be/_ccDqRTx-JU)
There is a brillant video by Let's Get Rusty where he builds a simple Password Manager, he takes us through the stages of representing this exact state problem. This is one of the times where Rust's rich type system comes to our rescue. First instead of spending memory to represent states in the struct or creating multiple structs to represent valid states, we'll go back to our enum example and this time instead of using an enum, we'll be using a Zero-Cost Type.

We start by first representing each of our Editor States as their own empty Structs called Unit-like Structs.

```rust
pub struct NavigateMode;
pub struct EditMode;
pub struct SelectMode;
pub struct CommandMode;
```

The rust standard library includes a provision for us to create and use Zero-Cost types, which are types that are subtracted away during compilation, so they don't take up real memory space unlike an enum to represent. We use the PhantomData type from the `std::marker` module. This allows us to tell the compiler that our `Editor` struct possesses a certain type `State`, even though we don't actually store a value of that type in memory. It's strictly a compile-time marker.

So, we modify our `Editor` struct to be generic over a type `State`.

```rust
use std::marker::PhantomData;

pub struct Editor<State = NavigateMode> {
    buffers: Vec<Document>,
    current_focused_index: usize,
    cursor_col: usize,
    cursor_line: usize,
    is_quittable: bool,
    command_line: String,
    error_line: String,
    state: PhantomData<State>,
}
```

The `PhantomData<State>` consumes no space, but it allows us to define implementations specific to `Editor<NavigateMode>`, `Editor<EditMode>`, and so on.

When we create the editor, we initialize it in `NavigateMode`:

```rust
impl Editor {
    pub fn new(buffers: Vec<Document>) -> Self {
        Self {
            buffers,
            current_focused_index: 0,
            is_quittable: true,
            cursor_line: 0,
            cursor_col: 0,
            command_line: String::new(),
            error_line: String::new(),
            state: PhantomData::<NavigateMode>,
        }
    }
    // ... buffer switching logic ...
}
```

### Handling Transitions

Now, how do we switch modes? Okay here's where the code gets a bit interesting. Rust's Ownership system is so neatly designed that it makes our problem solvable so much more efficiently. In the previous "bad" example, we might have had to clone data or restructure it. Here, we can define a `transition` function that consumes the current editor (takes ownership) and returns a new editor with the new state, moving all the heavy data (like the buffers vector) without cloning it.

```rust
impl<S> Editor<S> {
    fn transition<NewState>(self) -> Editor<NewState> {
        Editor {
            buffers: self.buffers,
            current_focused_index: self.current_focused_index,
            is_quittable: self.is_quittable,
            cursor_col: self.cursor_col,
            cursor_line: self.cursor_line,
            command_line: self.command_line,
            error_line: self.error_line,
            state: PhantomData,
        }
    }
}
```

This is incredibly efficient. It compiles down to a no-op essentially, just reinterpreting the type.

### Specific Implementations

Now we can implement logic that is *only* valid for specific modes. For example, `NavigateMode` should handle movement keys and mode switching, but shouldn't be able to insert text.

```rust
impl Editor<NavigateMode> {
    pub fn enter_edit_mode(self) -> Editor<EditMode> {
        self.transition()
    }

    pub fn handle_input(&mut self, key: KeyEvent) -> EditorAction {
        let mut action = EditorAction::None;
        match key.code {
            Char('i') => action = EditorAction::EnterEditMode,
            Char(':') => action = EditorAction::EnterCommandMode,
            Char('v') => action = EditorAction::EnterSelectMode,
            Char('h') => self.move_cursor_left(),
            Char('l') => self.move_cursor_right(),
            Char('k') => self.move_cursor_up(),
            Char('j') => self.move_cursor_down(),
            KeyCode::Tab => self.buffer_switch_forward(),
            KeyCode::BackTab => self.buffer_switch_backward(),
            _ => {}
        }
        action
    }
}
```

And `EditMode` gets the ability to insert and delete characters:

```rust
impl Editor<EditMode> {
    pub fn insert_char(&mut self, c: char) {
        let buffer = &mut self.buffers[self.current_focused_index as usize];
        buffer.insert_char(self.cursor_line, self.cursor_col, c);
        self.cursor_col += 1;
    }
    
    pub fn enter_navigate_mode(self) -> Editor<NavigateMode> {
        self.transition()
    }
}
```

This way, `editor.insert_char('a')` is a compile-time error if `editor` is in `NavigateMode`. We've successfully made invalid states unrepresentable!

## Finishing our Business Logic

Finally, to communicate with the outer loop (the main function or the UI layer) about what needs to happen (like quitting, saving, or just redrawing), we can return an `EditorAction` enum from our input handlers.

```rust
pub enum EditorAction {
    Quit,
    Save,
    SaveAndQuit,
    QuitAll,
    EnterCommandMode,
    EnterEditMode,
    EnterSelectMode,
    EnterNavigateMode,
    None,
}
```

This gives us a clean API for the frontend. The frontend just calls `editor.handle_input(key)` and gets back an action telling it what to do next or if the state changed.

## Conclusion
And that's it for the core logic setup! We've gone from a basic struct to a robust, type-safe state machine using Rust's Type State pattern. In the next part, we'll actually hook this up to the terminal, handle real keyboard input, and start rendering our text buffers to the screen, I think. I don't even know yet.

If you are interested in how you can build your own text editor, check out the top link in the description which leads to the Hecto project, by Philipp Flenker, that we have been following so far. If you are more interested in the Rust Programming Language and its paradigms, learn Rust using the free Rust Book website, link is again in the description.

Thank you for watching, and stay curious!

