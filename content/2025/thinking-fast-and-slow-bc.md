---
title: Thinking fast and slow about borrow checker
draft: false
date: 2025-05-26
tags:
  - rust
  - borrow-checker
  - learning
---

# Intro 

So lately I've been watching [Veritasium's video about AI and learning](https://www.youtube.com/watch?v=0xS68sl2D70). While talking about two types of systems and how we learn, a thought struck me. I don't notice the borrow checker anymore.

And this isn't the first time this happened to me. 

# Tale of two indexes

When I was still in high school, where we got our first taste of actual programming, I used to almost exclusively use Pascal. There for two years we used Pascal to solve problems, no C in sight.

To those that don't see an issue here, the problem is that Pascal has arbitrary index starts, while C only starts at `0`. In essence I had to go from `1`-based indices[^1] to `0`-based indices. Let me tell you, I still remember that it caused me headaches often.

But then, one day, I realized, I no longer struggle.

# What was that about borrow checker?

Yeah. I'll get to it.

When I started working on some hobby Rust projects around 2014, I must admit I didn't like borrow checker. I mean it was refusing so many correct programs. And you couldn't even elide it. Horrible. This stuff worked so easily in Java. And now I can't even express my favorite anti-pattern in safe Rust - [Singleton](https://en.wikipedia.org/wiki/Singleton_pattern)[^2].

But then, one day, I realized, I no longer fight borrow checker. Well, ok sometimes, but I fuck up by one indices, so it's a fair comparison.

# Fast thinking vs Slow thinking

In the above link, Derek explains about the two systems. I'll give the cliff notes version, but the talk is great and only 45min long[^3].

There are two systems[^4] :
- System One (aka Fast system) - fast, intuitive, gives easy and cheap answers, often makes errors.
- System Two (aka Slow system) - slow, effortful, painful to use, very limited, makes less errors.

So what happened when learning something that's hard? Like say unlearning several years of `1`-based indexing, and learning a new set of pattern, or say unlearning decades of not tracking lifetimes and now having to grapple with them?

Well first. You're going to struggle. A lot. Then as you slowly grind away, you make stupid mistakes, you go back to drawing board, you learn and after some time (I don't know if it was months and years) you just know it. On a very intuitive level. That means your knowledge has been finally internalized. And you finally know ~~kung-fu~~ borrow checker.

# So what can we learn?

One. An experienced Rust user will suffer from the [Curse of Knowledge](https://en.wikipedia.org/wiki/Curse_of_knowledge), regarding borrow checker, since they rarely fight it. On the upside, it means at some point you will stop "fighting it".

Two, an inexperienced Rust user is a gold mine of how his mental model clashes with borrow checkers. They clash with it quite often.  That said, just because they are clashing with it, doesn't mean they know how to fix it. Honestly people that work with lots of new Rust people are probably your best way of gouging the newbie mental model.

Three. The sooner learning happens, the easier will borrow checker seem to the developer. Easy doesn't mean simple, it means familiar.

Four. Lifetime elision's *might* be doing people a disservice, by making it easy at the start, you're going to make figuring them out much harder later. Think of it, as using a calculator to calculate multiplication, only for later to be forced to do it by hand, when numbers get too big. I'm not advocating for removing lifetime elision, but perhaps teaching it might require doing it by hand at first.

---

[^1]: Because most algorithm work in math work start at 1, and rarely 0.

[^2]:  I mean, you can do it with `unsafe`, but it's not as easy as in Java.

[^3]:Excluding the QnA section that starts at 45min.

[^4]: While the underlying research leaves much to be desired. I have noticed that performing actions constantly allows us to internalize them. From complex movements, to patterns of code.
