# Software Reliability: what we believe

_This is part one of Project Cinnte (KIN-TEH), an effort to look at some foundational questions in software reliability._

## Introduction

Being professionally interested in questions of software reliability, we have often wondered whether or not we can
find good models to help explain, characterise, or predict failure. The industry doesn't have much in the way of such models today.
What we have is practitioner's intuition, and it's often the best thing we do have, but it's hard to know
if it's the best thing we _could_ have.

In the act of reflecting on this question, we realised that there isn't a good source that documents precisely what
that intuition actually says. The rest of this document is an attempt to do that.

## The standard doctrine ##

Here is our take on the "standard doctrine" on bugs, outages, and how software development typically happens \- happened? \- in the pre-AI SWE era.

We label the following propositions for ease of reference, together with their commentary.

**P0:** *Bugs happen as a function of the sustained intellectual effort underpinning writing software. The longer the effort or the more intense the effort required, the larger the chance of incorrect software being written.*

Software engineers believe this because they model the act of writing software as having a problem to deal with, thinking of a way to solve that problem, and writing down the solution. The tougher the problem, the more chance you get it wrong; the larger the problem, the more chance you get it wrong.

**P1:** *Other things being equal, the bigger or more complex the software, the more bugs there are.* 

**P2:** *Other things being equal, the newer the software, the more bugs there are.*

These look like simple corollaries of the above, though there is more complexity there when you tease it apart. When we say “bigger” above, that follows clearly from P0, but “more complex” also brings in scenarios where the problem might not require a large mass of software to solve, but might be algorithmically complex, have a very large number of business domain exceptions, have uncommon constraints (e.g. latency), or similar. P2 in particular introduces a new idea: newly made software has not encountered reality yet, and will have more bugs than software which *has* encountered reality.

**P3:** *Bugs can occur for a variety of reasons, and on a number of different levels. The category of “bug” itself is quite wide, and includes at least typos, inaccurately writing what you want, accurately writing what you want but it turns out it’s not the right thing to want, incorrectly capturing your own requirements, incorrectly understanding other people’s requirements, working with third-party software or services with different models of the problem domain, compiler errors, microcode errors, firmware errors, or cosmic rays.*

The current authors have had all of these happen to them at one stage or another. There is probably some kind of long-tail distribution of the frequency of occurrence, but the structure doesn’t matter for most of these observations.

**P4:** *Bugs can occur for a wide variety of reasons, and any sufficiently large code-base will have latent bugs \- ones for which the triggering conditions have not yet been met.*

**P5:** *It is effectively impossible to eliminate bugs entirely from a sufficiently large code-base.*

**P6:** *Fixing bugs is easier in an unchanging codebase.*

**P7:** *Fixing a bug can often introduce a bug.*

Software engineers often combine optimism and pessimism in the same act, and sometimes in the same few seconds. For P4, absent formal derivation (which itself can have problems), most engineers believe obvious bugs are caught relatively early, but latent ones can wait a long while for the “right” combination of input, data, environment, *et cetera* to be triggered. Some of the recent Mythos-related security bugs are from code several decades old.

P5 is believed to be both a practical and a theoretical limit. It’s always hard to get time to do bug-fixes, so there is a practical limit to how much we can “get permission” to do that work, but there is also a combinatorial explosion element to both tracking down the bugs, their mutual interaction, and solving them without introducing more problems. So it is possible to spend a finite amount of effort and eliminate some current bugs, or some category of future bugs, but it is not possible to spend a finite amount of effort and eliminate all bugs.

P6 is a simple engineering observation in the sense that an equation with fewer variables is easier to solve than an equation with more. Unchanging data (as opposed to unchanging code) also makes it easier.

[Lorin Hochstein](https://lorinhochstein.org/) has talked about P7, but it is internalised in many software engineers that a large codebase is in dynamic tension, and intervening in one place can have unexpected effects in other places.

**P8:** *Outages and software failures happen because of bugs. They also happen because of things that aren’t directly attributable to bugs.*

**P9:** *A smaller change to software is more likely to result in working software than a larger change.*

P8 relates to things which it would be problematic to simply call bugs \- for example, traffic storms, for which the “bug” is “did not anticipate unexpected popularity of consumer good X”.

P9 has been continually demonstrated \- e.g. see small batch sizes in *Accelerate* and the DevOps movement, and is also reasonable common sense, though we can’t think of a formal support for it.

## Conclusion ##

Much of the work of software reliability, and associated conceptions of risk, rely on these beliefs in some way. Please note that we do not use the term “beliefs” in a way that might imply the propositions are in fact untrue. Most people in industry have hard-won experience of precisely those propositions, which is why they are widely held. It is also precisely because of them that we have practices such as change freezes, release gates, and canarying. But it is notable that not much of them have been explicitly written down: we continue to rely on these beliefs to drive policy, but it is not yet clear how we would revise them, or in which way we would.

The advent of AI SWE has arguably broken several of these assumptions, and further ones will follow.
