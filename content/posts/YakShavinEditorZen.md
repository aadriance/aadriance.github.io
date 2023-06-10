+++
title = "A Programmers Zen Garden: Yak Shaving Your Text Editor"
date = 2023-06-09

[taxonomies]
categories = ["programming"]
tags = ["workflow"]
+++

What does a spiritual practice, the hair maintenance of a Himalayan bovine and reviewing text editor config files have in common? Having long since solved my core productivity problems, what motivates me to continue to poke and prod my config file? It's not about optimization anymore.

<!-- more -->

<center><iframe src="https://elm0.tv/wp-content/uploads/2022/07/Zen-Garden-Relax-GIF-by-ELMO-downsized_large.gif" width="480" height="480" frameBorder="0"></iframe><p><a href="https://elm0.tv">Art by Elm0 @ Elm0.tv</a></p></center>


If you're not familiar with yak shaving, please feel free to read some [source material](http://projects.csail.mit.edu/gsb/old-archive/gsb-archive/gsb2000-02-11.html) and return. Suffice it to say if you have found yourself on a late afternoon updating packages in your command line to get something working to get something working, you've shaved a yak of your own.


My history with editor fiddling started in college. Before college, I used IDEs based on the project I was working on. Xcode for objective-c, Eclipse for Java, Visual Studio for XDA, and GameMaker's text editor for GML. At the time I thought IDEs were the only option. When I had a single hobby project going at any given time using these IDEs was fine. In college, I had multiple projects for multiple classes and I got sick of juggling different code-editing solutions.

As I recall, the first editor  I picked up to try and unify my code editing was Notepad++. From there I hopped around to various other options: sublime, atom, TextWrangler, and Visual Studio Code (though it came on the scene at the end of my college career). There was one quarter where I did an entire Python class out of Notepad. I did try modal editors, with a focus on Vim, though Emacs wasn't ignored. They didn't stick at the time. In college, while I did fiddle with my editors, I found my editor didn't meaningfully impact my ability to complete schoolwork.

About a year into being a kernel developer at Microsoft, something interesting happened. I started having issues writing code efficiently. It's not that I didn't know how to achieve my tasks, the tedium of typing in the code was slowing me down. It was common for me to solve a problem on paper and then look at my editor in dread knowing the tedium I was about to embark on. Once I noticed this pattern of dread I knew the true solution resided in engineering my way out of the tedious tasks of code authoring. Brute force was not going to be a sustainable answer to this tedium.

Enter stage left: vim the yak.

It took approximately a month of forcing myself to daily drive vim before I felt comfortable, a month after and code writing stopped feeling tedious and began feeling delightful. 

> Note: I will not claim Vim is the only tool to remove the tedium from code authoring. In the wise words of my once professor Aaron Keen, "Some will tell you if you don't use Vim you're not a real programmer. Those people are called jerks." Vim is simply the tool I opted to invest the time in.


If you're familiar with Vim, you may be familiar with its ecosystem of plugins. Vim can become many different editors depending on what you choose to install. For a time I dipped in and out of Vim the program, and Visual Studio Code with Vim bindings. This continued until I jumped ship to neovim, and started maintaining a more complex configuration.

I've been through at least 2 major pure Vim configs and 4 different neovim configs ranging from nearly identical to my older Vim configs to brand-new Lua-based setups. I found myself recently mid-config re-write wonder why it is I continue to do this. The tedium is long gone, so why continue to fiddle?

> Sometimes "just 'cause" is a valid answer. Life isn't a spreadsheet to be optimized, it is an experience to be enjoyed. Our time is limited though, and pausing to introspect on how we are spending it is a worthwhile endeavor, personally and professionally.

I pondered this question for some time until I was admiring zen gardens one day. I have never partaken in the art of zen garden cultivation. I have appreciated its beauty, and meditative capabilities from a distance. I respect the desire to focus and take time to cultivate a meaningful space. Zen gardens, like all of nature, don't stay at a single point in time. It takes a lifetime of maintenance to keep the garden in shape.

I spend a lot of time on my computer. For me, my computer is like a zen garden. I want to maintain, fiddle with and appreciate it. There is something meditative for me when I crack open my config files and spend a couple of hours removing, adding, and updating plugins and configurations in my set-up. Perhaps I want a new font, perhaps I have a plugin I've not utilized and can be removed from my daily setup. Sure, I find myself off updating various dependencies on my computer, and then finding those dependencies have dependencies, and my rust compiler is out of date and can't build something a project needs, and so on. However, this yak isn't shaved in vain. No, I think this fiddling, the extra touch of honed personalization keeps my hours of interacting with a computer feeling human. After all, it's not just a screen with text on it. It is my garden, carefully crafted to my preferences.

## Edit

As an ultimate form of irony, I engaged in yak shaving to post this. It has been a year since my last site update and it turns out my Zola set-up was stale and so was the GitHub action to deploy my website updates. It's all live now 🐃