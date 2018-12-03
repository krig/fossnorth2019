> When I wrote the following pages, or rather the bulk of them, I
> lived alone, in the woods, a mile from any neighbor, in a house
> which I had built myself, on the shore of Walden Pond, in Concord,
> Massachusetts, and earned my living by the labor of my hands only. I
> lived there two years and two months. At present I am a sojourner in
> civilized life again.

---

For those who don't know, the above quote is the opening line to
Walden by Henry David Thoreau, one of my favorite books of all
time. I mean, not just for the underlying message but also for the
writing which is amazing. I really like the last line: At present, I'm
a sojourner in civilized life again. It's like yeah, I'm wearing
pants -- temporarily.

When I started thinking about doing this talk, I really only had the
project itself in mind. Yeah, I have this thing I want to do, and once
I've done it I'll talk about it. Maybe even reproduce some of it live,
it'll be great.

Once I actually sat down to start writing the talk, though - after
actually finishing the project I had set out to do for the
presentation, which surprised and amazed me - I felt very defensive
about it. Like, the main thing I felt I'd have to do was defending the
very notion of doing something like this. Why would you even?

So I'll try to explain why I am standing here talking about Lisp from
1959 by way of Walden.

First of all, to me Walden is about applying the scientific method to
ones own life. What do I actually need, and what would make my life
meaningful? Instead of just going through the motions following the
path staked out by society, begin thinking about your own life from
first principles. What do I need to survive? Well, food and shelter,
right? So what Thoreau does is starting with nothing but an axe,
he builds his own house, plants his own food and goes from there.

Well, that's not exactly true. Actually, it turns out that the land
that he built his house on belongs to his friend Emerson, and every
week he'd go home to mommy to get his clothes washed and presumably
get fed proper food. So don't get too enamoured with this whole notion
of starting from zero. But to me the fact that actually doing that
turns out to be unrealistic, the deeper truth of it appeals to me.

One thing you hear a lot today is the idea of minimalism, like oh, I
only have 50 things in total and it's great. I guess this is kind of
similar to that but I feel like minimalism kind of misses the point,
or at least the point that I want to make. If that works for you,
that's great. The idea that I am getting at is not just to live like
an ascetic and deny yourself all the pleasures in life. The idea is to
really look at your life and strip away everything that doesn't matter
to you personally, and to really understand the things that do matter.

So, my big passion in life is programming. I love it. I mean, it's
definitely not just a job for me, I'm just incredibly thankful that I
get to program all day and someone gives me money for it, it's
amazing. I mean, imagine if your hobby was drinking beer and someone
came along and said, oh you like beer, I'll pay you to drink beer all
day. Well, that'd probably be bad in the long run, but you know what I
mean. I do have other interests, don't get me wrong, but programming
is the thing that I really connect with deeply. And so what I want to
do and to share with you is the notion of stripping programming down
to its essentials, to, equipped with nothing but an axe - C in this
case, build something that is _essential_ in the true meaning of the
word. Something that has the bits that have to be there, ideally all
of them, and none of the bits that don't have to be.

Of course I'm not there yet. The journey is the goal here. I think
maybe that's another thing that people maybe don't get about Walden,
incidentally. I don't think the idea is to actually live like he did
permanently or to flagellate yourself. But having gone through that
process of focusing on the essentials, he now knows himself a lot
better.

---

> Yes, that was the big revelation to me when I was in graduate
> school—when I finally understood that the half page of code on the
> bottom of page 13 of the Lisp 1.5 manual was Lisp in itself. These
> were “Maxwell’s Equations of Software!” This is the whole world of
> programming in a few lines that I can put my hand over.
- Alan Kay

So 1959 is not the origin story of programming, and although this is
going to be a history lesson in some sense it's not a trip back to the
earliest program in history. Instead, it's about the fundamentals of
programming in an abstract sense. 

This is a quote by Alan Kay, famous inventor of Smalltalk and legend
from the early days of graphical user interfaces at Xerox Parc. He is
also great at telling everyone today that they are doing everything
wrong, but he's a lovely guy so that's OK.

What he is talking about is the manual to Lisp 1.5, which at least as
far as I can tell seems to be the first proper production version of
Lisp, released in 1962. You can hopefully see the passage that he's
talking about in here, I don't know if it's readable.

But what this describes is the evaluation function of Lisp, written in
Lisp. Basically defining the core semantics of the language in the
language itself. It's fantastic. By the way, it doesn't look like lisp
because it's written in something called M-expressions. John McCarthy,
the inventor of Lisp and one of the authors of this and the earlier
lisp paper from 1959, didn't actually like S-expressions, the lisp
syntax we know today, very much, and he considered it an intermediate
format. He thought that people would write their code in M-expressions
and the compiler would turn that into S-expressions. Turns out no one
ever bothered to write the M-expression frontend, and the S-expression
syntax turned out to have benefits of its own, but hopefully it's
fairly readable.

So this function evalquote came from McCarthys earlier paper about
Lisp, called

> Recursive Functions of Symbolic Expressions  and Their Computation
> by Machine, Part I

Part I, although there was never any part II.

So this is the paper that is the origin of the project that I'm going
to show you. It really is a great paper. It's full of wonderful
statement like this one,

> [..] whereby a machine could be instructed to handle declarative as
> well as imperative sentences and could exhibit “common sense” in
> carrying out its instructions.

What McCarthy and the others at the MIT AI lab at that time were
trying to do wasn't just coming up with a new programming
language. They were trying to build machines that think. And although
to modern eyes they weren't really all that close (I don't think we're
much closer today, to be honest), the early papers all have this
wonderful flavor of optimism, talking about computers with common
sense and reasoning as if it's just around the corner.

If nothing else, I hope this talk gets you interested in reading old
papers.

What I've done is write a lisp interpreter, in C. And in the language
that interpreter interprets, I have written a small implementation of
the LISP 1.5 eval function, so that it can run basic LISP 1.5
programs. I've also implemented a garbage collector for the
interpreter. And the whole thing is about 550 lines of C.

However, it's not exactly the language described in the recursive
functions paper, and it's definitely not all of LISP 1.5. It's a
tribute. I originally posted an early version of this when John
McCarthy passed away, back in 2011. I had recently read the paper and
started going down the rabbit hole which is early lisp, so when that
happened it seemed like a nice gesture.

It's on github at this address if you want to take a look. It's a
single C file, a Makefile and some tests to run in the interpreter.

The core of the implementation are these three files. Let's dig in.



