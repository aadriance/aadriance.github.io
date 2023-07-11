+++
title = "I'm a Programmer... Not that Kind."
date = 2023-07-08

[taxonomies]
categories = ["programming"]
tags = ["self"]
+++

I don't make pretty fonts appear in your document editor. I don't make ads appear in your search results. I didn't round the corners in your UI. I didn't deliver your email, or track our online order.

<!-- more -->

> Look, this article is in a similar vein to ["The Night Watch"](https://www.usenix.org/system/files/1311_05-08_mickens.pdf). If you only have time to read one, read James Mickens masterpiece instead of my blog post.

I'm a kernel developer. Which means even among programmers [73.6%](https://www.businessinsider.com/736-of-all-statistics-are-made-up-2010-2) don't know what I do. I have tried to describe what I do to the non-technical as follows: If Windows was a car, I work on the internal mechanisms of the engine. You never see it, you're not sure how it works, only the nerdiest of car buyers consider it in their purchasing decision, and without it the car doesn't do much.

Turning on your computer seems like a simple matter, you push button and you operating system loads up. In reality there are 100 different things that all must go off without a hitch for your login screen to greet you. We take running complex, heavily animated, dynamic websites for granted. At the foundation of that is a kernel that knows how to talk to a CPU, over a PCIe connection to a GPU, and have those things work together, just so you can be annoyed by the auto-playing video ad on an article.

Your program may open a UDP connection, but what is a UDP connection anyway? In your system is a network connection interface, running it's own microcosm of an operating system called firmware, connected to your CPU using PCIe, which has it's own network communication scheme (The Transaction Layer) to get data from CPU to device. The kernel must know about all these different pieces of hardware in the system, and the different microcosms of firmware. It must know how to talk to them, coordinate them. There be dragons behind these doors, and the kernel makes sure user mode needn't see the sausage factory.

Your computer crashed. Was it a 3rd party kernel driver? The kernel itself? A PCIe device that went off the rails? A firmware mis-implementation of a UEFI or ACPI feature? Perhaps there was a hardware ~~bug~~ errata. Who knows. Some kernel engineer will get your crash dump and troll through the archaic layer to unwind what series of unfortunate events lead you here.

Computers are crazy, a beautiful create, but crazy.


