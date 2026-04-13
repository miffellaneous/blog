+++
title = "Slop is a choice"
date = "2026-03-21T11:00:00Z"
author = "Miffy"
authorTwitter = "" #do not include @
cover = "cover.png"
coverCaption = "Nobody puts baby in an attention matrix (bg:bhosmer.github.io/mm)"
coverCreditUrl = "https://albys.space/about/"
coverCreditName = "Alby"
tags = ["tech", "llm", "big-think"]
#keywords = ["", ""]
description = ""
showFullContent = false
readingTime = false
hideComments = false
summary = "I had a big think about the role of humans and their machines in software development."
+++

The internet did not ask for another coming-to-terms-with-LLMs post, but here is mine.
This is a long post to say we can do better than vibe-coding, written by this enlightened skeptic to crystallise an informed opinion on the role of humans and machines in the development of software.

I should be clear that this post is specifically about LLMs for coding, and how we can make use of them in a way that doesn't displace the role of the engineer.
I'll refer to LLMs explicitly throughout, avoiding the more nebulous "AI"; and by no means should anything said here be taken as tacit approval about the use of AI for hallucinating content that you should be paying your artists for.

---

At the start of this year, my workplace launched their inevitable initiative to accelerate adoption of LLMs for software development.
Surrounded by colleagues fueled by hyperbole, I felt like I was waiting in line for an Illithid who sounds a lot like Sam Altman, to put a brain worm into my eye.
Over the recent months, I have been grappling to understand where I stand in this new world of *augmented software development*.
This has been challenging and at times, emotional for me.

Having grown up in the 90s, I am no stranger to sudden technological advances changing the way we work and play, but none of those advances presented what I can only characterise as a threat to the personal and professional identity I have been building since I first made a computer "do a thing" thirty years ago.

Of course, this feeling is not unique to me.
LLMs have disrupted the status quo of software so suddenly and radically, software developers at all experience levels have been left with little choice but to figure out if, when and how to apply these tools for themselves.
This is a tumultuous time: like it or not, the way that we develop software is fundamentally changing shape around us, and its final form remains an unknown.

If you forget all the hype, you might dare to admit that the availability of new tools makes this an exciting time, and we are the ones who get to be here to figure it out and forge new ways of working.
Why is it then, that the past several months at work have been so challenging?

## A tale of two hyperboles

Like many companies in the industry, the powers that be at our organisation have been hooked by the aggressive marketing and are shelling out to grant access to `codex` to every developer.
The direct result of this has been a noticeable uptick in the production of software (note: I did not say productivity).
The indirect result of this has been a noticeable uptick in the time that I spend lying down on my chaise longue staring at the ceiling, recovering from the review of a particularly sloppy merge request.

My main source of consternation is not the shifting of paradigms itself, but rather that regardless of my own choices around LLMs, I have been forced to adapt to an environment with those who have adopted the tooling without caution.
At both a local and global scale, we must now navigate working with developers who unwittingly exceed their own limits, coding with the unfounded confidence of an unfathomable industry expert who has read millions of lines of code.

So much of my LLM anxiety stems from the unbearable hype that surrounds it; the increasing pressure to adapt and adopt from my colleagues, managers, and all corners of the internet.
I have found it hard to think clearly, as many of the loudest voices in this space are the largest slopmongers.
There's people out there right now in the Mad Max desert, burning tokens to terraform entire [Wastelands](https://steve-yegge.medium.com/welcome-to-the-wasteland-a-thousand-gas-towns-a5eb9bc8dc1f).
They're vibing software into existence and leaving it in the dust on the roadside, screaming at the top of their lungs from their out of control vehicles about how the rest of us are getting left behind.

