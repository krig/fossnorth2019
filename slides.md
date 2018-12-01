
<h1 style="font-size:1000%;">()</h1>

```
Kristoffer Grönlund
kgronlund@suse.com
```

---

> When I wrote the following pages, or rather the bulk of them, I
> lived alone, in the woods, a mile from any neighbor, in a house
> which I had built myself, on the shore of Walden Pond, in Concord,
> Massachusetts, and earned my living by the labor of my hands only. I
> lived there two years and two months. At present I am a sojourner in
> civilized life again.

---

> Yes, that was the big revelation to me when I was in graduate
> school—when I finally understood that the half page of code on the
> bottom of page 13 of the Lisp 1.5 manual was Lisp in itself. These
> were “Maxwell’s Equations of Software!” This is the whole world of
> programming in a few lines that I can put my hand over.

---

<!-- .slide: data-background-image="img/lisp15.png" -->

---

<!-- .slide: data-background-image="img/evalquote.png" data-background-size="contain" -->

---

### Recursive Functions of Symbolic Expressions 
### and Their Computation by Machine, Part I

---

> [..] whereby a machine could be instructed to handle declarative as
> well as imperative sentences and could exhibit “common sense” in
> carrying out its instructions.

---

This is not the greatest lisp in the world.

This is just a tribute.

---

github.com/krig/LISP

---

1. `Makefile`
2. `komplott.c`
3. `tests/lisp15.scm`

---

```
.PHONY: all test clean

all: komplott

komplott: komplott.c
	$(CC) -g -Og -Wall -Werror -std=c11 -o $@ komplott.c

test: komplott tests/lisp15.scm
	./komplott tests/lisp15.scm

clean:
	rm -f ./komplott
```

