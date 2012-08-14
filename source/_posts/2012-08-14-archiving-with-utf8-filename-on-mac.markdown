---
layout: post
title: "Archiving with utf8 filename on Mac"
date: 2012-08-14 22:54
comments: true
categories: Mac
tags: utf8 utf-8 unicode encoding encode archive compression broken_character Windows7 Mac
---

`Problem: Archiving with utf8 filename on Mac`

`Status: Solved`

`Comment: Not Cute Solution`

Details:

When you compress some files in Windows 7, and unarchive in Mac, you will see broken character. English is fine, reader for this post probably use other language. 

Because default compression in Windows operating system doesn't support maintaining file names in utf8, you need to use other archive program which support utf8. 

I recommend [Bandizip](http://www.bandicam.com/bandizip). There are two version, Mac version and Windows version. Because of software cost, I choose Windows version. It is **free**. I run it  on Mac with [Parallels Desktop 7](http://www.parallels.com/products/desktop/) coherence mode. This is Windows 7 virtualisation solution for Mac. 
You can find how to set up Bandizip utf-8 aware archiving in <http://www.bandicam.com/bandizip/help/utf8/>.
