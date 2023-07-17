+++
title = "What is High Quality Code Anyway?"
date = 2023-07-16

[taxonomies]
categories = ["programming"]
tags = ["theory"]
+++

A great many wars have been fought over the religion of quality code. For who has not had a peer slain by the zealots of [TDD](https://en.wikipedia.org/wiki/Test-driven_development)? What programmer among ye has not spent longer than their heart desired on a pull request to [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) their code further? Who has not seen a code base lain asunder by the conquest of [OOP](https://en.wikipedia.org/wiki/Object-oriented_programming)?

<!-- more -->

Snake oil comes in many flavors. What then marks the quality of your code? Fair reader, I think the following attributes contribute more to quality code than and checklist you can find in someones book.


## Make it readable, not clever

Is the below valid C code?
```c
  printf("%d\n", div(.numerator = 10, .denominator = 5));
  printf("%d\n", div( .denominator = 5,.numerator = 10));
```

Named args, no way! Yes way.

```c
#include <stdio.h>

typedef struct _DIV_ARGS {
  int numerator;
  int denominator;
}DIV_ARGS;

#define div(...) div_f((DIV_ARGS){__VA_ARGS__})

int
div_f(DIV_ARGS args) {
  return  args.numerator / args.denominator;
}

int
main (
  int argc,
  char **argv
  )

{

  printf("%d\n", div(.numerator = 10, .denominator = 5));
  printf("%d\n", div(.denominator = 5, .numerator = 10));
  printf("%d\n", div(10, 5));
  printf("%d\n", div(.numerator = 10, 5));

  return 0;
}
```

All these invocations return 2. Is this code clever? Sure, it's a simple way to add interesting syntax to C. Are you going to befuddle every normal C programmer that looks at your project? Yes.

Languages have an endless well of odd features that are often unused. There are serious problems that need all the stops pulled out. On average though, please write code that can be instantly read by someone else.

## Make it debuggable

A large part of debugability comes from the tooling surrounding your program. Often you won't be crafting memory dumps of the like manually. While you won't be crafting the debugging tools yourself, you can do things to make your job easier. Avoid piles of macro soup that obscure what is happening in the code. Don't add in half a dozen layers of unnecessary abstraction that turn the simplest of problems into tail chasing through abstract classes and function pointers passed through callback registered at the foundation of the world.

## Make it observable

The expected twin. Every system has a way to generate logs. I don't care if you're firmware banging bits to a COM port or a backend server generating event logs. This is more important when in the world of microservices where connecting a debugger to an exact point becomes much harder as the problem leads from one thing to the next. As a system becomes more complex having a way to observe it becomes more valuable. Being able to reconstruct what happened will be immensely valuable when production goes down.

The proportion of observability to debuggability is a sliding scale. Whatever your system, the combination of the two should allow for tracking down the majority of the problems without having to luck across a reproduction of the issue while a developer is monitoring it.

## Make it validatable 

For any given code change, you should be able to attest in a formal manner that no major functionality has broken. I'm not saying all code has to be TDD'ed into oblivion. I am saying that you're not going to remember the ends and outs of this code 6 months from now, and you certainly won't trust your co-worker to remember every detail. Create an agreeded test processes that makes it easy to validate a change is good before merging. Every service that offers hosting code offers the ability to run test passes against pull requests these days.

## Make it useable by peers

Look, I love programming languages, I've learned a fair few. I love Rust in particular. If you're the only developer on your team that understands Rust and the team hasn't also agreed to learn it for future use **DON'T WRITE SOMETHING IN RUST**. Do you want to maintain this particular code forever? Because that's how you end up having to maintain this code forever.

It's bad for you because you're going to feel the burden of being the only person that can maintain it. It's bad for your peers because they can't help themselves. It's bad for the user because if something goes wrong while you're on vacation they are just out of luck.

I am all for improving technology stacks. These are things that should be done with group buy-in to make sure you're not burning yourself out or producing code that's going to be dead when you decide to move on from the aforementioned burnout.

## Document it

Please don't comment on every line of code, or produce volumes of documents explaining the minute details of internals that will get changed before Christmas. However, that weird if statement you added to handle an edge case when a user requests a double chicken sandwich at 11:32 p.m. on the west coast while a career pigeon is flying over your data center? Maybe leave a comment as to why this oddity made its way into the code. If you have a unified ticketing system with a ticket of bug that can be pointed to even better.

In all seriousness, I ran across a comment that once said "This is implemented per the email with so and so, reach out to us for questions". This comment was two decades old, neither of those people had worked at the company in recent history, and that email certainly no longer exists.

Respect the intelligence of those reading your code, they don't need basic programming explained. What they can't glean from the code is WHY you inserted this statement to handle a non-obvious edge case.

## Have strong opinions, held loosely

This one is for you, and not your code. Software is a nebulous world where much is possible. You need to have your concept of what is right and needed to get from point A to point B in a sane fashion. If your code is solely composed of the opinion and direction of another, it will lack consistency. The larger the project gets, the more convoluted the layering of others opinion's becomes.

However, your opinions are just that. Your code does not exist in isolation. (Unless you're working on an open source project in which you are the sole maintainer and user. Congratulations / my condolences whichever is more appropriate). For the sanity of your peers, bend to common practices and the wisdom of the group when needed. Be able to understand, embrace, and adapt your programming view in combination with the lens of another. The sum of smart software engineers' opinions is oft greater than the individual.

## In conclusion

High-quality code has more to do about its ability to be managed and worked on by the team it exists in than it does the ~~religion~~  philosophies used to implement it. An embedded firmware for a medical device, and a portfolio website platform will have drastically different requirements and different techniques required to implement and maintain them. However, all software has to be managed by more than one person if it hopes to live a long and fulfilling life.

People and projects vary widely in their desires and needs. If instead of forcing checklists onto them no matter the circumstance, why don't we assess what our goal is, what talent and abilities we have on hand, and write some sane code to solve our problem. Yeah, I can't sell you a book on 10 steps to writing good software that way, but I'd rather software in general was saner.