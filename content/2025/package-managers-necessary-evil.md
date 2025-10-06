---
title: Why package managers are a necessary evil
draft: true
date: 2025-09-09
tags:
  - programming
  - package-managers
---
# Intro 

This blog post is inspired by the article [Package Managers are Evil](https://www.gingerbill.org/article/2025/09/08/package-managers-are-evil/), which is an edited version of discussions between Ginger Bill and a few others in The Standup podcast. 

I'll try to keep this post as simple and as language agnostic as possible. I'll also avoid stuff that wasn't written by Ginger Bill or stuff that's just a semantics dispute, like the Dependency Hell bit.

# The Premise

> [!cite]
> ## What do package managers do?
> Package managers download packages from a repositories, handles the dependencies and tries to fix them, and then it downloads its dependencies, and its dependencies, and its dependencies… and you can probably see where my criticism is going.
> This is the **automation of dependency hell**.
>...
> That’s my general criticism: the unnecessary automation.
> --- [Package Managers are evil | What do package manager do](https://www.gingerbill.org/article/2025/09/08/package-managers-are-evil/#what-do-package-managers-do)


> [!cite]
> ## Unbacked high trust
>People rarely, if ever, vet their code, especially third-party code. Most people assume random code off the internet _works_. This is a societal issue where programmers are very high trusting in a place where you should have the least amount of trust possible. To put it bluntly, a lot of programmers come from a highly developed countries which are in general high trust societies, and then they apply that to the rest of their online world.
> --- [Package Managers are evil | Unbacked high trust](https://www.gingerbill.org/article/2025/09/08/package-managers-are-evil/#unbacked-high-trust)

Basically, people are too trusting, and that plus automation is a recipe for disaster. However. That's not the full story. Maybe people from first-world countries are trusting. But I'm not trusting. Why am I putting in dependencies? Why not rewrite it from scratch?

# Faulty assumption


> [!cite]
>Those who cannot remember the past are condemned to repeat it.
> --- George Santayana[\[1\]](https://en.wikiquote.org/wiki/George_Santayana)

Here is where I think it fails. Programmers aren't too trustworthy; they should know by now everyone on the internet is actually two poodles in a trench coat until proven otherwise. In fact, the problem is different. To borrow a Terry Pratchett Discworld terminology[^1], they are "yzal" (or lazy backwards); they are so extremely lazy, they go through the other side and work overtime just to make sure they get to be lazy.

How does this fit with our observation? Well, the thing is, Odin isn't the only language that started without a package manager. In fact, there are several that started without a package manager while having a concept of a library/package/mod/jar, whatever you wish to call it.

## Java

Started around 1996 [^2], had no package manager to speak of, but had the concept of packages. What it did was create `.jar` files. Managing `.jar` files manually was such a pain in the neck that if someone didn't invent Maven in July 2004[^3] I would have made my own. It also got Gradle in 2012 [^4]

## JavaScript

JavaScript was made in 10 days[^5] in 1995, so it of course has no concept of package/module/whatever. After the release, its first package manager had its debut around 2011[^6].

# False sense of security

But ok. Let's say that the BDFL has prevented foul package managers from forming. What do we get for it? Some sort of trust?

## Trusting packages

Well, we might still need new drivers to connect to a `FutureDB4009`. So how do we do that in our system? Well, we vendor our dependency into our project.

Wow, there has been a vulnerability that affects us; the email sent even told me to urgently upgrade it!

Hmm. Why does the URL for the package end in `.xyz`? Why is my CPU pegged at 100%? Where is my 0.02 ETH?

Oh. I've been pwned. Lessons learned.

## Security by Computer-Based Torture

To no surprise, this doesn't really prevent any [XZ Utils](https://en.wikipedia.org/wiki/XZ_Utils_backdoor) or [phishing attacks](https://www.aikido.dev/blog/npm-debug-and-chalk-packages-compromised). Maybe that wasn't the point of it, but [Unbacked High Trust](https://www.gingerbill.org/article/2025/09/08/package-managers-are-evil/#unbacked-high-trust) seems to talk about trust in the context of security. At the end of the day, you'll have to trust *someone* outside of the things you shipped with.

Last time I checked, Odin had no support for AWS/SQLite/Redis/Aerospike, etc. in the vendor package. But even if they add it, the maintainers will then have the burden of maintaining their language + graphics stacks (Direct3D, Vulkan, Metal...) + random cloud vendors + all the databases + all possible format parsers + audio/video coders/decoders + all the cryptographic stuff + all HTTP version + all Unicode tooling + all the UI for all platform + ... + ad infinitum. That's a nigh impossible task, but maybe they can get every relevant vendor for every possible field to invest time in Odin.

# Conclusion

> [!cite]
> To the yzal go the spoils!
> --- me 2025

So what can we learn from these examples? If your language packager is a pain, the yzal developers will eventually invent their own package managers (even if your language has no concept of a package) and make your once pristine and dependency-free code into a nightmare of a thousand and one dependencies.

No language is immune; some may be more resistant than others, but they will all succumb to the yzal devs [^7]. Hell, even C++ got `vcpkg` and co.

## How do we solve this issue?

That's the neat part. You can't. The package manager is bad, but it's the least bad of all other options. Manual dependency check is just tedium that doesn't buy you much security and is a prime target for automation. Granted, automatic dependency resolution is a space hog. No one is going to argue that.

You can beef up your standard library, but it usually turns into a pick your poison situation:
1. We have to live with our errors, aka *"Standard is where packages go to die"* - backwards compatibility.
2. We have to split the ecosystem, aka *"Let's have Python 2 vs Python 3 situation"* - breaking backwards compatibility.
3. We choose a bit of breakage, aka *"My edition doesn't work with yours"* - which makes some errors unfixable while causing minor schisms in the ecosystem.

Package managers are an inevitable evil, but not all packages should be created to minimize dependencies!

If you write a top secret `.gov` space laser, you shouldn't be using NPM, and if you're writing a toy to be tossed into the trash next week, you don't need to structure it as if it's a joint NASA-CIA-MI6 project.

You have to be realistic about what the threat actor is and how to handle your risks; without that, discussing security is a fool's errand that falls into [[nirvana-fallacy|Nirvana fallacy]]. I.e., a vault door won't prevent your home from multi-dimensional aliens or a nuke. But they can stop robbers.

Eventually package managers will adapt and survive or perish in the onslaught of new threats. But that's what's always been happening.

---

[^1]: The term in Discworld is that often you can be extremely something, you break through the other side, and end up quite similar to the opposite. This is a concept known as *[[Enantiodromia]]*, or conversion into the opposite.

[^2]: JDK 1.0  Release date 23rd of January 1996 https://en.wikipedia.org/wiki/Java_version_history

[^3]: According to https://en.wikipedia.org/wiki/Apache_Maven Version 1 was released in July 2004.

[^4]: Stable 1.0 release in Jun 2012, according to https://gradle.org/releases/

[^5]: > The "first version" of JavaScript did in fact take ten days. The exact dates aren't confirmed, but Brendan Eich recalls it being [May 6-15, 1995](https://www.quora.com/In-which-10-days-of-May-did-Brendan-Eich-write-JavaScript-Mocha-in-1995). But this was only a minimal prototype ("Mocha") for internal demonstration. https://buttondown.com/hillelwayne/archive/did-brendan-eich-really-make-javascript-in-10-days/

[^6]: v1.0.1 of `npm` released Apr 30, 2011, https://github.com/npm/npm/releases/tag/v1.0.1

[^7]: Or completely disappear. No one built a COBOL package manager to my knowledge.
