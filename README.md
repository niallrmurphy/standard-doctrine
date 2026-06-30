# Software Reliability: what we believe
## Documenting the underlying intuitions we have about software.

Like every human community, software and systems engineers have dogma about their profession. Though the field’s self-image is grounded in reasoning, measurement, and experimentation, any experienced practitioner knows that our best practices are often entirely without evidential support. Consider the fact that Dijkstra’s famous letter [Goto Considered Harmful](https://doi.org/10.1145/362929.362947), immensely important for language design in the period, was written before there was any meaningful testing. More recently, the useful and highly influential [Design Patterns](https://refactoring.guru/design-patterns) are, in essence, documented practitioner anecdotes unsupported by a larger articulated theory, and [test-driven development](https://newsletter.kentbeck.com/p/canon-tdd), equally if not more influential, has a very well articulated rationale but again [no strong evidence](https://ieeexplore.ieee.org/document/6197200) it’s meaningfully better than any other approach.

Those examples are relatively benign: for example, Dijkstra was definitely directionally correct. But there are many others with less evidence and more rhetoric, and their overall effects have been less benign. One example is the legend of the 10x engineer, usually attributed to [a 1968 paper](https://dl.acm.org/doi/10.1145/362851.362858) with relatively weak experimental evidence, which nonetheless continued to influence hiring for the next 50+ years. Another is the [endless array](https://en.wikipedia.org/wiki/Microservices) of [productivity frameworks](https://agilemanifesto.org), most claiming universal applicability, none of which achieved the order-of-magnitude improvement Brooks’s [No Silver Bullet](https://dl.acm.org/doi/10.1109/MC.1987.1663532) said was impossible, despite immeasurable money, time, and effort being spent.

As a field, we [rarely reflect deeply](https://bsky.app/profile/hazelweakly.me/post/3maqbzkdkyk2h) on how we come to believe things about software and systems engineering. Software is a complex, mostly social act; it would be hard to measure at the best of times, and evidence-based software engineering does not attract as much funding or attention as software engineering itself. But we don’t do ourselves favours with how we approach it. Sometimes our threshold for believing something is high, sometimes low; sometimes an appeal to authority is sufficient, sometimes not. Sociologically, our understanding of the interplay of knowledge transfer, community, and authority is not very sophisticated, and it is hard to see this improving soon.

It seems that it’s really our intuition about how software is best constructed that is the real determinant of what we do in the industry. Which makes it all the stranger that so little about this intuition is actually written down.

## The current context ##

You might well say, so what? Software engineers can often combine ideological fierceness towards their own approaches with a relaxed pragmatism about whether or not there’s a good reason to believe them. As long as software is getting delivered, few care to question why. We are happy to live unexamined lives.

But my background in reliability engineering, combined with what AI is doing to the practice of development, gives me a more specific concern. We have based a substantial portion of how we work on our intuitive understanding of how software is constructed, but in particular, on how software fails. AI SWE is rapidly changing the underlying dynamics, and breaking our mental models. Awareness of this is coursing through LinkedIn thinkpieces at an alarming rate, but what I don’t see very much of is an articulation of what we already believe, in a way that we could more rigorously examine.

For example, there is a lot of evidence suggesting that software engineering throughput or productivity, broadly measured, has substantially increased: the Faros AI Engineering report, 2026 shows that per-developer epics completed rose 66%, and task throughput was up 33.7%, but the incidents per month also rose 57.9%, while the incidents per PR rose an astonishing 242.7%. Though currently not well-evidenced, I also find it striking that the industry is full of anecdotes that the quality of AI-generated software may be a substantial improvement on human-generated software, though the anecdotes aren’t usually specific about how that quality is measured.

Regardless, it seems clear that our model of how software construction works must change: to labour the analogy, we have found a way to pour more fuel in the top of the engine - the wheels are turning faster and the handling is improving, but the car also explodes messily a lot more than we expect. We don’t yet understand why. Distinguishing whether it is “more lines of code means more incidents” or “more lines of AI-generated code means more incidents” or even “our intuition as-is cannot handle this new approach safely” is a crucial exercise which we cannot currently perform.

## The standard model ##

Here is our take on the majority position on bugs, outages, and how software development typically happens \- happened? \- in the pre-AI SWE era.

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

Lorin Hochstein has talked about P7, but it is internalised in many software engineers that a large codebase is in dynamic tension, and intervening in one place can have unexpected effects in other places.

**P9:** *Outages and software failures happen because of bugs. They also happen because of things that aren’t directly attributable to bugs.*

**P10:** *A smaller change to software is more likely to result in working software than a larger change.*

P9 relates to things which it would be problematic to simply call bugs \- for example, traffic storms, for which the “bug” is “did not anticipate unexpected popularity of consumer good X”.

P10 has been continually demonstrated \- e.g. see small batch sizes in *Accelerate* and the DevOps movement, and is also reasonable common sense, though we can’t think of a formal support for it.

## Conclusion ##

Much of the work of software reliability, and associated conceptions of risk, rely on these beliefs in some way. Please note that we do not use the term “beliefs” in a way that might imply the propositions are in fact untrue \- most people in industry have hard-won experience of precisely those propositions, which is why they are widely held, and it is precisely because of them that we have practices such as change freezes, release gates, and canarying. But it is notable that not much of them have been explicitly written down; we continue to rely on these beliefs to drive policy, but it is not clear how we would revise them, or in which way we would.

That in itself is a pressing problem, since the advent of AI SWE has arguably broken several of these assumptions, and further ones will follow.
