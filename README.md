# Software Reliability: what we believe
## Documenting the underlying intuitions we have about software.

Like every human community, software and systems engineers have dogma about their profession. Though the field’s self-image is grounded in reasoning, measurement, and experimentation, any experienced practitioner knows that our best practices are often entirely without evidential support. Consider the fact that Dijkstra’s famous letter [Goto Considered Harmful](https://doi.org/10.1145/362929.362947), immensely important for language design in the period, was written before there was any meaningful testing. More recently, the useful and highly influential [Design Patterns](https://refactoring.guru/design-patterns) are, in essence, documented practitioner anecdotes unsupported by a larger articulated theory, and [test-driven development](https://newsletter.kentbeck.com/p/canon-tdd), equally if not more influential, has a very well articulated rationale but again [no strong evidence](https://ieeexplore.ieee.org/document/6197200) it’s meaningfully better than any other approach.

Those examples are relatively benign: for example, Dijkstra was definitely directionally correct. But there are many others with less evidence and more rhetoric, and their overall effects have been less benign. One example is the legend of the 10x engineer, usually attributed to [a 1968 paper](https://dl.acm.org/doi/10.1145/362851.362858) with relatively weak experimental evidence, which nonetheless continued to influence hiring for the next 50+ years. Another is the [endless array](https://en.wikipedia.org/wiki/Microservices) of [productivity frameworks](https://agilemanifesto.org), most claiming universal applicability, none of which achieved the order-of-magnitude improvement Brooks’s [No Silver Bullet](https://dl.acm.org/doi/10.1109/MC.1987.1663532) said was impossible, despite immeasurable money, time, and effort being spent.

As a field, we [rarely reflect deeply](https://bsky.app/profile/hazelweakly.me/post/3maqbzkdkyk2h) on how we come to believe things about software and systems engineering. Software is a complex, mostly social act; it would be hard to measure at the best of times, and evidence-based software engineering does not attract as much funding or attention as software engineering itself. But we don’t do ourselves favours with how we approach it. Sometimes our threshold for believing something is high, sometimes low; sometimes an appeal to authority is sufficient, sometimes not. Sociologically, our understanding of the interplay of knowledge transfer, community, and authority is not very sophisticated, and it is hard to see this improving soon.

It seems that it’s really our intuition about how software is best constructed that is the real determinant of what we do in the industry. Which makes it all the stranger that so little about this intuition is actually written down.

##The current context##

You might well say, so what? Software engineers can often combine ideological fierceness towards their own approaches with a relaxed pragmatism about whether or not there’s a good reason to believe them. As long as software is getting delivered, few care to question why. We are happy to live unexamined lives.

But my background in reliability engineering, combined with what AI is doing to the practice of development, gives me a more specific concern. We have based a substantial portion of how we work on our intuitive understanding of how software is constructed, but in particular, on how software fails. AI SWE is rapidly changing the underlying dynamics, and breaking our mental models. Awareness of this is coursing through LinkedIn thinkpieces at an alarming rate, but what I don’t see very much of is an articulation of what we already believe, in a way that we could more rigorously examine.

For example, there is a lot of evidence suggesting that software engineering throughput or productivity, broadly measured, has substantially increased: the Faros AI Engineering report, 2026 shows that per-developer epics completed rose 66%, and task throughput was up 33.7%, but the incidents per month also rose 57.9%, while the incidents per PR rose an astonishing 242.7%. Though currently not well-evidenced, I also find it striking that the industry is full of anecdotes that the quality of AI-generated software may be a substantial improvement on human-generated software, though the anecdotes aren’t usually specific about how that quality is measured.

Regardless, it seems clear that our model of how software construction works must change: to labour the analogy, we have found a way to pour more fuel in the top of the engine - the wheels are turning faster and the handling is improving, but the car also explodes messily a lot more than we expect. We don’t yet understand why. Distinguishing whether it is “more lines of code means more incidents” or “more lines of AI-generated code means more incidents” or even “our intuition as-is cannot handle this new approach safely” is a crucial exercise which we cannot currently perform.

##The standard model##

Here is our take as to the majority position on bugs, outages, and how software development typically happens - happened? - in the pre-AI SWE era.

We label the following propositions for ease of reference, and accompany them with commentary.

*P0:* There is a relationship between the size of a piece of software, its attendant complexity, and its bugs. It is approximate, and varies even within a codebase.

*C0:* This is based on the intuition that bugs happen as a function of sustained intellectual effort, and the longer the effort or the more intense the effort required, the larger the chance of incorrect software being written.

*P1:* Bugs can occur for a variety of reasons, and it is effectively impossible to eliminate them entirely from a sufficiently large code-base. You can sometimes make it easier by not changing anything else. Fixing a bug can often introduce a bug.

*C1:* “Effectively impossible” is believed to be both a practical and a theoretical limit by standard intuition.

For those otherwise unaware, the first few attempts to prove the Quicksort algorithm were correct in various ways, so it is not even fully convincing that a proof exists for an engineer to regard code as correct.

*P2:* It is possible to spend more effort on eliminating bugs and be successful - both existing ones and future ones. But you cannot spend a finite amount of effort and eliminate either all current ones or all future ones.

*P3:* Bugs can occur for a variety of reasons, and on a number of different levels. The category of “bug” itself is quite wide.

*P4:* Sample - non-exhaustive - bug categories include typos, inaccurately writing what you want, accurately writing what you want but it turns out it’s not the right thing to want, incorrectly capturing your own requirements, incorrectly understanding other people’s requirements, working with third-party software or services with different models of the problem domain, compiler errors, microcode errors, firmware errors, or cosmic rays.

*C4:* I’ve seen examples of all of those - the idea that


6: Outages and software failures happen because of bugs. They also happen because of things that aren’t directly attributable to bugs.
7: 
4: Though working on reliability can reduce outages, they can never be entirely eliminated. unless the software or its inputs and environment does not change.
The second group, proposition B, is related to a key practice of the DevOps and SRE communities:
8: A smaller change to software is more likely to result in working software than a larger change.
6: 

