
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

#### Recursive Functions of Symbolic Expressions 
#### and Their Computation by Machine, Part I

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

# ?

---

## special forms

---

```
(quote X) ; -> X

(cons X Y) ; -> (X Y)

(cond (<case1> <then1>) (<case2> <then2>) ...)

(begin EXPR...)

(or EXPR...)

(define NAME EXPR)

(lambda (ARG...) BODY...)
```

---

## `lisp15.scm`

---

```
(define cadr (lambda (c) (car (cdr c))))
(define cdar (lambda (c) (cdr (car c))))
(define caar (lambda (c) (car (car c))))
(define cddr (lambda (c) (cdr (cdr c))))
(define caadr (lambda (c) (car (car (cdr c)))))
(define cadar (lambda (c) (car (cdr (car c)))))
(define caaar (lambda (c) (car (car (car c)))))
(define caddr (lambda (c) (car (cdr (cdr c)))))
(define cdadr (lambda (c) (cdr (car (cdr c)))))
(define cddar (lambda (c) (cdr (cdr (car c)))))
(define cdaar (lambda (c) (cdr (car (car c)))))
(define cdddr (lambda (c) (cdr (cdr (cdr c)))))
(define not (lambda (x) (cond ((null? x) #t) (#t #f))))
(define atom? (lambda (x) (cond ((null? x) #f) ((pair? x) #f) (#t #t))))
(define else #t)

```

---

```
; build assoclist from lists of keys and values
; x = keys
; y = values
; a = assoclist
(define pairlis (lambda (x y a)
                  (cond ((null? x) a)
                        (else (cons (cons (car x) (car y))
                                    (pairlis (cdr x) (cdr y) a))))))
```

---

```
; find value matching key in assoclist
; x = key
; a = assoclist
(define assoc (lambda (x a)
                (cond ((equal? (caar a) x) (car a))
                      (else (assoc x (cdr a))))))
```
