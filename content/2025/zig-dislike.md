---
title: Why I don't like Zig
draft: false
date: 2025-08-26
tags:
  - zig
  - coloring
  - programming-language
  - design
  - comptime
---
# I don't like Zig  
  
*Preface*: I don't like Zig. I tried liking it, but it never clicked for me. It may have been spite from one too many [[rust-nirvana-fallacy|Rust vs. Zig debates (feat. Nirvana fallacy)]].   
Or the weird little syntax constructs like `.{}` (it's weird, but I could get used to it).   
Or how it changes from version to version (again, it's v1.0).   
Or the subpar error quality (I'm spoiled by IDEs and Rust's `cargo check`).  
But I have more profound problems with it.  
  
# No privacy  
  
Zig has no private fields in structs (it does have private structs or methods). I honestly find this insistence of having everything public all the time a bit baffling. Good encapsulation is necessary to maintain invariants[^1]. If everything is public, whatever invariant maintaining code might as well not exist.   
  
## Reasoning for the lack of privacy   
The reasoning for that can be found in [this comment by Andrew Kelley](https://github.com/ziglang/zig/issues/9909#issuecomment-942686366). His reasoning can be summed as:  
  
 1.  "accessors are an anti pattern"  
  2. "name fields carefully and leave them as part of the public API, carefully documenting what they do."  
  3. "(public field in question)… being public also allows proper abstractions to be built, if the ones provided are not enough."  
  4. "private fields is an addition to the language, making it more complicated and requiring the language to answer a bunch of questions that otherwise would not exist."  
  
I'm not impressed by the arguments provided. Argument #1 is basically saying since Java abuses accessors, you shouldn't use it. Ok, Java also abuses function and class names by having `reallyLongAndMeaninglessNamesThatDontDoMuch` and `AbstractFactoryProxySingletonFactory`? Should we limit function names and struct names to 10 characters to avoid misuse?  
  
Similarly, argument #2 falls flat on its face. If a document was enough to stop people from doing something wrong, Unix would be a paradise, and C would see no UB [^2] in practice. But that's not the world we observe.  
  
Argument #3 is an argument in that example, not an argument in general.   
  
As for #4, yeah, adding private fields would complicate the compiler, but now we allow complications to live everywhere else. And people have started or will start to invent their conventions (`__private_field_do_not_touch_or_i_will_kill_you`) or hacks.  
  
  
# Leaky `comptime`  
  
But it gets even worse. I was interested in understanding the `comptime`. It's a very lauded feature, and with good reason. It seems to offer a seamless transition between generic and non-generic code. While investigating how they achieved it and what the trade-offs are, I found the [Zig-style generics are not well-suited for most languages](https://typesanitizer.com/blog/zig-generics.html) article (please give it a read; it should take around 19-23 min.). After reading it, I'm not so sure Rust needs to really embrace `comptime`.  
  
## A semi-practical demonstration  
Let's say we write the following library (as of Zig ~`0.15.1`). It has no values and returns a random value somehow. A true black box.  
  
```zig  
// Circa Zig 0.15  
fn give_rand() u8 {  
    return 42; // WHOOPS!! Classic XKCD#221 style mistake
}  
```  
  
We publish the library on ziggurat.io[^3]. After a few days we get a call from a security researcher that recognized that the random function is not *really* random. He tells us that if we don't fix it, we'll be the next https://xkcd.com/221/.  
  
Afraid of Internet fame, we write the following function to fix it:  
```zig  
// Finally fixed!  
fn give_rand() u8 {  
    var seed: u64 = undefined;    
    std.posix.getrandom(std.mem.asBytes(&seed)) catch |err| {
		std.debug.print("Failed to get random seed: {}\n", .{err});        
		return 43;    
	};   
	return @intCast(seed & 0xFF);
}  
```  
  
All is well, right? The security researcher is not too happy, but it's at least returning random results now. Phew! We can finally rest easy... except now we get a call from angry dependents. We broke their code. Huh?!?  
  
They send you a minimal example:  
```zig  
fn main() {  
    const random = comptime fixed_rand_u8();    
    std.debug.print("Hello, {}!\n", .{random});
}  
```  
  
Wait. What? We didn't declare the function to be `comptime`. The dependency just assumed it was. Welcome to the wonderful world of [[hyrums-law|Hyrum's law]]. You now have two solutions. Break backwards compatibility, yank your library, and publish a new major version -OR- never fix the bug. Both are equally enticing.  
  
## Leaky, shmeaky, who cares?  
People that care about backward compatibility. I.e., your dependencies. But not you. You are a cool rebel. You program in Zig and drive a motorcycle you built yourself from used parts. You probably don't need to bother with the rest of this article.  
  
Is he gone? Good.   
  
So if you care about backwards compatibility, you now can't call any function you either don't have full source control over, or you have to be extremely paranoid that your function doesn't incidentally trigger the `comptime` heuristics[^5]. I guess that's one way to minimize dependencies. Make everyone paranoid about everyone else's code. It's brilliant, in a dark way.  
  
But this seems similar. A set of functions that can't be called from other functions if you want tight guarantees and that can easily be called from within each other but not outside?  
  
## Did Zig just reinvent function coloring? With invisible colors?  
  
The question here is honest. Are `comptime` just red functions[^6] in disguise? Let's have a look through the definition.  
  
| Definition                                                        | `comptime` | `async` |  
| ----------------------------------------------------------------- | ---------- | ------- |  
| Every function is of one color.                                    | ✅          | ✅       |  
| The way you call your color depends on the function.               | ✅*         | ✅       |  
| You can only call red functions from within another red function. | ✅          | ✅       |  
| Red functions are painful to call.                                 | ❓          | ✅       |  
| Some core library functions are red.                              | ✅          | ✅       |  
1. Does every function have a color? - Yes. Each function has a `comptime`-ness, even if it's auto-determined by the compiler.  
2. Does the way you call your function depend on the color of the function? - Since `comptime` is auto-determined, it's difficult to get a grasp. But if you call a `const x = comptime my_non_comptime_fn()` on a non-`comptime` function, you do get a compilation error. Hence an asterisk.  
3. Does the "you can only call red functions from within red functions" rule apply? - Yes. Strangely enough, if you have a non-`comptime` function in your function, and it's being used, the function loses its `comptime`-ness.  
4. Ok, but are the red functions painful to call? - Here it's flipped. Rather than making `comptime` harder to call from `comptime`, the inverse is true. You can't call non-`comptime` functions using `comptime`.  
5. Yes. Many core library functions are `comptime`.  
  
Is this a coloring problem? I'm not sure. Rationally, it seems very close to it. On a gut level, this feels like coloring with red and blue markers that only show up under UV light. Even the behavior of the function is viral, but in a leaky, silent way.  
  
And to clarify, this isn't unknown; in the link to GitHub I mentioned, Andrew Kelley mentioned they leak `comptime` behavior. 
> [!cite] Andrewrk on Oct 13 2021
> There are many properties that can be "leaked" into the public interface of an API. For example: size/alignment of a struct, performance characteristics of a given function implementation, whether a function can be executed at comptime, existence/non-existence of declarations, and more

It just seems the way the Zig people roll.  
  
## Trying to solve it  
But more than that, leaky details will not mesh well with a [SemVer](https://semver.org/) package manager like NPM or Cargo. How could you even solve this problem in a way that's acceptable for all sides? I'm going to assume that you making function `comptime` or not via documentation is pointless. No one RTFMs. So with that in mind, how would you fix it in a way that prevents the SemVer hazard?   
  
### `must_comptime` annotation 
Imagine for a second we add an annotation[^7] or something to Zig. This function is always `comptime` and it's an error to call it from non-`comptime` contexts.  
  
```zig  
@must_comptime  
fn always_comptime() u8 {  
    return 42;
}  
```  
  
Then we call it from a main without it - it will error.  


```zig  
fn main() {  
	const random = always_comptime(); // ERROR: Missing comptime call!
	std.debug.print("Hello, {}!\n", .{random});
}  
```
  
However, the moment we make it, two things happen.   
1. We can't use non-`comptime` in `comptime`. It's how code behaves today.  
2. We just made `comptime` painful to call. We completed the circuit; it is now definitely a coloring problem.  
  
### `noncomptime` annotation  
Ok, the last part wasn't a huge success. Can we go the opposite way? What if we made functions that weren't supposed to be `comptime` special?[^8] There are two sub-options to make `@noncomptime` work. One could be if the code detects it's `comptime` it errors, S  
  
```zig  
@noncomptime  
fn never_comptime() u8 {  
    var seed: u64 = undefined;    
    std.posix.getrandom(std.mem.asBytes(&seed)) catch |err| {
	    std.debug.print("Failed to get random seed: {}\n", .{err});
	    return 43;    
	};    
	return @intCast(seed & 0xFF);
}  
  
@noncomptime  
fn never_gonna_let_you_comptime() u8 {  
    return 42;
}  
  
fn main() {  
	const random = comptime never_comptime(); // ERROR: Found comptime call!
	std.debug.print("Hello, {}!\n", .{random});
 }  
```  
  
Wait. If we remove `@noncomptime`, isn't that how they behave today? Well, not quite, since a function:  
```zig  
@noncomptime  
fn never_gonna_let_you_comptime() u8 {  
    return 42;
}  
```  
  
It would now be considered  non-`comptime`. Perhaps this is the better way. However, at this point, you also notice something. We essentially are starting to bifurcate the syntax. The seamless syntax has started to fray. We're making `noncomptime` functions harder to call from `comptime`. In essence, we're turning `noncomptime` red.   
  
# In conclusion  
  
I could survive the syntax. And I could theoretically survive the changes. I could probably survive the error messages. But `comptime` and lack of privacy modifiers just don't gel with me. It bothers me on an instinctual and conscious level.  
   
  
[^1]: In an imperative language. Perhaps a proof a la Ada Spark would fare better.  

[^2]: Because everyone carefully read and memorized Appendix J.2 of the C standard, see https://www.dii.uchile.cl/~daespino/files/Iso_C_1999_definition.pdf pg 490  

[^3]: Future NPM competitor written for and in Zig.

[^4]: I won't pretend to claim to know how Zig does this, so I call it a heuristic. As a bonus round, what happens if the way this mechanism changes from Zig version to Zig version? Do some functions lose `comptime`?  

[^5]: As per [What color is your function](https://journal.stuffwithstuff.com/2015/02/01/what-color-is-your-function/).

[^6]: Note: I don't know how Zig does code annotations, so I'm using Java's convention. `@annotations` will do something to code when it's evaluated. Or when docs are generated.

[^7]: There are two sub-options when making `@noncomptime`. One is making it so that if the function is `comptime` is used in `@noncomptime` block the compiler throws an error that the function is `comptime`. The second option is to just ensure that function can't be called with `comptime` even if contents is `comptime`. In my example I'm only considering the latter approach, since it's closer to how Zig behaves now.
