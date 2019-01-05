# Let's LISP like it's 1959

This talk is going to be partly about history, partly about lisp in
general and partly about my tiny lisp implementation - I'll get to
that last.

## Background

Lisp was conceived in the late 1950's by John McCarthy who was one of
the founders of the AI research lab at MIT together with Marvin
Minsky, both with a background in mathematics.

McCarthy was also the person who came up with the term "artificial
intelligence", at least as far as I have been able to determine.

Reading the papers they wrote about AI in the 50s is a lot of
fun. They were absolutely convinced at that time that true human level
reasoning was just around the corner. I have one paper by McCarthy
from 1959 called "Programs with Common Sense" containing this mission
statement:

> Our ultimate objective is to make programs that learn from their
> experience as effectively as humans do.

I think there are some really valuable lessons to draw from the early
days of AI, not in the least how hard it is to know where you are when
you have very little experience to draw upon. Early on with some new
technology, you simply don't know enough to know how little you know.

Some years later, McCarthy published a paper with the following title:

> HUMAN-LEVEL AI IS HARDER THAN IT SEEMED IN 1955 

I have a feeling that it's harder than it seems in 2019 as well, but
that's a different story.

## Lisp

The idea for a list processing programming language originally came
from a language called IPL 2 created in 1956. The other main
inspiration for Lisp was FORTRAN, created in 1957, a huge innovation
at the time, being the first non-assembly language or higher level
language.

The computer used by McCarthy was an IBM 704, the first mass-produced
computer with floating-point arithmetic hardware.

https://en.wikipedia.org/wiki/File:IBM_Electronic_Data_Processing_Machine_-_GPN-2000-001881.jpg

I haven't found any video of an IBM 704 in action, but I have found
video of an IBM 1401 running a FORTRAN compiler to compile a small
program, and it's fascinating. I don't have time to play it now but
I've put the link in the slides.

https://www.youtube.com/watch?v=uFQ3sajIdaM

Here's a quote from McCarthy that's pretty great:

> Representing sentences by list structure seemed appropriate - it
> still is - and a list processing language also seemed appropriate
> for programming the operations involved in deduction - and still
> is.

The thing to note here though is that at the time, lisp as we know it
was not conceived as a programming language. Instead, it started as an
exercise to simplify the Turing machine. McCarthy started thinking
about compiling Lisp in 1958, that is turning lisp expressions into
machine instructions, and hired an undergraduate called Steve Russell
effectively as a human compiler. His task was to take snippets of Lisp
code and translate them into machine code.

After a couple of months, McCarthy came up with the eval function.

> Another way to show that Lisp was neater than Turing machines was to
> write a universal Lisp function and show that it is briefer and more
> comprehensible than the description of a universal Turing
> machine. This was the Lisp function eval..., which computes the
> value of a Lisp expression.... Writing eval required inventing a
> notation representing Lisp functions as Lisp data, and such a
> notation was devised for the purposes of the paper with no thought
> that it would be used to express Lisp programs in practice.

Steve Russell later went on to write Spacewar!, one of the earliest
examples of a video game, so he's a pretty cool dude. Also his
nickname is "Slug".

Anyway, Steve Russell realised that if he could implement eval and
then feed the code to that, he wouldn't have to keep hand-compiling
things. He presented the idea to McCarthy, and here is what McCarthy
said about that later in an interview:

> Steve Russell said, look, why don't I program this eval..., and I
> said to him, ho, ho, you're confusing theory with practice, this
> eval is intended for reading, not for computing. But he went ahead
> and did it. That is, he compiled the eval in my paper into [IBM] 704
> machine code, fixing bugs, and then advertised this as a Lisp
> interpreter, which it certainly was. So at that point Lisp had
> essentially the form that it has today....

So implementing eval in Lisp was not just the origins of interpreted
languages, but it was also a kind of universal definition of what a
programming language is, at its core. And that really fascinates
me. Here is a famous quote from Alan Kay, the inventor of Smalltalk
and object oriented programming:

> Yes, that was the big revelation to me when I was in graduate
> school—when I finally understood that the half page of code on the
> bottom of page 13 of the Lisp 1.5 manual was Lisp in itself. These
> were “Maxwell’s Equations of Software!” This is the whole world of
> programming in a few lines that I can put my hand over.
- Alan Kay

## S-expressions

Okay, so Lisp is not a programming language but more like a family of
languages or a mathematical notation for computation. But what does it
look like?

McCarthy had originally come up with a different syntax which was a
lot more like Fortran called M-expressions, and his idea was that
S-expressions were useful for talking about computation as a
mathematical phenomenon while M-expressions would be what programmers
actually used to write code.

