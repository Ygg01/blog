---
title: FDD (Fad Driven Development)
draft: true
date: 2026-07-01
tags:
  - fad
  - odin
  - programming
  - development
---
>[!tip]
> So I think what happened is computing has turned into pop culture and the universities are not helping in general, at least not in the US.
> 
> So, Cicero---anybody know a good Cicero quote having to do with the present and past? Let's check your classical education here. So, you know who Cicero was. He was one of those old Roman guys.
> 
> So, Cicero once wrote: 'He who knows only his own generation remains forever a child.'
>
> [Programming and Scaling](https://www.youtube.com/watch?v=YyIQKBzIuBY) (Alan Kay, 2011)

It's always good to start an article with a quote. It demonstrates the level of smug necessary to read the rest of the post.

Jokes aside, I've been thinking a lot about this quote and a similar one:

>[!quote]
> Those who cannot remember the past are condemned to repeat it.
>
> [George Santayana](https://en.wikiquote.org/wiki/George_Santayana)

This resonates with me recently-ish.
# Forgetting the Lessons of Dependency Hell

It seems people often forget the painful lessons of previous languages to determine that they will vanquish the [Demon of Dependency Management](/blog/2025/package-managers-necessary-evil) by, wait for it...

Returning us to the Ye Olden Dependency Hell.

That's like solving cancer by going back to leeches. And maybe I'm wrong. There is a near-zero chance leeches will be the thing that *"solves cancer for real this time."™* Or, most likely, someone will reinvent the NPM for the ten billionth time, and Odin will end up with a package manager. Time will tell, but I'm betting on an NPM-like package manager IFF[^1] Odin gets major traction.

# Rewrite it in Rust

It may shock some readers, since I do like Rust, but the Rewrite it in Rust (RIIR) crowd has always been a bit of a sore spot for me. I don't mind if you are rewriting a critical component in Rust because your old one is dying under the load. But I assume you do your due diligence. 

Otherwise you'll end up with the [Cascade of Attention Deficit Teenagers (CADT)](https://www.jwz.org/doc/cadt.html) group. Where each new member is rewriting your component in Rust, then Zig, then Jai, then Odin, then Circle, then C[^2], then Kotlin, then Java, then ... 



---

[^1]: If and only if. It requires major traction to get someone crazy enough to write an NPM clone in their spare time.

[^2]: Because C is fashionable again, baby!
