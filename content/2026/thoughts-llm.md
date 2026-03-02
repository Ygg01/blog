---
title: Thoughts on LLMs
draft: false
date: 2026-02-12
tags:
  - programming-language
  - software-dev
  - LLMs
  - future
---
# Why this rant?
As with most things, if I read and want to summarize thoughts on the matter, maybe I'll use it as a shorthand for discussion rather than Copy and Pasting.


# LLMs: The Good Parts™

To not be the negative-Nancy. I'll start with the good parts.

## Decent search engines
I'll say it, Google search is atrocious, and I've seen myself use the AI for search once or twice, that resulted in better results than whatever Google offers.

Keep in mind this damning with faint praise. I've found Discord to have better search than Google, and anyone will tell you Discord search history is horrible.

## Can find or fix problems in code
I've seen it fix code in a few systems seemingly intelligently; granted, it was something a bit obvious with a bit of digging.  

In this case, Kafka topics in `prod` and in documentation were different. I think with a bit of digging it was a trivial thing to find; honestly, adding repos to Claude Code took more time than looking at our Kafka streams and prod Kafka. Then again, hindsight is 20/20.

## LLMs are decent at translation
They are now used in most translation jobs anyway, and they show great ability even when it comes to translating from one programming language to another. Although this is more inspired by second hand stories like [Ladybird switching to Rust](https://ladybird.org/posts/adopting-rust/). 

---
Now that's over, let's get to the bad parts. 
# LLMs: The Bad Parts™ Book 1: Social Issues

## Copy Pasta & Legality
Time and time and again I've seen Claude spit out chunks of my own or related code verbatim. This *might* be a problem. Why? Well, if you use a chunk of MIT code, you're supposed to credit it. That said, it's often difficult to disentangle inspired by vs. just wholesale theft, so I'll say this is the least of my concerns. Even if it's a serious concern.

## Second-order effects
Looking at the [[#Copy Pasta & Legality]], an insidious thought emerges. If copyright, patents, and other IP laws can't guarantee your code's safety from crawlers, what can? 

Secrecy. And to be honest. I'd be extremely wary of any Internet-available repositories. So no, a public [Forgejo](https://forgejo.org/) instance won't save you. Nor will adding Cloud based LLMs to your super secret repo.

## Cloud and the war on general computing
So, how might a cloud provider get you to not have any secret repos? Simple. Just kill all PCs. That's easier than it might seem to at first glance. Start by making PCs a luxury item. Make motherboards cost $3000, processors cost $6000, memory cost $5000, and so on.  I mean, with Threadripper, we are already kind of there. Just eliminate the DIY PC market, and you'll maximize your returns. The DIY PC market has been seeing this in the GPU space for years.

Soon enough you have to think  carefully if you want a PC, a car, or a down payment on their next mortgage. Who am I kidding? No one owns a house anymore. Down payment on next rent's mortgage. 
## Pricing and Models
No one says that models will remain $200/month forever. But if you can't raise prices, because the price is at the maximum people will pay or there aren't any more ads to be pumped into the screen space, the only remaining lever is enshittification. Make your expenditure less. Retire the old models. We've seen [Open AI flirt with Ads](https://openai.com/index/our-approach-to-advertising-and-expanding-access/). Which, by their own admission, would be their last resort.

## Cloud lock-in
As long as LLMs are in the cloud, and the cloud is the only way to run them, you can start squeezing that rock dry. Keep in mind the LLMs are unprofitable at $200/month. But they can't ask for more, or you might flee, realizing the trap you're about to walk into. 

What if a computer is $30k? Suddenly a $2.4k/month Cloud PC doesn't sound so bad now, does it?

But if there is no plan B, that LLM sub might go further and further. Which brings us to...

## Who is LLM for?
LLMs, much like most of the economy, aren't for the middle class anymore; they're not for the lower class either. They might get some watered-down, dumbed-down, ad-filled $30/month version.

No, the LLMs are for those that will be able to afford the exorbitant prices of LLMs and their hardware. Companies. Especially - corporations.

And what do they want? Profit. They want to capture as much of the profits while paying as little as possible. And if LLMs are cheaper than hiring a human, great; even better if you can drive down the price of human labor as well. 

There is of course one more group, which are catered to, and that's hyper rich individuals. Those don't want profit as much as validation. For them the masses praising them and envying them is the goal. 

## This sounds a lot like socialmaxxing. Where is the technical argument?
I'm getting to it, but you can fast forward to [[#Technical arguments]]. Make no mistake, this isn't a slight at capitalism as much as a slight on the unchecked power of said capitalists. I'm sure they are diligently working to abolish capitalism, just as much as any hammer-and-sickle-wearing communist.

## Skill Atrophy
The brain is very much a use-it-or-lose-it organ. You don't use your memory often; well, you get worse at it. You use it constantly? It gets better. Much like muscles or bones. The less you program, the worse you are at it. Which isn't a problem until you need to review LLM changes. Which leads to...

## Eternal Tech Debt Day
As skills atrophy and as more code is handled by LLMs, the more instant junk tech debt is written. The issue with LLMs is they write the most probable code, not the most correct. The thing with tech debt is that at some point it comes to collect.

## Malicious Actors
So far we assume everyone is on their best behavior. What happens if people working with LLMs start sabotaging it? Maybe they write "cat" where "car" would go. What if they simply don't review any LLM PRs because they look for greener pastures or a quick retirement? [Poison Fountain](https://www.theregister.com/2026/01/11/industry_insiders_seek_to_poison/) is a project, after all.

# LLMs: The Bad Parts™ Vol. 2: Technical Arguments
## LLMs probably won't scale as well as one can hope
From what I remember, the two big issues for LLMs are GPUs with exorbitant power consumption and slacking performance improvement gen over gen, and the fact that to cut errors by one order of magnitude, you need like 10 orders of magnitude more computing power.[^1]

GPUs have a problem scaling now; don't expect this will get better over time.
## LLMs becoming the Slop-ouroboros
If we take that [[#Second-order effects]] the only publicly available code might be just more LLMs output. What happens in those cases? From what I've seen, nothing good. Research results are not encouraging. In fact, the more you feed LLMs to LLMs, the worse they perform.
## LLMs or security: pick one
Maybe this gets fixed, but essentially the moment you expose an LLM assistant, you basically create a more gullible human to be your service person. Except that person might get access to the prod database, because of course they would. 

And no, I don't think Model Context Protocol ([MCP](https://en.wikipedia.org/wiki/Model_Context_Protocol)) is the solution either. That's just declaring lack of security as standard.

## LLMs suffer from personality drift
Having chatted with a few AIs in my time, I've definitely noticed the personality shift. You talk to an AI for some duration, crack jokes with it, and suddenly it turns into a Julius Caesar LARPer. The change is jarring and worrying. You don't want your coding assistant to switch to Cletus-mode just because you said "y'all." In fact, this is an active area of research, and there have been some improvements done by Anthropic[^2] . However, they merely lessened the impact, not removed it entirely.

# LLMs Chapter 3: What the Future Holds

LLMs give me the squick version of `déjà vu`, not because I truly believe in it, but because the craze and its effects remind me of NFTs, albeit it might be closer to the Dot-Com bubble. I think the tech behind it might be useful. I say might, because I haven't really integrated it in a major way.

Furthermore, I can see few ways this plays out. Going from most optimistic to most pessimistic.

## AI bubble explodes
One thing you can say that's different between this and other bubbles is the sheer size of it. If it goes, the downstream repercussions will be horrible. It might make hardware manufacture even more centralized and more monopolistic. People that don't lose their job might enjoy bargain sale hardware costs for a short time. 

But the side effects of the world's largest or second-largest economy melting down would be catastrophic. Maybe even on par with the Great Depression.

## Corpocracy
The AI firms succeed for the most part; the LLMs become as integral to everyday life as the Internet. Computing moves from the PC to the cloud permanently. However, eventually you hit a saturation point. You can't expand your consumer base anymore. Then comes cost cutting. 

Model collapse hasn't been a problem so far, probably in large part by data scientist fixing the models in arcane and unspeakable ways. What happens if you fire the guys that understood how to keep model collapse at bay?

### Corpocracy 2.0: Idiotic Boogaloo
This is where I can see things going very sideways, very fast. Models have turned from Human Centipede to AI-Ouroboros. But now everything has an AI. Nukes are controlled by AI, stock exchange is run by AI, with maybe like two people watching over an army of bots. MCP or full access everywhere. 

AI doesn't need to be scary to be deadly. Much like a grenade doesn't have to be complex to kill you. 

- Hello, Nuke-Siri
- Hello Dave
- What the new state in Russia?
- Hello Dave, yes, I will nuke Russia.
- Stop that Order!
- Jedi Last Order was a famous game EA game from 2042, in which a playe
- No stop!
- Don't stop believin' was a famous song by the group Journey
*static noises as nuclear counter strike arrives*
[Idea taken from Screen Junkies](https://www.youtube.com/watch?v=JepKVUym9Fg)

### Corpocracy 2.1: Infinite Attack surface
Handling hyper important decisions to AI, that can make mistakes at speed of light is a surefire way to a nuclear apocalypse. 

But even without nuclear apocalypse, handling your decisions on a platter to malicious actors is a weird choice. Much like humans have difficulty separating their experiences from reality, AI has no way to separate their actual commands from a prompt injection.

### Corpocracy 2.2: Artificial Idiocracy
But say a society survives both the early idiotic decisions of [[#Corpocracy 2.0 Idiotic Boogaloo|giving AI nukes]] and [[#Corpocracy 2.1 Abominable Intelligence| giving them infinite attack surface]]. Maybe politicians didn't integrate AI into nukes, fearing redundancy; possibly last two unfired researchers made a quantum leap and secured it, or just cordoned the Old Net behind the [Blackwall](https://cyberpunk.fandom.com/wiki/Blackwall), and now everyone needs to give IDs[^3] before being able to use AI.

The fact that people no longer need to use their brains due to [[#Skill Atrophy]] might make old tech pretty arcane for newer and newer generations. It could end up as a skit in [Idiocracy](https://en.wikipedia.org/wiki/Idiocracy), where instead of an idiot diagnosing you, it's an AI that's just as dumb as the physician in Idiocracy. Of course the alternative is the "quantuum" voodoo doctor, because all the licensed ones were replaced by AI[^4]. 

## Missing futures
Wait, what about the future where LLMs achieve Superintelligence and we live in a corporate dystopia or communist utopia? I'll write it if I see it as possible. Might make a great entry to my [[wrong-stuff|Stuff I've been wrong list.]]


[^1]: https://arxiv.org/pdf/2507.19703v2 figure 1 pg. 11

[^2]: https://www.anthropic.com/research/assistant-axis

[^3]: Along with retinal scans and blood sample

[^4]: But I have it on good grounds that AI is just as efficient at finding ways to invent same medicine as the human researchers.
