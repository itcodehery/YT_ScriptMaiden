# Vim and Monkeytype

--
## TITLE

## The Bottleneck in your Workflow That Isn't Your Brain
### Description: The Hidden Productivity Skill Nobody Talks About - Exploring Vim and Monkeytype

Hi guys, my name is Hari and this is Project Directory, where we'll be focusing on
technical videos related to projects that I and the OSS community have built.

Recently it has become a trend among my classmates to quickly just whip up a typing test
tool like Monkeytype and check how fast they can type under a fixed time. One thing I've observed
is that obviously they're not really thinking about individual keystrokes when they do that. 
It's more about that flow state, and when they mess up, it breaks their rhythm completely.
So, the best scores come when they're not consciously thinking about typing. 

But why does this matter, right? Why are we even doing this? As a means to pass time or for 
something more? In the case of my peers, honestly I can argue for both sides. It gets boring
sitting with next to nothing to do in class, especially during extra classes where the 
lecturer let's you free for most of the classes ; but here comes the question: is it better as
a programmer to type quickly to write code?


## Cognitive Bandwidth

The crux of today's video is just me trying to hypothesize that typing does make you a better programmer, and 
the evidence that I found seems to support my hypothesis but not in the way you might think. We will 
learn more about that later in the video.

According to an insight I found, typing isn't a bottleneck at all actually. Programmers rarely 
type more than a few characters before pausing to think, often getting only 10 keystrokes out
before having to stop and make corrections. So if typing isn't a bottleneck, what is?

Imagine you are in the middle of writing a 200 line function for a server that serves a specific
part of the database to the authorized user. You are locked in right now and you are in 
that flow state thinking about how you need to implement this considering a thousand other factors
all running in your head. You write a line and remember that this function has to do this
particular thing before moving to the main logical part of the function. 

You gather yourself, move your hand to your mouse to scroll all the way to the start of the function 
and find the exact line where you have to add this supposed implementation of the idea you just had 
in your mind. You find the line and add the remaining lines and once you're done with that process
which probably took 30 seconds to a minute for you to do, you come back to the line where you
were already writing the rest of the function in a rhythm. The flow is now broken and you have to spend
that time to gather back your thoughts you had on this part of the code. It has simply been too long
to add those lines and get back to what you were doing earlier.

Think of it this way - Your brain is like RAM - it has limited working memory. When you're coding, you're 
fetching from your ROM:
- The problem that you are solving
- The architecture in your head
- Dependencies between components
- Variable names, function signatures

If you also have to think about how to type, that's one more process competing for resources. 
It's almost like you keep what you need right now to work in your personal cache, and that 
disruption sets you back to having to reload from the 'hard drive'.
It's not about speed—it's about removing friction.

## Documentation/Comments Problem
Writing (including code) is thinking. When there's friction between thought and expression, 
you lose thoughts. They're fragile. How many times have you had a brilliant idea, 
started implementing it, got stuck on some syntax error, and by the time you 
fixed it... the original insight was gone?
If you can type faster, then you operate closer to the speed of thought, it's not 
just about working faster, it's about being able to clearly express what is in your head.

Here's one more argument: improving typing speed increases chances of writing more detailed documentation and code comments.
The psychology being if typing feels like work, you'll naturally write:

- Shorter variable names (d instead of deserializedData)
- Fewer comments
- Sparse documentation
- Code that Future You will hate

When typing has friction, you unconsciously optimize for less typing. It's not a conscious 
choice—it's your brain naturally taking the path of least resistance.

## The Vim Philosophy
Okay, let's talk about Vim for a second.
Here comes Vim and says that if you think quick, you deserve to have a text editor that moves at your 
speed instead of limiting you and causing that friction. 
Vim is a modal text editor, which means it has different modes logically so that every action you could 
take in your text file can be done with just your keyboard. We mainly have four modes:
- Normal Mode: Which is the default mode that has your navigation and meta key binds like Undo, Cut, Copy and Paste.
- Insert Mode: Which is the mode we enter using either i or e from normal mode which let's us edit our file.
- Visual Mode: Which can again be accessed from the Normal mode and it lets us select text across the whole file.
- Command Mode: Which can be accessed using the colon (':') and it let's you execute vim commands like edit and quit.

You can go back to Normal mode 
Vim asks you to think of text as something that can be navigated spatially, like a map, with text 
broken into logical chunks like words, sentences, paragraphs, and blocks. 

It is built with the purpose of minimizing keystrokes, because as a power user you spend most time reading, not writing.
Let's see how to navigate through your file at maximum speed using Vim.