The closest thing to an implementation of that was a language called
Dylan created by Apple in the 90s, and I'm pretty sure no one here has
heard of it. M-expressions never made it into the implementations but
a lot of the early documentation is written using M-expressions rather
than S-expressions (the parentheses style of lisp that we know
today).

Parentheses. Lots of parentheses. At least that's usually the initial
reaction that anyone has to seeing Lisp code. I'll admit that it was
my reaction too. The first thing to say about Lisp is that trying to
edit Lisp code without an editor which highlights matching parentheses
for you is a nightmare. Of all the things that impress me about
programming in the elder days, being able to write Lisp code without a
modern editor and getting the paretheses right is the biggest one.

Well, in fact it becomes clear that this WAS a big problem back
then. There is a great quote from the Lisp 1.5 manual instructing the
programmer to put a bunch of extra closing parentheses at the end of
the last punch card, just in case.

> To prevent reading from continuing indefinitely, each packet should
> end with STOP followed by a large number of right parentheses. An
> unpaired right parenthesis will cause a read error and terminate
> reading.
>
> STOP )))))))))))))))))

Alright, so enough history, let's get to something concrete. Here's a
speed intro to the world of Lisps.

> foo
> (a b c d)
> ()

In its most basic form, Lisp has two kinds of elements: Lists and
atoms. This is an atom called foo, and this is a list of four atoms, a
b c and d, and finally an empty list.

> ((a b c) (d e f))

This is a list containing two sub-lists: One containing the atoms a b
and c, and the other containing d e and f.

> (f x)

When representing function calls, the first element in the list is the
name of the function to be called, and the rest are arguments to the
call. So this is a call to the function f passing x as an argument.

> (lambda (x) (* x 2))

The world lambda comes from something called lambda calculus, which
was invented by Alonzo Church in the 1930s and which McCarthy thought
was a more convenient way of thinking about computation than the
Turing machine by Alan Turing. So when he designed lisp, he based it
on the lambda calculus and that's why functions are defined using the
word lambda or the greek letter lambda. So this is a function which
takes an argument x and multiplies it by two.

> ((lambda (x) (* x 2)) 4)

Here we are calling the function we just created with 4 as the
argument, so this should return 8. Things are already getting pretty
paretheses-y, I know. You do get used to it.

That is pretty much the entire syntax of lisp. Now we get to some of
the most primitive operations of lisp, and here different dialects
already start to diverge.

> #t
> #f
> (atom? x) => #t

In scheme, truth is represented by the atoms hash t and hash f, while
in Common Lisp anything that isn't the empty list is true while the
empty list is considered false. Here I'm showing the scheme version.

> (quote a) => a
> (quote (a b c)) => (a b c)
> '(a b c) => (a b c)

In order to be able to write something without evaluating it, like a
list that shouldn't be read as a function call, we use something
called `quote`, which doesn't evaluate its argument and just returns
it. More modern lisps allow you to write a single quote in front of
something and converts that into a call to `quote`.

One thing to notice here is that lisp doesn't differentiate between
data and code. Functions are just lists, and can be created and passed
around just like everything else. There are no statements, everything
is an expression.

