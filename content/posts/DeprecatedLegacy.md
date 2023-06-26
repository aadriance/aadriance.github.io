+++
title = "Your Code is Either Deprecated a Hero or Maintained as Legacy"
date = 2023-06-25

[taxonomies]
categories = ["programming"]
tags = ["workflow"]
+++

There is frequent discourse on how to make code more maintainable. I for one think more discussion is needed on making code deprecateable. It may be nice to envision that a beautifully designed code base will last forever, but there are two things certain for code; becoming legacy, getting deprecated, and off by one errors.

<!-- more -->

# Isn't Disposable Code Bad?

I believe all code is destined to be legacy code. It doesn't matter if it's because the architect behind it retired, business needs changed, or you wrote this 6 months ago in a caffeine-hyped state while hurdling toward a deadline at terminal velocity. Some day, this well-intentioned pile of code is going to get opened to investigate a bug, and the poor soul trying to figure out what's wrong will drop their head into their hands bewildered this code passed muster and made it to production. Sometimes you check the git log and realize it was your arch-rival, your past self, that produced this dastardly code.

Once this legacy code has lived a good life, what's to be done with it? I advocate that it is given a respectful funeral and replaced. Why?

- Solving a problem a 2nd time with the knowledge of how the first faired in production will result in a more robust implementation. The WET (Write Everything Twice) style of programming even advocates you immediately deprecate the first implementation.
- Business needs always changes. Good code won't topple over with every single change, accumulate years of changes and even the most carfelly architected manor will look more Winchester mansion than modern pent house.
- Most code has an atrociously low [bus factor](https://en.wikipedia.org/wiki/Bus_factor). If code is designed to be deprecateable, you can always re-set a components bus factor with a new implementation!


Side note: Don't re-write things just because. The life time of code varies wildly from decades for certain pieces of software in an operating system to a couple of years (if you're lucky) in javascript land. I can prescribe no universal life-time for code. I do know that life time is finite, and likely much shorter than your own.


# The Art of Deprecation

## The Logging is Mightier Than The Comment

 I have seen many code snippets with an edge case with a comment along the lines of "You should not be here". Cool. I have no idea why I'm not suppose to be here, and I have no idea if people end up here in production. A good log serves two purposes:
 - Telemetry for if this condition is hit in production. This insight is valuable for an updated implementation. Can this case be re-factored out? Do we want more in-depth handling here?
 - Better insight into what this log/comment represents. Since logs are viewed separate from code I observe people write better messages rather than assuming it is self evident from the code.


Having the data from a current implementation lets a re-implementation make smart decisions about were to spend effort. Additionally, without logs to guide the new implementation you'll either be forced to import legacy code you don't understand or risk leaving it out and having a production incident.

## Comments Are Good Too

Sometimes comments are still needed. That time is when the business logic is non-obvious. Did this implementation come from a specification? Please add a comment that says what specification, it's version, and relevant section. Is there a clever hack added to the code to avoid an issues that's been seen in production? Please have a reference to the bug in the ticket tracking system or a tl;dr explanation with the code. I've seen many a hacks in code that says "added to avoid issues seen in vendor Xs environment". Who is the vendor? What was the issue? Or worse, there is some random block of code that looks like:

```c
if (stars_aligned != 0) {
   x = x >> 5;
}
```

Please, make life easy on your future self and the future maintainers and deprecators of the code and leave an obvious history.

## Avoid Coupling

No, not the object oriented kind. In the OO space, avoiding coupling is often talked about on a micro scale where no class should depend on others. That's usually unrealistic, creates more abstractions than are useful, and is going to make it harder to deprecate the code when the new author is trying to reason about the 7 layers of hell the code was abstracted into.

Instead, avoid coupling from an interface point of view. I'll use a real world technology example: IPMI. IPMI is a protocol for communicating with a special co-processor that ships with server machines for management purposes (machine reboot, firmware flashing, sometimes KVM, and so on). IPMI can travel over several hardware interfaces including: KCS, SSIF, HTTP. The specifics of these interfaces are moot. What would be important for the purposes of deprecation is that the SSIF interface not have coupling with the KCS interface. This would allow a new SSIF interface to be dropped in with ease, allowing for a gradual deprecation of the code instead of a full blown apocalyptic rebirth. Do the smaller parts of the SSIF interface depend on one another? Is it a monolithic module without much abstraction? I don't care if I can slot a new one in.

## Have High Level Documentation

I run documentation days for my team. I love me some documentation. I don't love over documentation though the quickly grows stale because it no longer matches reality. What I personally do when writing documentation is the following:
- Document large architectural pieces that won't change short of a large re-write.
- Document interesting history where appropriate. Sometimes commenting on edge cases isn't enough to capture large pieces of architecture that were implemented a certain way.


# Get Out There and Delete Code

There is probably too much of it anyway :) While you're at it make sure your new stuff is easy to delete in the future!