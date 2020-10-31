---
title: "The Art and Craft of Saxophone Mouthpiece Design"
subtitle: "Duke University Bass Connections Project - 2020"
layout: post
date: 2020-06-30 17:00
headerImage: https://bassconnections.duke.edu/sites/bassconnections.duke.edu/files/styles/large/public/saxophone-fb-v2.jpg?itok=qEr0nZUg
tag:
- R
- Bass Connections
- Research
projects: true
category: project
author: maxbartlett
description: 
--- 

![](https://bassconnections.duke.edu/sites/bassconnections.duke.edu/files/site-images/mouthpiece-rev.png)

## Introduction

During my final year at Duke I had the privilege of working with fellow undergraduate students, professors, and university staff on a research project exploring tenor saxophone mouthpiece design. This project was made possible through Bass Connections, a selective program that "bridges the classroom and the real world, giving students a chance to roll up their sleeves and tackle complex societal problems alongside faculty from across Duke."[^1] While I'm not sure that saxophone mouthpiece design qualifies as a complex societal problem, it was rewarding to spend time on a project that involved both my love of music as well as computer science and statistics.

Throughout the course of the project I used technologies that I likely wouldn't have had access to otherwise, namely the Micro-CT scanner, 3D printer, and 3D modelling software available at Duke's Shared Materials Instrumentation Facility.[^2] Though I'd had plenty of experience visualizing and analyzing data, for the first time it was up to me and my teammates to make sense of it all with relatively little guidance. Working independently on a long-term research project was a complete 180 from the regular coursework to which I'd grown accustomed, and for that reason I'd recommend it to all students.

Instead of repeating the details of the project itself, which can be found in the footnotes, I'll use this space to outline the project's showcase website developed in R Shiny.[^3]<sup>,</sup>[^4]<sup>,</sup>[^5]<sup>,</sup>[^6]<sup>,</sup>[^7]

## Background

At the end of each academic year, Bass Connections teams gather in Penn Pavillion to show off their hard work through poster presentations and lightning talks. That was the case, at least, until the outbreak of COVID-19 and the subsequent plan to host a virtual showcase. As our project involved 3D models, dynamic visualizations, and audio, we found it liberating to deliver our results through the web in addition to our poster.[^8] Given that I had just a few weeks to produce this from scratch, I turned to R Shiny. However, I'd never worked on a Shiny project at this scale, and I took away some lessons that I've since been able to apply to other projects.

## Development

In case you're unfamiliar with Shiny, it's a framework for developing web applications in R in the tidyverse family of packages.[^9] As Bootstrap handles the front-end of Shiny, the R Shiny site boldly states "No web development skills are required." I'd add an asterisk saying "for most simple projects." Where Shiny really shines (pun most definitely intended) is in its ability to create bite-sized dynamic visualizations or dashboards -- not full-fledged websites. There is always a trade-off between a technology's learning curve and flexibility. Spend some time on Shiny's gallery and this will quickly become evident.[^10]  I found myself getting frustrated as I was forced to use hacky solutions to do things that would be simpler in other languages. To be fair, some of this could be attributed to the fact that I was mostly learning as I went along, but if I had to do it all over again I would likely use d3.js instead.

It wasn't until late in the project that I discovered some Shiny features that attempt to rectify these issues, namely JavaScript extensions, modular code, a testing framework, and a means of improving performance and scalability. A better awareness of these features and more time spent researching them up front would've resulted in less technical debt. Since finishing this project, I've been more diligent about digging into documentation and building small examples to test my knowledge when learning new frameworks. I've also made an effort to better orient myself towards test-driven development, an emphasis that I feel was lacking in my undergraduate education.

While performance wasn't a pressing issue at this scale, there are certainly places where I could've optimized the application. There is a good deal of computation that could've been performed elsewhere, for example, in data wrangling and sorting. But this didn't noticeably impact performance and I left it as is. This also made the application more flexible in case I wanted to make additions in the future. However, some optimizations were necessary. For example, the original stereolithography (.STL) files contained millions of polygons and were many gigabytes each, but I scaled this down by several orders of magnitude without a noticeable loss of quality (to the naked eye, at least). 

It wasn't all bad, however. Shiny allowed me to quickly create mockups to share with my team and get continuous feedback. In particular, Bootstrap's grid system and R Markdown made it simple to test out different layouts for the visualization and narrative pages. Given the purpose of the app and the time constraint, these pages didn't have to be flashy, as long as they effectively communicated our research. Additionally, R's open source community provides extended functionality through the 15,000+ packages available on the Comprehensive R Archive Network (CRAN). Although there are things I would change, I was ultimately satisfied with the outcome of the web app, as I was able to share the my work with family and friends who couldn't have otherwise seen my team's presentation.

## Conclusion

I feel fortunate not only to have worked on the R Shiny app, but on the project as a whole with my incredible teammates. I learned a great deal about new technologies as well as my own work style and how I could improve when working under pressure. I anticipate that this awareness will continue to be useful as I work in team settings in the future.

#### Footnotes

[^1]: [Duke Bass Connections](https://bassconnections.duke.edu/)
[^2]: [Duke SMIF](https://smif.pratt.duke.edu/)
[^3]: [The Art and Craft of Saxophone Mouthpiece Design (2019-2020)](https://bassconnections.duke.edu/project-teams/art-and-craft-saxophone-mouthpiece-design-2019-2020)
[^4]: [Archiving Saxophone Mouthpiece Designs](https://bassconnections.duke.edu/virtual-showcase/archiving-saxophone-mouthpiece-designs)
[^5]: [Where science meets art -- and all that jazz](https://issuu.com/dukemagazine/docs/1017_dm_spring2020_issu/22)
[^6]: [The Art and Craft of Saxophone Mouthpiece Design Showcase Website](https://maxbartlett.shinyapps.io/project/)
[^7]: [Showcase Shiny App Github](https://github.com/Duke-Bass-Connections-Saxophone-Project/Showcase-Shiny-App)
[^8]: It should be noted that our poster was named runner-up in the poster competition out of 40+ entries thanks to the work of team member Gia Jadick.
[^9]: [R Shiny](https://shiny.rstudio.com/)
[^10]: [R Shiny Gallery](https://shiny.rstudio.com/gallery/)