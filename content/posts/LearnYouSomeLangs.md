+++
title = "Learn You Some Langs"
date = 2023-06-30

[taxonomies]
categories = ["programming"]
tags = ["theory"]
+++

Jack of all trades master of none.

Jack of all trades master of one?

Jack of all trades master of none... but often better than a master of one?

What the heck was the original quote, and why would learning more programming languages be helpful?

<!-- more -->

I'm convinced the original quotation's etymology has been lost to history. While the modern joke has oft been "jack of all trades master of none" as a jab at a generalist, two possible etymologies of a different light have been attributed to this saying:
- "Jack of all trades master of **one**" has been attributed to Benjamin Franklin. The purpose being that one should have wide knowledge, and be able to apply it to a particular specialty.
- "Jack of all trades master of none, **but often better than a master of one**" doesn't have an attribution to a particular person that I've heard. However, the claim is that this quote pre-dates the modern truncated counterpart. The purpose being as a complement to someone who was not the top in their field but was able to get things done due to their wide knowledge.


Alright, what does this have to do with learning a language? I'd advocate you're a jack of all trades, master of **one**. If you're a career programmer like myself, you will have a small set of languages you code in daily (C for myself). Exposure to languages you don't use daily for your profession can help one your professional skills though, but expanding your understanding of programming and teaching you new tools you can add to your daily work flows.

You don't need to learn these new languages by heart, C is the only language I can confidently spit program out in that's syntactically correct first try, and has all the right libraries/headers included for the task at hand. However, there are many languages I have written small projects in and could pick back up with a small reference on hand.

So, what types of languages should you learn? And what are their benefits?

# Embedded Languages

Everything use to be an embedded language. We're far abstracted away from hardware these days. Taking the time to learn an embedded language will expand your knowledge of what's going on under the hood, what is physically slow in hardware and how does that affect your program? Even if you're a javascript developer, if you understand the fundamentals of what's going on in the hardware you'll be able to write far more efficient programs.

Possible Languages:
- C
- C++
- Rust
- Zig

If you have a good time, you can always write a fun project on a microcontroller, such as custom keyboard firmware!


# Learn a Native Language

A native language interacts with its operating system directly. You're making system calls, and collaborating with the operating system. Like the embedded languages before them, these languages help give you a better understanding of what's going on at the operating system level. Unless you're a kernel/firmware/embedded engineer your code is running on an operating system, and how your code interacts with it has impacts on how your system will behave.

All the embedded languages apply here and so do:
- Swift & Objective-C
- Go
- Odin

If you want to write more performance demanding software like a game or a ray tracer, these are great choices.

# Learn a Scripting Language

This recommendation is more utilitarian. However, if you don't have one of these in your toolbox already it is an important addition. A simple scripting language that you can produce simple tools in quickly will make your life easier. Need some ad-hoc automation for testing your code as you bring it up? Need to do some cleanup of file names and locations? Got a dump of logs you would like to do some one-off post processing on? Yeah, you could do all this in any language, but if that language is in the above list of languages it'll cost more dev time than it's work.

Possible languages include:
- Bash
- Lua
- Typescript
- Javascript
- Powershell
- Python


# Learn a Functional Language

Functional programming languages can feel like alien nonsense when first diving into them. However, their very different paradigm forces you to solve problems a different way. Mondas, immutable data, composable functions, optionals, pattern matching, and so on. It feels wild if you come from a more traditional world of languages like C and Java. However, parts of these functional concepts are starting to show up in modern languages such as Rust. Solving problems in a familiar but alien environment will force your brain out of its box. I program C in a kernel, yet I am glad I took the time to learn and write in functional languages for the new perspective it's given me when problem solving.

Functional languages come with some of the best getting started resources, so my recommendations are a little more verbose this time:

- Haskell: Haskell's online learning course and book ["Learn You A Haskell for Great Good!"](http://learnyouahaskell.com) has been around the longest. Haskell was one of the first functional languages I played with back when I was in college. I wouldn't recommend it as THE language to learn these days, but it deserves its top spot here for its history.
- OCaml: OCaml has its own [learning portal](https://ocaml.org/docs).
- F#: Microsoft's own .NET functional language. It has a [learning website](https://dotnet.microsoft.com/en-us/languages/fsharp) for getting started.
- Elixir: Elixir has ["Joy of Elixir"](https://joyofelixir.com).


# Learn a Web Language

Look, as a C developer I'm often surrounded by snide comments aimed the direction of web developers. I think the web is a beautiful technology, mastery of the DOM, working across a broad range of devices and browsers, and well designed client-server interactions is no easy feat. If you're more on the native side of development, there is a lot to be learned here about how to make software that works across a broad range of environments.

Languages:
- Cheat and use WASM. Take other languages you learned in this list and use them as a web language.
- Javascript or Typescript
- Ruby
- PHP
- Elixir + [Phoenix](https://www.phoenixframework.org)