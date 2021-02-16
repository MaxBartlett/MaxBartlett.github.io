---
title: "How To Download Google Drive Files From Owner's Trash"
layout: post
date: 2021-02-15 15:00
image: /assets/images/google-drive-blog-header.jpg
headerImage: false
tag:
- Python
- Google
projects: false
category: blog
author: maxbartlett
description: "Max Bartlett's Blog"
---

![](/assets/images/google-drive-blog-header.jpg)

I was recently digging through some of my emails and found a Google Drive link to an old computer science project I'd done in high school. When I clicked on it, I was greeted with the message, "No preview available. File is in owner's trash." with no option to download the file. You can see for yourself at [this link](https://drive.google.com/file/d/0B7LBRM3NgtFSSFFPTUR1QURiT3M/view).

A quick search uncovered a surprising exploit.[^1] I could download this file from the owner's trash by replacing parts of the URL. You can see the result at [this link](https://drive.google.com/u/0/uc?id=0B7LBRM3NgtFSSFFPTUR1QURiT3M&export=download).

I wrote a simple Python script that you can use from the command line instead fixing the links manually, which you can find [here on my GitHub](https://github.com/MaxBartlett/download-google-drive-files-from-owners-trash).

You can also find the [project itself on my Github](https://github.com/MaxBartlett/LofScape), along with a demo video in the README. It was fun to look at code of mine from so long ago, and I'm glad I was able to download the file without bothering my old CS teacher, but I hope this vulnerability doesn't cause any widespread privacy issues for Google Drive users.

#### Footnotes

[^1]: [Stack Overflow: Google Drive - “File is in owner's trash” - how to download](https://webapps.stackexchange.com/questions/142139/google-drive-file-is-in-owners-trash-how-to-download)