- Within a line, w and b leap forward and backward by words, while e lands on word endings. 
- gg lets you go to the start of the file.
- We can scale up to sentences with ) and (, or entire paragraphs with } and {. 
- For code, % bounces between matching brackets, letting you navigate function bodies and conditional blocks instantly. 
- Visual mode (v) lets you select these chunks: V enters Visual-Line mode which allows you to select lines, 
    vip grabs an entire paragraph, vi{ selects everything inside braces, va" captures a quoted string including the 
    quotes themselves. 

You stop thinking "move cursor, delete character by character" and start thinking "delete this logical unit." 
Your mental model shifts from character-by-character trudging to flying between landmarks. The file becomes a map where you know 
exactly how to get from point A to point B, and text transforms from a linear stream into a structured space you can manipulate 
with surgical precision.

Once you can type without thinking, you can start thinking in text operations rather than 
character-by-character editing. Vim's modal editing fits perfectly here because:

- You're not constantly typing
- You're thinking in normal mode, then executing in bursts
- The commands become a language for expressing transformations

## The Thinking Interruption Problem 
Okay. I spent the whole video arguing that typing fluency removes cognitive friction so you can think clearly.
Now we need to address the elephant in the room: AI and Autocomplete. Even traditional autocomplete in some
sense is very useful when it comes to filling out and completing what you started thinking. 
AI autocomplete completes what it thinks you should think next. It is like having someone whisper suggestions 
in your ear while you're trying to compose a sentence... which is again, pretty nice right?, even if the 
suggestions are a little interruptive. That is until you realize that AI autocomplete adds a different 
kind of cognitive load:

```dont include while speaking 
- Evaluation overhead: Every suggestion requires a micro-decision: "Is this what I wanted? Close enough? 
                    Should I accept it or keep typing?"
- Anchor bias: Seeing a suggestion shapes your thinking. You might accept "good enough" code instead 
                    of the elegant solution you were about to discover
- Context switching: Your train of thought gets interrupted by evaluating someone else's (the AI's) 
                    train of thought
```
- Evaluation overhead: Every AI suggestion forces you to pause and decide whether to accept it, reject 
                    it, or modify it—a constant stream of micro-decisions that fragments your attention.
- Anchor bias: Seeing the AI's suggestion first shapes your thinking, making you more likely to 
                    accept "good enough" code instead of discovering the better solution you were about to write.
- Context switching: AI autocomplete interrupts your flow by forcing you to evaluate its train of 
                    thought instead of staying immersed in your own.

One developer described it perfectly: when Copilot suggests something, you're no longer writing 
code—you're reviewing code. That's a completely different mental model.

Obviously just like I don't want to type System.out.println character by character, maybe I don't 
want to think through standard error handling for the 500th time. AI autocomplete could be for 
thought patterns what regular autocomplete is for typing patterns.

For Beginners, AI autocomplete can be a crutch that prevents learning. They never develop those 
mental models because the AI always fills in the gaps.

Experts might use it like a faster form of copy-paste from Stack Overflow—they know what they 
want, AI just saves the mechanical work.

When YOU type code, you're thinking through the problem.
When AI generates code, you're thinking through whether the AI thought correctly.
These are fundamentally different cognitive processes.

The first is synthesis. The second is evaluation. Both are valuable, but if you spend all your 
time evaluating AI suggestions, when do you practice synthesis?

## Conclusion
When there's no friction between thought and text, writing code stops being "I know what I want, now I 
need to type it out" and becomes "I'm figuring out what I want by writing it."
The act of editing is the act of thinking. 

And the wildest part? None of this is about speed. You're not typing faster to save time on some 
hypothetical project timeline. You're removing friction to think more clearly. The productivity boost 
isn't "I finished in 8 hours instead of 10"—it's "I solved a better problem" or "I wrote more 
maintainable code" or "I actually documented the weird parts."

Monkeytype is great because it teaches you that fluency. Vim, on the other hand, teaches you to be expressive. 
Together, they remove the friction between your thoughts and the screen. Not because you're typing faster, 
but because you're thinking in text operations rather than keystrokes and mouse actions.

## SELF-PLUG
If you are interested in more content related to typing and vim, check out the top link
in the description, where I have provided links for getting started with Vim and Neovim, and 
a formal guide to get better at typing faster.

They will also be available in the script of this video, which is in the form
of an open-source markdown document on my Github. Link in the description.
 
Thank you for watching.