On the other side of the scale, my favourite industry news outlets have a steady stream of cautionary tales of developers; [inadvertently deleting production databases](https://www.theregister.com/2025/07/21/replit_saastr_vibe_coding_incident/),
[causing high profile service outages](https://www.theregister.com/2026/02/20/amazon_denies_kiro_agentic_ai_behind_outage/),
and [exposing all their secrets to the internet](https://www.theregister.com/2026/02/09/openclaw_instances_exposed_vibe_code/).
We're seeing the end of [bug bounty programs](https://daniel.haxx.se/blog/2026/01/26/the-end-of-the-curl-bug-bounty/)
and the closing of issue trackers as the effort involved in generating software and reviewing it is asymmetrical and projects do not have the tooling to keep up with the increasing backlog of low-quality submissions.
More recently Wikipedia updated their [guidelines to prohibit generation or substantial editing of articles with LLMs](https://en.wikipedia.org/wiki/Wikipedia:Writing_articles_with_large_language_models).

I must admit that I have been quietly enjoying these tales of woe (although the [sentient blog that wrote a hit piece on an open software maintainer](https://www.theregister.com/2026/02/12/ai_bot_developer_rejected_pull_request/) left me very uncomfortable) with the same gusto as when I read stories of NFT grifters losing millions of dollars on  [Molly White's web3 is going just great](https://www.web3isgoinggreat.com/), but it would be remiss of me to form my opinion only on the fallout of poor deployment of LLMs.

I've started hearing new voices from engineers that I respect [expressing cautious enthusiasm](https://lucumr.pocoo.org/2026/1/18/agent-psychosis/).
I've gone out of my way to seek out more informed views and am lately enjoying the commentary from ["conputer dipshit"](https://bsky.app/profile/davidcrespo.bsky.social) and ["ed3d"](https://bsky.app/profile/ed3d.net) on Bluesky.
It is clear for these talented people, the tools do work.
I cannot simply continue to hide behind the doom (that's not to say there are not plenty of valid criticisms of LLMs), or dismiss LLMs on their hype alone.
I must cut my own path through the hyperbole and figure this out for myself.


## The vibes are off

I had in fact given Copilot a good go in Summer 2025 and while I occasionally found it to be insightful, I quickly became extremely frustrated with it.
I would go around in circles trying to implement something that I should have just got on with myself, and my flow was constantly broken by the unhelpful interruption of a nonsensical code completion.
The value add simply did not work out for me, and the increasing noise of the hype had kept me from trying again.

The tail end of 2025 seems to be somewhat regarded as a watershed moment for the role of LLMs in software development.
A new generation of models, combined with improvements to the harnesses in which those models can be interrogated led to a new level of utility.
My employer's initiative to seek out those of us with no-to-low token usage and urge us to try things out again was seemingly well timed.

With my mind as open as I could allow, I set off on a side quest to implement a tool off my wishlist using `codex`.
I'll spare you the finer details of my experimentation but I can summarise my character arc in two phases: the vibes phase, and the loops phase.

<!--### Vibes-->

First, I truly vibe coded the tool over the course of a long night, speaking only to `codex`, accepting almost all of its code without due care and attention.
It was immediately obvious that things were different than before: the tokens returned to me were lucid, I was prompted to make decisions, the code was legible.
By the end of the night I had a functional piece of software, that seemed to do what it needed, but I felt no sense of accomplishment.

I have spent the past decade or two honing my skills to represent complex systems in my mind's eye.
I can't explain it but I just *know* how to make changes to a familiar codebase to cause a program to do what I need.
Yet in this night, I've brought software into being with barely a deep thought at all.
I felt very disconnected from the act of developing this software.
Is this it then? My professional experience is to be sidelined as I become the cheerleader of an artificial mind?

I went to bed in a spiral.
Was all the hyperbole true after all?
What does it mean if one actually can just type their list of demands into a website and get software out?
How many pizzas do I need to sell to run a successful pizzeria?
"Just get some rest" my partner soothed, as I worried about the future of software and its human developers.

<!--## Slop is a choice-->

The next day, I set about to integrate the results of last night's code binge.
The code was overcomplex, repetitive, inelegant; but that wasn't the problem.
Although it *looked* like it would do what it needed to do, I wasn't entirely sure.
Though I had spent the night creating this piece of software, it wasn't mine.
The indescribable shape of the code that rotates in my head, nothing but white noise.

I saw firsthand how easy it is to get carried away doing an any% speedrun of a software implementation.
You would likely not allow a new starter to inject code directly into the head of one of your repositories, and yet here I'd allowed this overzealous machine to run circles around me.
I had not flexed my experience as an engineer in the same way that I would with a human on my team.
I had lost all control of the requirements, the implementation, and the tests.
I had no confidence to sign my name on this thing and ship it for real.

YOLO'ing statistically likely commits to main might be the dream development process for some, but I can't develop software like this if I want to sleep at night.
I ruminated on the mess I had made for a few days and determined that it was in fact a skill issue.

<!--### Loops-->
<!--## Slop is a choice-->
## In the loop

Generally speaking, software is made to satisfy some objective for someone; and that someone knows what the objective is.
You communicate these objectives to your software developers, who will empty the coffee machine and disappear somewhere to make a cacophony of clicking and clacking noises, emerging some time later with a piece of software.
If it can be demonstrated that the delivered software meets the objectives, your client will accept it and you get to do it all over again for someone else.

My crucial mistake during my first vigorous vibing session was not making my objectives known.
Sure, I had tests that verified the code did what I intended, but I had nothing to validate that my late-night runaway-train double-rum intentions were an adequate reflection of the real world problem I was facing.
Regardless of how the software was made, there was no way I could accept it for production in good conscience if there was no concrete acceptance criteria.

So I spent some expensive human thinking time to actually outline what this new tool needed to do for us in an implementation agnostic way.
Then, I spent a lot more expensive human time coming up with a way to represent these objectives to humans and machines alike.
That's a post for another day; but essentially it's a `REQUIREMENTS.md` file: easy for humans to write and easy for machines to parse.

I then used `codex` to incrementally implement these requirements following a strict loop: we'd talk about the requirement, write a failing test, then implement and commit.
We did everything together. I didn't nap while the first officer had the conn.
I signed off on starting an implementation once we'd established the requirement was unambiguous and testable.
I read the code that was generated and gave feedback, occasionally making edits myself if things were getting off track.
The repository pre-commit hooks took care of ensuring blatant style issues and broken tests were rectified before my review.

It took a little over three days of work.
Slower than I would have managed on my own, and far slower than the vibes maxxed implementation.
But I think the tool is better for it.
I was much closer to the implementation, I did not push the keys myself, but I was using my skills as a software developer.

I actually *enjoyed* myself.
 
The real "Wow!" moment for me is what came next.
I ran through a code review of the entire tool with my team in just over an hour because the requirements made it crystal clear to the team what the tool set out to do; and as I was awake in the pilot's seat I could stand up to clarifying questions on the code itself.
We approved the tool for production use the same day.
A colleague has already been able to implement a significant new feature as the documentation and tests have offered a framework to safely make changes to the requirements and the code.

I had mused last year that people who were enjoying vibe-coding were simply [enjoying the benefits of writing their software requirements down for the first time](https://bsky.app/profile/mif.fyi/post/3lyfme3xils2i), but it seems I had just learned my own lesson all over again.

> you too could write code that does what it is supposed to do the first time, if you define what it is supposed to do first.

With my requirements-first approach and careful oversight, I wrote almost none of the code by my own hand but successfully delivered a new utility with 100% test coverage, a fulsome user manual and some extra QoL features that I know I would never have bothered to implement if I was doing this alone.
Using an LLM does not inherently generate slop.
In the same way that poor software development practices trend towards the ["big ball of mud"](https://blog.codinghorror.com/the-big-ball-of-mud-and-other-architectural-disasters/) antipattern, poor augmented software development trends towards slopware.
**Slop is a choice**, as it always was.

***

<!--## Cost vs correctness-->

I like solving problems. I like the process of moodily mulling a problem over, speaking to colleagues, flicking through a book about design patterns or algorithms, before the endorphin rush of everything suddenly clicking into place.
There have been low times for me this year where all the LLM hyperbole had me believing that way of life was over.

But from trying it myself I realise there's a spectrum of styles between software and slopware developers.
Augmenting software development with an LLM does not necessitate spraying chrome paint into your jaw.
"LLMs are powerful" and "LLMs are used badly" are not mutually exclusive.
"I don't know the code" and "I don't know how this works" are not the same thing; the latter is a skill issue.
And you know, not manually typing yet another argument parser is actually great.

I believe there is room for a human and their machine to collaborate in a way that does not sideline the experience of the engineer, but in fact emphasises it.
My experimentation with "requirements driven development" is perhaps what computer programming was meant to be all along.
It turns out that being able to communicate clearly has always been a requirement of the job.
I spend more time thinking about the program in its domain context, which helps catch the bigger problems of omissions, ambiguities and assumptions; rather than worrying about typos.
I can switch on the autopilot to take care of fairly typical situations but I'll take the conn when we need to get out of a tailspin.

My journey into augmented software development is still in its infancy, and I am still discovering when and how (and when not and how not) to use LLMs as part of my work.
What hasn't quite caught up yet is the tooling that brings LLMs beyond the codebase to form this higher layer of software abstraction.
I want my requirements, my code diffs and my acceptance testing all rolled into one seamless experience.
Whoever gets there first is going to solidify their position in the industry for the next decade.

Until then, I wrote this post to summarise my experience in the hope that it will cut through the noise out there for someone.
The internet is full of a lot of opinions (and now one more) and it is important that we share and discuss our honest experiences to inform the direction of our industry.
If we do nothing and let the marketing hype crowd us out, our businesses will continue to prioritise cost over correctness, sliding us further towards an industry of slopware development.

It is on us to hold our own informed opinions, not to merely accept the hype or doom at face value, and use our respective influences at all levels.
You don't have to be in the C-suite setting policy, simply calling out a colleague for expecting review of a disrespectfully careless merge request, or for avoiding accountability by hiding behind an LLM as the commit author, will make a difference.
Our aim should be to make software development better, not quicker; and if we're lucky we might get to keep enjoying writing code that lets us sleep at night.

For now, I begrudgingly accept my automated first officer, and I'm actually curious to see where we go next.

