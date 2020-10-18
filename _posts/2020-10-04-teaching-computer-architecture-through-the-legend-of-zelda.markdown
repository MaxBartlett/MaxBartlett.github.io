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

Video games have long been used as a means of teaching computer science fundamentals.[^1]<sup>,</sup>[^2]<sup>,</sup>[^3] It's easy to see why, as 73 percent of Americans ages 2 and older play video games.[^4] Resarch has shown that visualization helps science and engineering students learn, and video games are primarily a visual medium.[^5]

While many books, online classes, and entire programming frameworks[^6] use this approach for beginners, the pickings are slim for more advanced programmers. I've seen plenty of students fall in love with programming through Python and Java, only to be scared off when they encounter C in upper-level computer architecture and operating systems courses.

<p style = "float:left;width:100%">
   <img src="https://media.geeksforgeeks.org/wp-content/uploads/memoryLayoutC.jpg" width="50%" height="250" />
   <img src="https://i.stack.imgur.com/xY8WC.png" width="49%" height="250" />
</p> <br>

If you're familiar with this material, you may have seen the above pictures in textbooks and lecture slides. These diagrams are a good start, but students are likely to get lost without context. What does proper memory management look like in an actual computer program -- and more importantly -- what happens when things go wrong? These courses often point out "undefined behavior" as a result of memory mismanagement without demonstrating what this means in practice. Memory leaks and buffer overflows are some of the most common and pernicious bugs,[^7] and future software engineers should thoroughly understand their effects.

Video games are an effective teaching tool in this area, as they must constantly load numerous objects and functions in and out of memory. But with limited budgets and QA testing, as well as tight deadlines, plenty of video games are released with massive vulnerabilities. By exploiting severe memory leaks, players can execute arbitrary code, and the possibilities
are endless.[^8]<sup>,</sup>[^9] Let's take a look at some of the better-documented examples in *The Legend of Zelda: Ocarina of Time*, and *The Legend of Zelda: Wind Waker*.[^10]<sup>,</sup>[^11]

### Arbitrary Code Execution in *Ocarina of Time*

By performing a glitch known as Stale Reference Manipulation (SRM), the player can edit variables, call functions, and even execute arbitrary code in the game's heap. The heap contains a doubly-linked list of actor instances, actor functions, and other data. Actor instances can point to other actor instances, for example, holding and dropping items, or collecting an item by using the boomerang. Using glitches, the player can make these actors disappear and write other data to the dangling pointer. Writing arbitrary data to the heap often causes the game to crash, but in other cases, the player can obtain items out of sequence, teleport to locations and cutscenes, and perform many other unintended actions.[^12] Using this method, the game can be completed in under 10 minutes. The videos below do a fantastic job at explaining the details:

<iframe width="560" height="315" src="https://www.youtube.com/embed/wdRJWDKb5Bo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/SPi_d0zhNFc" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

### Actor Unloading in *The Wind Waker*

Instead of taking advantage of null pointers to write data to the heap and execute functions, actor unloading removes actors by fragmenting memory. There are a number of glitches to duplicate certain actors, including arrows, items, enemies, and more. When the game spawns enough duplicate actors and never deallocates them from memory, other actors may fail to load. This is primarily used to unload a large barrier to the final boss, effectively allowing the player to skip more than half of the game. The video below shows this in practice:


<iframe width="560" height="315" src="https://www.youtube.com/embed/UvkLrPWVPao" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

### Conclusion

Most of the resoruces on these topics are heavily geared towards the speedrunning community,[^13] though they could easily be adapted for a purely educational purpose. [This video](https://www.youtube.com/watch?v=1hs451PfFzQ) serves as a great example, using elegant graphics alongside video game footage to explain complex computer science and statistics to a general audience. 

As shown in the videos above, most modern video game console emulators have features such as the ability to load save states, replay input, pause and start the game, advance gameplay frame by frame, and view and modify memory addresses and their corresponding instructions in assembly language. Many of these emulators are open source, such as Dolphin,[^14] allowing educators to develop and share their own visualization tools.

Video games are complex enough to serve as a model for many topics in computer science, yet they are simple in that many students are already familiar with their basic mechanics. I'm optimistic about the potential of video games to improve, and I encourage educators  notoriously complex topics and engage students in upper-level computer science courses.

#### Footnotes

[^1]: [Teaching computer science through game design](https://ieeexplore.ieee.org/document/1297314)
[^2]: [Idaho Teacher Uses Video Games to Teach Science](https://www.govtech.com/education/Idaho-Teacher-Uses-Computer-Video-Games-to-Teach-Science.html)
[^3]: [Introducing Java Programming to Kids: Minecraft Mods and JavaFX](https://www.youtube.com/watch?v=ulM0u9RXFvQ)
[^4]: [According to The NPD Group, 73 Percent of U.S. Consumers Play Video Games](https://www.npd.com/wps/portal/npd/us/news/press-releases/2019/according-to-the-npd-group--73-percent-of-u-s--consumers-play-video-games/)
[^5]: [Visual learning for science and engineering](https://ieeexplore.ieee.org/document/1510540)
[^6]: [Scratch Programming Language](https://scratch.mit.edu/)
[^7]: [The Heartbleed Bug](https://heartbleed.com/)
[^8]: [[TAS] Pokémon Yellow - Arbitrary Code Execution](https://www.youtube.com/watch?v=Vjm8P8utT5g)
[^9]: [How a robot got Super Mario 64 and Portal “running” on an SNES](https://arstechnica.com/gaming/2017/01/how-a-robot-got-super-mario-64-and-portal-running-on-an-snes/)
[^10]: [SRM Explained](https://www.zeldaspeedruns.com/oot/srm/srm-explained)
[^11]: [Actor Unloading](https://www.zeldaspeedruns.com/tww/Miscellaneous/actor-unloading)
[^12]: [Lost Woods SRM Compilation (age, magic, inventory, quest status, farore's wind)](https://www.youtube.com/watch?v=eldzc3cmQ3s)
[^13]: [You Think Your Favorite Video Game Is Hard? Try Speedrunning It](https://www.npr.org/2020/09/24/916167970/you-think-your-favorite-video-game-is-hard-try-speedrunning-it)
[^14]: [Dolphin Emulator](https://dolphin-emu.org/)
