# Building your Terminal Workstation

---
## TITLE

## Stop Using VS Code: Build an AI-Powered Terminal IDE That's Blazing Fast

Hi guys, my name is Hari and this is Project Directory, where we'll be focusing on
technical videos related to projects that I and the OSS community have built.

I'm currently at college and I've always hated how I'd always have to carry a charger
along with my laptop because the charge just wouldn't hold up, not even for more than
a couple of hours. This wasn't just an inconvenience; it was a major bottleneck for
my productivity. My frustrations eventually led me on a search for better
battery-efficient and optimized software to replace the tools that I use at college,
which is mostly just my IDE. This search led me down a rabbit hole on terminal-based text editors.
What's more optimized than not having a GUI at all?

The more I got into it, the more I realized that most, if not all of the tools, that
I use have a more efficient and often better solution in the terminal. I slowly
got into the practice of using my terminal more. This is my comprehensive list of
terminal based tools that I discovered on my journey in no particular order.

---

## Wezterm - My Windows Terminal Replacement

I'm on Windows, and obviously, the default Command Prompt is not a
great experience. For a while, I used the newer Windows Terminal, which is a
big improvement. However, I was looking for something more customizable and
powerful that would work the same across Windows, macOS, and Linux.
That's when I found Wezterm.

Wezterm is a GPU-accelerated, cross-platform terminal emulator and multiplexer,
all written in Rust. It is extremely configurable and uses Lua, which is a simple scripting language
to let the user configure it.
You can customize everything from keybindings, to tabs, to panes, and even
create your own custom workflows. It also has built-in support for
multiplexing, which means you can have multiple terminals running in one
window without needing a separate tool like tmux. For me, Wezterm is
the perfect blend of performance and features, and it has made
my terminal experience on Windows so much better.