> (cons x '(y)) => (x y)
> (car (cons x y)) => x
> (cdr (cons x y)) => y

Here are some basic operations for manipulating lists. Lists are
constructed as pairs, with the first element of the pair being the
thing held by this node in the list, and the second element in the
pair pointing to the next pair, or pointing to nil if this is the last
element in the list. `cons` appends a new element to the front of a
list, and `car` and `cdr` extracts the elements of a list.

Just to make this clearer, here are some boxes with arrows sticking
out of them.

***

Okay, now we start getting into more juicy stuff.

> (cond ((atom? (quote (a b c))) 10) (#t 20)) => 20

`cond` is the basic conditional expression. In fact, lisp was the
first language to have a conditional expression, so this was quite an
innovation. `cond` takes a series of pairs and iterates over the pairs
one by one, evaluating the first element in the pair and if it's true,
evaluating the second element. If false, it moves on to the next pair.

> (equal a b)

`equal` returns true if a and b are the same, meaning the same atom or
both are the empty list. We can define a more complicated equality
function later which would compare lists element by element, but the
most basic comparison just decides that lists with elements in them
are always unique and different from each other.

> (lambda (x) (* x x))

We've seen lambda before, which lets us define functions.

> (define square (lambda (x) (* x x )))

Using `define`, we can give a name to our function.

## Lisp today

Today, there are a few different dialects of lisp in use. Scheme, most
commonly in the free software world known via GNU Guile, is the oldest
of these, followed by Common Lisp which was intended to be *the*
standard programming language but never really caught on. Next comes
Emacs lisp as used in the emacs text editor, and finally we have
Clojure and ClojureScript which are quite a bit newer than the others
and runs on the JVM and in the browser.

I think Lisp is great, but I'm not a huge fan of any particular
implementation. The closest thing to a lisp that I would use would
probably be Guile, which is a scheme implementation used for example
in Guix, a functional package manager based on Nix.

MORE GUIX HERE

## komplott

OK, so now that we know what Lisp is and what to do with it, lets make
one ourselves.

The most common demonstration of implementing lisp is to implement
lisp in lisp. This is a neat trick and it's what McCarthy did in one
page back in 1959, and what Alan Kay called Maxwell's Equations of
software. It's not all that enlightening though, at least it wasn't
for me. So a slightly different trick is to implement lisp in C, so
that someone coming from a "normal" programming language can get a
sense of what's going on.

This is something that I've done a few times and that I would
recommend everyone doing as a fun exercise if nothing else. The one
I'm going to show you here is at 560 lines of code and include a
garbage collector. Hopefully I'll have time to get into that.

















# Timeline

Why is lisp so fascinating? I think the reason is that Lisp is not a
programming language at all. It is a notation for talking about
computation with other humans more than it is a language for telling
computers what to do. That it can also be understood by computers is a
nothing more than a side-effect and thus nothing that we truly
functional programmers worry much about.

## Beginnings in AI

> HUMAN-LEVEL AI IS HARDER THAN IT SEEMED IN 1955 

Headline of document written by McCarthy in 2006.

> If my 1955 hopes had been realized, human-level AI would have been
> achieved before many (most?) of you were born.
http://www-formal.stanford.edu/jmc/slides/wrong/wrong-sli/wrong-sli.html

> He believed that we would have to know much more about how human
> intelligence works before being able to duplicate it in machines,
> writing that “[unfortunately we] understand human mental processes
> only slightly better than a fish understands swimming.”
http://ai.stanford.edu/~nilsson/John_McCarthy.pdf


Historically, lisp IS a theoretical exercise:

> Another way to show that Lisp was neater than Turing machines was to
> write a universal Lisp function and show that it is briefer and more
> comprehensible than the description of a universal Turing
> machine. This was the Lisp function eval..., which computes the
> value of a Lisp expression.... Writing eval required inventing a
> notation representing Lisp functions as Lisp data, and such a
> notation was devised for the purposes of the paper with no thought
> that it would be used to express Lisp programs in practice.

John McCarthy started thinking about compiling Lisp in 1958, and hired
Steve "Slug" Russell effectively as the compiler: His first task was
to hand-compile Lisp code. But after a couple of months, McCarthy
came up with the eval function.

Steve Russell then realised that instead of compiling the function he
could implement eval directly and then feed the code to that, and so
the first lisp interpreter was born.

This was a big surprise at the time. Here is what McCarthy said about
it later in an interview:

> Steve Russell said, look, why don't I program this eval..., and I
> said to him, ho, ho, you're confusing theory with practice, this
> eval is intended for reading, not for computing. But he went ahead
> and did it. That is, he compiled the eval in my paper into [IBM] 704
> machine code, fixing bugs, and then advertised this as a Lisp
> interpreter, which it certainly was. So at that point Lisp had
> essentially the form that it has today....


More Steve Russell quotes:

> I wrote the first implemenation of a LISP interpreter on the IBM 704
> at MIT in early in 1959. I hand-compiled John McCarthy's "Universal
> LISP Function".

CAR/CDR:

> Because of an unfortunate temporary lapse of inspiration, we
> couldn't think of any other names for the 2 pointers in a list node
> than "address" and "decrement", so we called the functions CAR for
> "Contents of Address of Register" and CDR for "Contents of Decrement
> of Register".
>
> After several months and giving a few classes in LISP, we realized
> that "first" and "rest" were better names, and we (John McCarthy, I
> and some of the rest of the AI Project) tried to get people to use
> them instead.
>
> Alas, it was too late! We couldn't make it stick at all. So we have
> CAR and CDR.

http://www.iwriteiam.nl/HaCAR_CDR.html

# First garbage collector

> At any given time only a part of the memory reserved for list
> structures will actually be in use for storing S-expressions. The
> remaining registers (in our system the number, initially, is
> approximately 15,000) are arranged in a single list called the
> free-storage list. A certain register, FREE, in the program contains
> the location of the first register in this list. When a word is
> required to form some additional list structure, the first word on
> the free-storage list is taken and the number in register FREE is
> changed to become the location of the second word on the
> free-storage list. No provision need be made for the user to program
> the return of registers to the free-storage list.

http://blog.fogus.me/2011/11/03/in-the-shadow-of-john-mccarthy/


David Luckham:

> McCarthy was a strange fellow with many odd attitudes in human
> relationships, one being that he would never lift a finger to help
> any of his own people. As a result he continuously lost long serving
> staff such as Steve Russell, Dan Edwards, and others. Those were
> days when McCarthy was championing the idea of large central
> computers serving hundreds of users. Even in 1969 the AI lab had a
> direct news feed from the New York Times. And on-line text chatting
> between users, with video support was possible. Thus the lab
> supported the kind of Internet features we are all accustomed to
> today.
>
> In the early 1970’s McCarthy seemed to become disinterested in the
> projects of his staff, and to retire to his office and read the
> newspapers on-line.

Not only did they invent the internet, they also very quickly invented
wasting time on the internet as well.


# APPENDIX - HUMOROUS ANECDOTE

The first on-line demonstration of LISP was also the first of a precursor of time-sharing that we called ``time-stealing''. The audience comprised the participants in one of M.I.T.'s Industrial Liaison Symposia on whom it was important to make a good impression. A Flexowriter had been connected to the IBM 704 and the operating system modified so that it collected characters from the Flexowriter in a buffer when their presence was signalled by an interrupt. Whenever a carriage return occurred, the line was given to LISP for processing. The demonstration depended on the fact that the memory of the computer had just been increased from 8192 words to 32768 words so that batches could be collected that presumed only a small memory.

The demonstration was also one of the first to use closed circuit TV in order to spare the spectators the museum feet consequent on crowding around a terminal waiting for something to happen. Thus they were on the fourth floor, and I was in the first floor computer room exercising LISP and speaking into a microphone. The problem chosen was to determine whether a first order differential equation of the form was exact by testing whether , which also involved some primitive algebraic simplification.

Everything was going well, if slowly, when suddenly the Flexowriter began to type (at ten characters per second)

``THE GARBAGE COLLECTOR HAS BEEN CALLED. SOME INTERESTING STATISTICS ARE AS FOLLOWS:''

and on and on and on. The garbage collector was quite new at the time, we were rather proud of it and curious about it, and our normal output was on a line printer, so it printed a full page every time it was called giving how many words were marked and how many were collected and the size of list space, etc. During a previous rehearsal, the garbage collector hadn't been called, but we had not refreshed the LISP core image, so we ran out of free storage during the demonstration.

Nothing had ever been said about a garbage collector, and I could only imagine the reaction of the audience. We were already behind time on a tight schedule, it was clear that typing out the garbage collector message would take all the remaining time allocated to the demonstration, and both the lecturer and the audience were incapacitated by laughter. I think some of them thought we were victims of a practical joker.


---

Outline:

> One list to bring them all,
> One list to find them.
> One list to rule them all,
> And in parentheses bind them.

* Introduction to lisp
  * original paper
  * lists
  * atoms
  * special forms
* Motivation (me)
* Motivation (others)
* Introduction to komplott
* Parsing lisp
* Garbage collector
* Printing lists
* Eval
  "The hard drive - the true heart of the mother modem"
* Next steps
  * Reduce line count (smallest possible lisp)
  * Syscalls
  * Standard library
  * Macros
  * Compiling
  * CPS
  * TCO
* Papers and links


> When I wrote the following pages, or rather the bulk of them, I
> lived alone, in the woods, a mile from any neighbor, in a house
> which I had built myself, on the shore of Walden Pond, in Concord,
> Massachusetts, and earned my living by the labor of my hands only. I
> lived there two years and two months. At present I am a sojourner in
> civilized life again.

---

For those who don't know, the above quote is the opening line to
Walden by Henry David Thoreau, great american author from the 19th
century and pioneer environmentalist. It's really striking to read
Walden now, how much of his thinking seems really contemporary. Also I
really like the last line: At present, I'm a sojourner in civilized
life again. Sums up life as a remote worker. It's like yeah, I'm
wearing pants -- temporarily.

It's also interesting that Thoreau was not really a luddite or against
modern society. He wrote about the railroad, the latest invention of
the time, as both a technological marvel that would be tremendously
helpful to people and lamenting the environmental destruction and
noise that it brought.

I've kind of been having similarly mixed feelings towards computing
and the internet. When I went to university in 1999, it was the height
of the dot com bubble, and we really thought that the internet was
going to change everything for the better. Everyone will have free
access to information, it'll eliminate poverty and boost democracy
world-wide, sharing is caring, everyone can make a website, no need
to go through any middle-men anymore... Well, now it's obvious that
sure, some of that is true but there's a massive dark side to the
internet as well, the same technology that can power democracy can be
used against it.

In 2017 I gave a talk called Package managers all the way down where I
really just wanted to express my concern over how programming
languages have developed lately. Again, mixed feelings - I love
tooling like cargo for rust which makes life really easy for
developers, but at the same time handling thousands of dependencies
turns out to be really hard and security is a big issue.

So I guess both with computers in general and with programming, I've
felt a need to pull a Walden. To grab my axe and walk into the woods
and build myself a hut from scratch.


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



