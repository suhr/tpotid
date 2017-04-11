Title: This Period of Time in Dieseq #0

I'm starting to blog the development of Dieseq, the microtonal (both [12ET](https://en.wikipedia.org/wiki/Equal_temperament) and 31ET) sequencer/daw, and this is the first post.

The series is called “This period of time in Dieseq” instead of “This week in Dieseq”, because I prefer to have something to talk about, so I will post when something is done instead of posting each week.

## What and why

Dieseq is a microtonal ([tricesimoprimal](https://en.wikipedia.org/wiki/31_equal_temperament)) sequencer/daw with an accent on composing. The name “dieseq” is a portmanteau of words *dies*is (a [microtonal interval](https://en.wikipedia.org/wiki/Diesis) about 40 cents wide) and *seq*uencer.

That's basically all. To tell more, I should answer *why* instead of *what.* So: why a yet another sequencer/daw?

Well, there're actually two reasons:

1. Traditional pianoroll is awkward to use when you have 31 notes per octave
2. Most DAW are very unfriendly to a composer

The first problem is easy to illustrate. This is how quatertone music looks in a typical DAW (the picture is not mine):

![](http://sevish.com/wp-content/uploads/2016/03/scumbag-piano-roll-quartertones.jpg)

Not only 7-5 color pattern mismatches the tuning, but notes are also very spaced. Figuring out what intervals are used is hard, and a complex piece won't even fit in the screen.

With 31ET it becomes only worse.

To understand the second problem, consider an analogy. Suppose you want to write some kind of acticle. Or maybe a story or even a book. To do this, you can use something like this:

![](https://hsto.org/getpro/habr/post_images/069/ca5/2a7/069ca52a7d19440c7a7e68c05f009d3b.gif)

But maybe you want something a little bit less compicated. Then you can use something like this:

![](https://i.imgur.com/vEdmQ0S.png)

A huge difference, as you can see. While [Ghostwriter](https://wereturtle.github.io/ghostwriter/) (a great piece of software I'm using right now) removes any distraction so you focus on the text, something like on the first picture only distracts you out.

This is a great example of the fact that UI *matters.* Together with [Brian Eno music](https://www.youtube.com/watch?v=HBoWf3t9FsE) Ghostwriter makes even my attention deficit mind to calm, focus and do the job.

So, I want to compose music. The problem is, most DAWs with their dozens controls are about everything but composing. The main window usually does not even have a piano roll, instead it shows tracks and mixer. But clip manipulation is surely not the first thing I want to do, nor is mixing or mastering.

Instead, I want just to tweak the synth and then compose. Simple as that.

There's some software that seems to follow this idea. The first is not an app, but a website: [beepbox.co](http://beepbox.co/). This litte tool for creating chiptunes is a great example how simple can everything be: there're notes on the top, 4 tracks on the bottom, and a few controlls on the right. And that's all you need, everything else is up to your creativity. This tool is a source of inspiration for Dieseq, it made me believe that simplicity is possible.

The second example is a tracker. But unlike other trackers, it combines flexibility with simplicity. And on mobile devices, it is actually *the* tracker. This tracker is called Sunvox:

<iframe width="854" height="480" src="https://www.youtube.com/embed/Pi8O1K5wCY0" frameborder="0" allowfullscreen></iframe>

Sunvox is a very cool tracker, and I want to borrow a lot of ideas from it. Except the pattern editor, I'm a visual thinker and Dieseq is a PC application.

The last two tools worth mention are [Radium](http://users.notam02.no/~kjetism/radium/) and [Odesi](http://odesi.mixedinkey.com/detailedguide/). Radium is a music editor highly influenced by trackers (probably Sunvox too), and Odesi is a program designed for composing. Maybe I'll borrow from something from these programs too.

## Ideas

Dieseq has a piano roll on the top, timeline with tracks on the bottom, and routing frame (like in Sunvox) on the right. Separation between frames is freely movable, like in Sunvox.

Great! Now we only have to make the piano roll compact and easy to use.

### Making piano roll smaller

The main idea is to make a piano roll step smaller than note bars height, so notes can be “in between”. Like this:

![](https://i.imgur.com/gAB1umy.png)

This way one octave will take about 16 rows. It is more than usual 12 rows, but still pretty reasonable.

But there's a little problem that everything a bit more complicated: 31 is a prime number. In particular, it is an odd number. This means that if some note is in a row, a note that is an octave higher is between the rows. So equivalent notes doesn't look the same anymore.

The solution of this problem is simple. Like in Go or [Xiangqi](https://en.wikipedia.org/wiki/Xiangqi), Dieseq has no rows. Instead, notes are placed on lines, like this: 

![](https://i.imgur.com/edWjkO6.png)

(sorry for a awful picture made by using a mouse in Gimp)

Octave lines can be a little thicker.

### Recognizing intervals easier

How to show that some interval is a [harmonic seventh](https://en.wikipedia.org/wiki/Harmonic_seventh)? Well, the most straightforward way is this:

![](https://i.imgur.com/72cdLf4.png)

And that's basically is Dieseq's “show intervals”. For a harmonic seventh chord, it will show 5:4, 6:5 and 7:6.

An another way to help you recognize intervals visually is basically a more advanced version of picrelated:

![](http://davidkulma.com/wp-content/uploads/2016/08/Integer-Circle.001.png)

Imagine some kind clock with two arrows, that move as you move the note pitch. The first arrow has five postions on the clocks, moves to a next position each diesis and makes a cycle after five diesis (a whole tone). The second arrow has six positions and moves to a next position each whole tone.

For example, a perfect fifth is four whole tones plus two diesis.

Because 31 is a prime, the number of positions the clock has (30) does not match the number of intervals. As a result, equivalent notes look different, a semi-diminished octave looks like a prime and an octave looks like a diesis.

But when you know in which half-octaves notes are, figuring out the correct interval is easy.

The idea is to draw the pitch arrows on each note bar, so two ways to represent an interval will cooperate. A fourth and an undecimal tritone look very similar on the piano roll, but the pitch clock makes the difference visible, a diesis and an octave are the same on the clock but the piano roll makes the difference obvious.

### Other ideas

* It would be nice to have a fullscreen mode there only thing you have is a piano roll
* It would be nice to be able to edit several tracks in the same time while using the piano roll
* Ghostwrites has the “focus” mode when everything but the paragraph you're editing is shadowed. Dieseq should have something like that too
* How often you type into a piano roll (hint: you probably don't)? Let's have vim-style shortcuts, where help is just `h` instead of `Ctrl-H`

## The Plan

Start from a least useful implementation: a plain piano roll without anything (not even undo/redo) that can play MIDI via PortMidi. Then incrementally add functionality.

You probably are asking right now: Where're screenshots? Why I'm drawing images in Gimp instead of showing the program? Where's the repository with source code?

Well... there's no code yet. I ran into a little problem soon after starting the development.

## Graphics programming is harder than theoretical physics

Real world is incredibly complex. But in physics, you use *math* to build simple and elegant models.

Things you want to do using a computer are often so simple even a kid can describe them. But software developers use *programming* to make everything complicated and opaque.

Ok, I know this is unfair. But in physics you can find a lot of beautiful math with books and papers explaining this math. While in programmming you have dozens of amorphous and mysterious constructs with often no documentation at all. Physics is complex, but physicists try to tame the complexity, programming is complex, and that complexity is untamed.

Long story short, I tried to implement Dieseq and failed.

At first, I tried to just use Conrod, the GUI library written in pure Rust. Unfortunatelly, it finds out, conrod is so complicated so even scrolling is a non-trivial problem. Even after [asking the author](https://github.com/PistonDevelopers/conrod/issues/956) on Github, scrolling is a pure mystery for me.

So I thought: “Complex solutions are often a result of solving a wrong problem. Maybe I should just take a graphics library and figure out how to use it in my project”. I took a look at gfx-rs and glium, and... well. It was quite naïve to think they're any simplier (especially gfx-rs). They are complex in their own ways.

Don't get me wrong, I *approximately* understand how the example with a triangle works. But I don't understand how it works inside, nor I see how use in a complex project. I have absolutely no eagle vision over the project, all I can do is to just monkey around with triangle example code.

## Help needed

![](https://i.imgur.com/KAJa1W6.jpg)

Programming is well-known to be hard. But different people have difficulties with different aspects of programming. For most people, programming is hard in a wrong way: they struggle with trivial things like pointers, recursion, types, monads or borrow checker, but then they implement compilers, web engines, OSes, data bases, emulators, quines like it's nothing.

For me, it's opposite. I learn various concepts quickly, but I'm not so good with dealing with an untamed complexity.

I have no skill and I must code. But I stuck and I need help to make sense of all that graphical mess. I hope this post will inspire you at least a bit.

I have a lot of things to learn.