Download it on [Wezterm.org](https://wezterm.org)

## Neovim - A Modular Terminal Text Editor

You can't talk about terminal-based text editors without mentioning Neovim.
It's a fork of the legendary Vim modal text editor, but with a focus on
community-driven development through plugins. Neovim is incredibly
powerful and modular. You can turn it into a full-fledged IDE for
any language with the right plugins and configuration.

The configuration is done in Lua, just like Wezterm, which allows for
a very powerful and customized setup. However, this is also its biggest
challenge. Getting started with Neovim and configuring it to your liking
can be a steep learning curve. That's exactly why Neovim has a lot of
prebuilt configs - some of them include LazyVim, LunarVim, NVChad (which used
to be my preferred daily driver), and AstroNVim (my current neovim setup).
While I have a working Astronvim setup that I use
from time to time, I found another editor that fit my needs better
for a daily driver.

To download it, on Windows you can use you inbuilt package manager Winget and
run

```shell
winget install Neovim.neovim
```

or from Scoop

```shell
scoop install neovim
```

On MacOS, you can install it using Homebrew.

```shell
brew install neovim
```

On Linux, refer the Installing Neovim guide for the command for your
specific distribution. Link is in the description. 


## Helix - My IDE Replacement

For the longest time, I was a heavy VS Code user. I loved the features and
the massive extension ecosystem. But as I mentioned, I was on a quest for
a more lightweight and battery-efficient setup. I then switched to Neovim after hearing
a lot on it. Neovim was great, but after trying multiple Neovim prebuilt configs
and finding them a bit slow on Windows, I stumbled upon Helix.

Helix is a post-modern modal text editor that's also written in Rust but is much
lighter. It takes inspiration from Vim's modal editing but simplifies it.
The philosophy is "selections first, actions next", which feels very intuitive.
What sold me on Helix is that it works great right out of the box. It has built-in
support for Language Servers, debugging, and a fantastic fuzzy finder. Because
it's written in Rust, it's blazing fast. For my day-to-day coding, Helix
has become my go-to IDE, and I rarely find myself reaching for VS Code anymore.

Download it on [Helix-Editor.com](https://helix-editor.com/)

## Lazygit - My Git Replacement

Let's face it, while Git is powerful, using it from the command line can be
a bit frustrating, especially when you forget commands and accidentally make an
irreversible change.
That's when I discovered probably my most favorite and most used TUI app, Lazygit,
which I use almost everyday.

Lazygit is a terminal UI for Git. It gives you a visual interface to manage
your repositories. You can easily stage files, create commits, manage branches,
and even handle interactive rebases with just a few keystrokes. It makes my
Git workflow so much faster and I use it almost everyday. I honestly can't imagine going
back to using raw Git commands for everything. It's a must-have for anyone
who uses Git in the terminal.

Install it using your preferred package manager: Winget or Scoop for Windows and
Homebrew for Mac.

## GitHub CLI - Create, View Repos on the Terminal

The GitHub CLI, or `gh`, is another tool I use daily. It brings GitHub to
your terminal. You can create and view repositories, manage pull requests
and issues, and even connect to a codespace, all without leaving your command line.

It's incredibly useful for scripting and automating your GitHub workflow.
For example, you can create a new repository and push your code to it
with just a couple of commands. It's the perfect companion to
Lazygit and makes managing my projects on GitHub a breeze.

Download it on [CLI.GitHub.com](https://cli.github.com/)

## Gemini CLI - My AI Assistant on the Terminal

Okay, here's a thing I thought wouldn't be possible. I thought there's no way I can
replace GitHub Copilot on the terminal... That is until I found Gemini CLI.
Gemini CLI is an AI assistant that I can use directly from my terminal. It's like having
a GitHub Copilot that's lighter and integrated into your terminal.
Exactly what I wanted.

I can ask it to explain a piece of code, suggest a refactoring, or even write a
function for me. It's incredibly helpful when I'm stuck on a problem or just
want a second opinion. Having this power directly in my terminal, integrated
with my workflow, is a game-changer. And... it's also free.

Download it from its official repository:
[github.com/google-gemini/gemini-cli](https://github.com/google-gemini/gemini-cli)

---

## Wezterm + Helix + Gemini CLI - All in one terminal Dev Environment

Now, let's put it all together. My ultimate terminal IDE setup combines Wezterm,
Helix, and the Gemini CLI. This is my workflow for most of the projects that
I work on. With Wezterm's multiplexing capabilities, I can create a layout
that's perfect for my development workflow.

Let me show you some of my favorite dev environments I've created and use myself:

### Rustlings Dev Env (Simple Split)
For example, when I'm working on the Rustlings exercises, I have a simple
vertical split. On the left, I have Helix open to the current exercise file.
On the right, I have a shell where I can run `rustlings` to get instant feedback on my code.

### Flutter Dev Env (Splits)
For Flutter development, I have a more complex layout. I have a pane for Helix to write
my Dart code, another pane running the app on my phone, a pane with
the Gemini CLI ready to help me with any questions I have about the Flutter framework,
a fourth pane in the left with my file explorer `broot` running.

### Rust Dev Env (Splits)
And for my general Rust development, I have a similar setup. Helix on one side,
a shell for running `cargo build` or `cargo run` on the other, the
Gemini CLI in a separate tab for when I need it, and bacon, which is my
background code checker for Rust, more on it later.

This setup is incredibly efficient and allows me to stay focused in one
environment.

---

## Additionals:

Here are a few more tools that I use to round out my terminal experience. Let's speedrun this section actually:

### broot - File Explorer
`broot` , which I talked about earlier, is a modern file explorer for the terminal. It gives you a tree
view of your files and allows you to fuzzy search and navigate your filesystem with ease.

### zoxide - A Smarter cd command
`zoxide` is a replacement for the `cd` command. It remembers which
directories you use most frequently, so you can jump to them with just a
few keystrokes. For example, `z p` might take me to my "projects" directory,
no matter where I am in the filesystem.

### lsd - My ls replacement
`lsd` is a modern replacement for the `ls` command. It adds colors, icons,
and a much more readable layout to the file listing. It's a small thing, but
it makes a big difference in the day-to-day usability of the terminal.

### bat - A better cat
`bat` is a `cat` clone with syntax highlighting and Git integration. When you
use `bat` to view a file, it automatically detects the language and highlights
the syntax, making it much easier to read code.

### bacon - Rust Auto-Builder
`bacon` is a background Rust code checker. It watches your files for changes
and runs `cargo check` in the background, so you get instant feedback on
your code without having to manually run the command. It's a great tool
for catching errors early.

### btop - Terminal Task Manager
It's pretty crazy what you can achieve in terms of functionality in a terminal.
`btop` is my terminal based task manager that is faster, battery-efficient and
often times more clear and informative than my actual Windows task manager for
certain things.

### Lastly, tock - A Terminal Clock 
`tock` is a simple clock that I built myself for the terminal using pure Rust.
It's built using the TUI framework Ratatui. I keep my taskbar on Windows hidden and
I just was tired of having to hover over my taskbar everytime I needed to look at
the time. Quickhacks!

---

## CONCLUSION:
Of course, it wasn't easy switching to the terminal for most of my workflows, but
I am so glad I made the decision because it has improved my workflow and made
it much more faster and efficient. I learnt a lot of new things, like using my
first modal editor, creating apps for the terminal, and optimizing my workflow,
and I hope you too learned a thing or two about the potential of your terminal.

## SELF-PLUG
If you are interested in more Terminal based projects, check out the top link
in the description, where I used Rust to create my own Terminal UI app to
manage my Wifi connections, called sigui. 

I've provided the download links for all the tools I talked about in the
description. It will also be available in the script of this video in the form
of an open-source markdown document on my Github. Link in the description.
 
Thank you for watching.
