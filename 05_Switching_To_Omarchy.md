# Switching to Omarchy

---
## TITLE

## Why I Finally Gave Up on Windows | Switching to Omarchy

Hi guys, my name is Hari and this is Project Directory, where we'll be focusing on technical videos related to projects that I and the OSS community have built.

Recently Windows has been on a downward spiral because of Microsoft's ignorance and misplaced trust on the push towards AI. For the average person, they probably don't realise how misplaced our trust is on Windows because that's all we've ever known, but all Microsoft has been doing for the past few months is quietly turning our operating system into something else.

We need to have a quick talk about this, because this video is not just about my transition to Linux, Microsoft used to be a company that cared about its consumers, it used to be a company that its consumers respected, but hearing about its recent decisions these few months it just feels like the company has completely given up on giving people what they desire and replaced it with this life-less corporate greed, this constant focus on revenue generation more than being people-centric.

With Windows, especially, Microsoft has been focusing on the wrong things. Every single core feature of Windows has been experiencing problems, and Microsoft themselves have acknowledged this. Even recent quality-of-life updates carry with them unknown bugs affecting things like the Start Menu, Taskbar, File Explorer, and the System Settings.

Worst part is that Windows isn't free. Sure it might look like its free but Microsoft takes its cut for Windows for every pre-built PCs and Laptops. For people who want to build their own PCs, the Basic Home version of Windows costs around $139, and yet it comes riddled with advertisements and pre-installed bloatware like Candy Crush, and forced Microsoft apps that you can't even uninstall on your own. Many people don't even realise that bloat like this slows their PC down and affects its performance so they keep it installed.

And then we have the "Agentic OS" Direction that Microsoft has chosen for Windows. The newest feature of Windows lets AI make decisions on users' behalf, because what even is the point of using our computer if we don't let Microsoft know what we're doing with it, right? This among the growing privacy concerns and this forced AI integration is what pushed me to take a decision.

So here's what I did. I switched to Linux.

## Why Now

This change has been a long time coming. The word about Linux that I used to hear when I was in my college doing my Bachelors is that Linux is an operating system that was specifically used in Servers and not much in consumer computers, unless you were a Hacker or a Network Engineer. Again this assumption was based around the fact that none of my college mates or even Professors really used any form of Linux. It was always either Windows or MacOS, so we never really got a chance to think of Linux as a potential option to even consider. This was again backed by the fact that all of our workflow in college revolved around Microsoft tools, especially Office. 

Now, that I'm free from those barriers and open to not limiting myself to certain tools, I thought now would be the best time to switch to Linux, and that's what I did.

## Omarchy
Now what is Omarchy? Omarchy is an custom setup of this distro called Arch Linux and the Hyprland Tiling Manager created by David Heinemeier Hansson, or DHH, this amazing figure in Computer Science most famously known for creating the Ruby on Rails framework.

Usually when people recommend Linux for other people, it's one among these. This is to help people to easily transition to Linux from Windows so that they feel right at home. Distributions like Ubuntu and Fedora mimic the UI and experience of Windows to feed this feeling of familiarity within the users coming from Windows.

But nah, the UI of Windows looked boring anyway. I wanted something that looked and felt fresh and brand new, so the popular choice in that regard was to try Arch Linux, the holy grail of customizability among the Linux distros, but Arch is also a very lightweight and very do-it-yourself kind of distro. This means that we are responsible ourselves for installing simple things like a graphical user interface, a network driver and drivers for your own specific graphics card, and sometimes changing things might break something else. That level of control comes at this cost, but I didn't want to compromise on that.

A couple of months ago, I heard of this new distro made by apparently the creator of Ruby on Rails, it was called Omarchy and it was a pre-built distro based on Arch. I was just curiously watching a lot of videos on it and I decided that this would be the way to go.

## Why Omarchy

There are a few things that Omarchy has going for it - first being its built on Arch, so every new Arch version, I get automatic updates. The way I install stuff is the simplest I've seen in any Operating System -> I press Super+Alt+Space to open the Omarchy Menu, go to Install, it gives me this whole menu on what exactly I want to install. If I had to install Firefox for example, I would just open Packages and in this Terminal UI, I just type Firefox and press enter. That's it. It also doesn't use a regular Desktop environment like GNOME in Ubuntu and Fedora. Instead, it uses Hyprland, which is a Tiling Window Manager. For any window I open, it lets me do stuff like this...

<show Hyprland magic!>

So yes, it is a very developer focused Distro and it is also very opinionated, having key binds for Social Media, pre-installed Discord, ChatGPT and OBS, but all of these are highly customizable. In fact, the whole Operating System, down to how it looks and feels is all customizable, that's the best part. It's also got Themes!

<show themes!>

