---
title: If Font Rendering Wants You Dead, The GUI Will Murder You
draft: false
date: 2026-03-02
tags:
  - programming
  - gui
---
# Pre-ramble

I haven't shipped any GUIs yet. But I've been around the kitchen, and you don't need to be a chef to see how boiling an egg is harder than making a Century Egg. 

Also, I'm not saying you can't build native. Sure you can; you just need N teams for N platforms, which is why people opt for Electron.

# The question

This came up in many convos. 

>[!quote]
>Why hasn't X language made a good cross-platform <u><b>native</b></u> GUI? 
>It's because X language sucks at making GUIs.

To this my answer is as follows: That's true for any X that's not JavaScript and any toolkit not called Electron.

Ok, but why is that?

If [font rendering wants you dead](https://faultlore.com/blah/text-hates-you/#style-layout-and-shape-all-depend-on-each-other), Cross-Platform GUIs will murder you in an alleyway, hack your body into several pieces and mail them to cops. 

Ok. But why? Let's imagine that we write our Cross-Platform GUI toolkit. Something like Qt but more pizzaz, and better.

# Part I: The Cross-Platform Part
Trying to build a cross-platform GUI part runs into first obstacle, the whole multiple platform thing.

## Which Way, GUI Man? Consistent or Native?
When developing a GUI toolkit, you are almost forced to choose a side. Does your UI try to look the same on all platforms (aka consistent), or do you offer a common denominator for all platforms? Or some kind of escape hatch?

The sanest choice is looking consistent cross-platform, but then your GUIs by default will look like clown shoes on some platforms. Most likely Apple.

## The Apple

By trying to build a competing UI toolkit to Apple you have implicitly declared yourself their mortal enemy. Apple wants to look unique and different from everyone else and by making a UI toolkit on your platform, you are going to run afoul of either their `"Almost NO Backwards Compatibility"` policy or their UI guidelines.

To be fair, Apple fanboys have been a bit quieter since [Liquid Ass](https://en.wikipedia.org/wiki/Liquid_Glass) came out, but before they would complain if an app wasn't jibing well with the rest of the apps.

## Linux, land of the free. Home of ten thousand competing DE.

Linux might be the worst to develop native GUIs. First, you don't know if you even have a GUI. Then you don't know which DE you have (and those determine window decoration and rough Human Interface Guidelines). Hopefully you picked consistency over native look and feel, so outside spawning a decorated window, you control its insides fully.

## Which Windows UI toolkit is in vogue?

Windows also isn't free of this. Hopefully you use the latest available toolkit. Except Ribbon. Ribbon is passé. It's all about, lemme check notes - Microsoft Copilot.

## What do you mean there are other Operating systems?

Yeah, there are other operating systems besides the big three. How will you account for their idiosyncrasies? Hopefully not, but who knows, right?

## The Graphics Stack

To make matters worse, you also need to account for different graphics stacks on different platforms. Windows gets you Direct X, and on MacOS you get Metal which looks like Vulkan, but with more of Apple's simmering hatred. On some ancient machines you just get OpenGL, and you need those if you want to make your UI stack perform well. 

# Part 2: The Gooey Part

All is going smoothly; you picked your platforms and languages, and you can even render a rectangle and a triangle. Now comes everything else.

## Font rendering

Just read the damn [Visual Novel](https://faultlore.com/blah/text-hates-you/#terminology)! If you want cliff notes, the combo of fonts, emojis, different direction languages (right-to-left and left-to-right), as well as rotating your fonts, leads to a tangle of tradeoffs with no clear winner.

## Layout

Whatever way you pick, you're going to need a way to layout your widgets. 
At best you'll implement the Cassowary style sheets, making auto-layout very decent. Albeit difficult to achieve some near pixel-perfect layouts. 
At worst you'll have a WidgetBag, that is brittle, pixel perfect, and causes frustration to anyone working on it.

## Immediate or retained

If you're doing toys or simpler games, immediate UI is fine. If your game looks like a stack of Excel files, retained is almost a must. Why? Because immediate draws every element on every frame. Even those that didn't change. 

On the plus side, this isn't an exclusive OR, but then you have to write both stacks.

## Localization, Accessibility, etc.

The moment Unicode was created, there was a sound of thousand GUI programmers crying in unison. Not only does it lead to [Font rendering]({{% ref "#font-rendering" %}}) but it also causes issues when localizing cross languages.

And I don't mean just replacing text using regex and an Excel sheet. I mean dynamic strings. Stuff like: 

> You have 1 unread email.

This requires some Unicode utilities like [CLDR](https://cldr.unicode.org/) (Common Locale Data Repository). 

## Where are my widgets?

Yeah, you're going to need a ton of widgets. People love convenience. And there is a world of difference between having a color picker come preinstalled and having to make it yourself. And color picker isn't even that challenging to make, but every widget that ships is one less you have to write yourself or download from a shady, ad-ridden site.

## Where is my debugger?

Ok, so you got Font rendering, Layout, Unicode, Localization, Accessibility, Internationalization, Widgets, the whole shebang. But you now need to create a Widget that doesn't exist. And predictably, it breaks. So now, you need to debug it. How do you debug it? Ok, so you need some inspector, or a way to stop your script execution to inspect it, or just visualize it somehow.  

# Part 3: The 900 lb. Gorilla in the Room—Chromium

All the previous discussion ignores the huge screaming gorilla in the room. Competition. And by competition I mean Chromium.

## Yeah, your GUI is great, but we are a CSS shop

You've made it! Your GUI toolkit has hit the shelves, metaphorically. Now you just need to get someone to get your app. And the latest AppFactory has picked your GUI, and... They switched to Chromium. When asked about it, they respond that their designers only know CSS and LLMs weren't of any help. Which dovetails nicely into.

## Problem 22 - Catch 22

To get many Widgets/Extensions, you need people to develop for your UI toolkit over competitors, including Chromium. But to get people to use your UI toolkit, you need people to at least have all the widgets and features that Chromium has. Also, JS has been around forever, so LLMs are good at outputting webpage slop.

Pretty much same goes for documentation, you need people using it, to get best tutorials, 

# Part 4: The Infinite Ramble

So to answer [The question]({{% ref "#the-question" %}}). A language isn't good cross platform GUI because it isn't backed by several mega corps expressly for the purpose of making UIs. You have decades of hardware and software backwards-compatibility to look forward to, platforms hostile to your existence, users expecting localizations, widgets, scripting, debugging, visual editor, documentation, etc. Not to mention the 

> [!cite] 
> Electron isn't the best cross platform GUI, but it's so far the least awful one.
>   --- Ygg01, 01/07/2026

And make no mistake, I've missed a lot of the finer details, but still each of these subpoints could be its own ramble.
