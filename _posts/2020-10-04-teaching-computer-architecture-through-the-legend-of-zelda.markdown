---
title: "Teaching Computer Architecture through The Legend of Zelda"
layout: post
date: 2020-10-04 15:00
image: /assets/images/zelda-blog-header.png
headerImage: false
tag:
- Education
- Computer Architecture
- Speedrunning
projects: false
category: blog
author: maxbartlett
description: "Max Bartlett's Blog"
---

![](/assets/images/zelda-blog-header.png)

### Introduction

Video games have [long been used](https://ieeexplore.ieee.org/document/1297314) as a [means of teaching](https://www.govtech.com/education/Idaho-Teacher-Uses-Computer-Video-Games-to-Teach-Science.html) [computer science fundamentals](https://www.youtube.com/watch?v=ulM0u9RXFvQ). It's easy to see why, as [73 percent of Americans ages 2 and older play video games](https://www.npd.com/wps/portal/npd/us/news/press-releases/2019/according-to-the-npd-group--73-percent-of-u-s--consumers-play-video-games/). Resarch has shown that [visualization helps science and engineering students learn](https://ieeexplore.ieee.org/document/1510540), and video games are primarily a visual medium. 

While many books, online classes, and [entire programming frameworks](https://scratch.mit.edu/) use this approach for beginners, the pickings are slim for more advanced topics. I've seen plenty of students fall in love with programming through Python and Java, only to be scared off when they encounter C in upper-level computer architecture and operating systems courses.

<p style = "float:left;width:100%">
   <img src="https://media.geeksforgeeks.org/wp-content/uploads/memoryLayoutC.jpg" width="50%" height="250" />
   <img src="https://i.stack.imgur.com/xY8WC.png" width="49%" height="250" />
</p> <br>

If you're familiar with this material, you may have seen the above pictures in textbooks and lecture slides. These are a good start, but students are likely to get lost without context. What does proper memory management look like in an actual program -- and more importantly -- what happens when things go wrong? These courses often point out "undefined behavior" as a result of memory mismanagement without demonstrating what this means in practice. Memory leaks and buffer overflows are some of the most [common and pernicious bugs](https://heartbleed.com/), and future software engineers should thoroughly understand their effects.

Video games are an effective teaching tool in this area, as they must constantly load numerous objects and functions in and out of memory. But with limited budgets and QA testing, as well as tight deadlines, plenty of video games are released with massive vulnerabilities. By exploiting severe memory leaks, players can execute arbitrary code and [the possibilities](https://www.youtube.com/watch?v=Vjm8P8utT5g
) [are endless](https://arstechnica.com/gaming/2017/01/how-a-robot-got-super-mario-64-and-portal-running-on-an-snes/). Let's take a look at some of the better-documented examples in [*The Legend of Zelda: Ocarina of Time*](https://www.zeldaspeedruns.com/oot/srm/srm-explained), and [*The Legend of Zelda: Wind Waker*](https://www.zeldaspeedruns.com/tww/Miscellaneous/actor-unloading).

### Arbitrary Code Execution in *Ocarina of Time*

By performing a glitch known as Stale Reference Manipulation (SRM), the player can edit variables, call functions, and even execute arbitrary code in the game's heap. The heap contains a doubly-linked list of actor instances, actor functions, and other data. Actors can point to other actors, for example, the player can hold and drop items. By performing certain glitches, the player can make these actors disappear and write other data to the dangling pointer. Writing arbitrary data to the heap often causes the game to crash, but in other cases, the player can obtain items out of sequence, teleport to locations and cutscenes, and perform many other unintended actions. Using this method, the game can be completed in under 10 minutes. The videos below do a fantastic job at explaining the details:

<iframe width="560" height="315" src="https://www.youtube.com/embed/wdRJWDKb5Bo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/SPi_d0zhNFc" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

### Actor Unloading in *The Wind Waker*

Instead of taking advantage of null pointers to write data to the heap and execute functions, actor unloading removes actors from memory by fragmenting memory. There are a number of glitches to duplicate certain actors, including arrows, items, enemies, and more. When enough duplicate actors are spawned without being deallocated, other actors may fail to load. This is primarily used to unload a large barrier to the final boss, effectively allowing the player to skip more than half of the game. The video below shows this in practice:


<iframe width="560" height="315" src="https://www.youtube.com/embed/UvkLrPWVPao" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

### Conclusion

Most of the resoruces on these topics are heavily geared towards the [speedrunning community](https://www.npr.org/2020/09/24/916167970/you-think-your-favorite-video-game-is-hard-try-speedrunning-it), though they could easily be modified for a purely educational purpose. Videos such as [this one](https://www.youtube.com/watch?v=1hs451PfFzQ) use elegant graphics alongside video game footage to explain complex computer science and statistics to a general audience. 

As shown in the videos above, most modern video game console emulators have features such as the ability to load save states, replay input, pause and start the game, advance gameplay frame by frame, and view and modify memory addresses and their corresponding instructions in assembly language. Many of these emulators are open source, such as [Dolphin](https://dolphin-emu.org/), allowing educators to develop and share their own visualization tools.

Video games are complex enough to serve as a model for many topics in computer science, yet they are simple in that many students are already familiar with their basic mechanics. Applying advanced concepts to students' existing mental models in this way is largely unexplored, though I'm optimistic about its potential to to simplify notoriously complex topics and engage students in upper-level computer science courses.