Of course, you can also create your own theme and Omarchy comes with its own Theme creation engine - Aether.

## Everything Just Works

The philosophy behind this distro is that it is a complete system that ships with everything a software dev needs to be productive immediately, from Neovim to Spotify, Chromium to LibreOffice. Everything is themed and integrated out of the box.

For Developers, most of the dev tools we already use like Git, Docker, Neovim, and terminals are already pre-installed. Installing any dev tool or any software for that matter is as easy as opening the Omarchy Menu, pressing Install and choosing what kind of software you need to install. For everyday application software, you can expect to find it either in the Package installer or the AUR which is the Arch User Repository. Omarchy also lets you create your own Web Apps to open sites like YouTube and Gemini from your App Launcher. It is also a very keyboard oriented distro similar to the workflow of Raycast, and it offers a much better multitasking experience because of an actual tiling window manager with Hyprland.

I was surprised to see that even Gaming, arguably one of the most notoriously difficult part about switching to another Operating System, just works. Valve has helped the Linux Community in making strides in Gaming compatibility between Windows and Linux, with its compatibility layer Proton built in by default in Steam on Linux. Valve's handheld console, the Steam Deck also works with Arch Linux as its Operating System. Worst case scenario, Omarchy also offers an easy way to run Windows through a Docker virtual machine and the option to do that is again built into the Omarchy Menu itself.

It also doesn't compromise on Security and Privacy. Full-disk encryption is mandatory in Omarchy when you set it up, that's the reason you really can't install it on a partition of an existing drive, you have to install it in its own drive. The Firewall is enabled by default. No more ads, no more shady background processes that clog up your Task Manager, no AI watching you. You are in control of your system completely.

## Installing Omarchy
Despite Arch Linux's reputation for being notoriously difficult to install, Omarchy's installation experience is comparable to any simple consumer distro like Ubuntu and Debian. First we download the ISO from the official Omarchy website - omarchy.org. Burn the ISO into any pen drive you have lying around using a tool like Balena Etcher or Rufus, I used Rufus. Boot into the Pen Drive in your PC and follow the installation script of Omarchy. It is very minimal in the initial setup, asking very few questions, and that's about it. Your part in the installation would be done in 30 seconds max, and within 10-20 minutes, your fresh installation of Omarchy would be complete, with everything set up and ready to go.

Omarchy, like I mentioned before, is a keyboard focused distro, and most of the keyboard shortcuts revolve around the Super key, which is our command center. The main shortcuts include Super + Space to open the App Launcher, Super + Alt + Space to open the Omarchy menu, Super + 1 2 3 and so on to navigate through workspaces. The unified theme system is accessible using Super + Ctrl + Shift + Space. Obviously you can't memorize all the keybinds in one go so Omarchy conveniently includes a Keybind menu which you can access using Super + K. It shows a searchable menu for all the Keybinds.

## Honest Considerations
After spending almost 15 years on Windows, the decision to switch to another operating system didn't just happen without sacrifices. For every single native software that I needed to use, I had to find an alternative built for Linux that again involved a learning curve like my Video Editor. I used to use Premiere Pro for that, the past few months however I have been trying out kdenlive and it has been fun. In fact, all of the videos on this channel have been edited on kdenlive. The hardest software that I thought I had to replace was my DAW, my music making and editing software, I used to use FL Studio and I then switched to Ableton, both of which are only available on Windows and MacOS. The solution to that turned out to be simple again, just run them on a VM running Windows. Lastly, Gaming which turned out alright because I mostly play just single-player games anyway, so Steam was more than sufficient. 

With all of these though, there were still things that people would consider to be drawbacks. A new Operating System is a commitment, especially with something like Omarchy and Arch Linux, the learning curve for the keyboard-first workflow is real. The tiling window manager is another thing that people might find to take some adjustment. Omarchy works on the assumes that its users are comfortable with keyboard-driven interfaces and command-line tools.

Despite all this, the Omarchy and Arch community is very helpful and active. Omarchy itself is regularly maintained and improved by DHH, and its Free and Open Source.

## Conclusion
At this point, we can all agree that Windows is only getting worse. The ads are not going away anytime soon, the AI integration is accelerating and core features keep breaking. As developers, we deserve better. Omarchy gives you a beautiful, fast, privacy-respecting system that's actually designed FOR developers, not advertisers.

If you are interested in switching from Windows, check out Omarchy.org. To learn how to use Omarchy, check out learn.omacom.io for a detailed guide on how to use Omarchy. To go for a more traditional transition, check out ubuntu.com and fedoraproject.org, all of these links are in the description. If you are already a Linux user, put a comment down below on your transition and your thoughts on Omarchy.

Thank you for watching, and stay curious!

## Resources
omarchy.org
learn.omacom.io (the manual)
GitHub: basecamp/omarchy
