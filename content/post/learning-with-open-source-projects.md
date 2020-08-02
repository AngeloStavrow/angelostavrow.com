---
title: "Learning With Open Source Projects"
date: 2020-08-02T10:45:00-04:00
draft: false
summary: "Social learning as a (community) service: I've been wanting to learn more about building macOS apps, so what better way to learn than contribute to a great project like NetNewsWire?"
tags: ["projects", "swift", "netnewswire"]
---

_**UPDATED:** I opened my [original PR] against the wrong branch, so I've fixed the links here to point to the new PR._

I've been wanting to learn more about building macOS apps. SwiftUI multiplatform apps are a great way forward in building something quickly, but you'll often still need to drop into AppKit and UIKit to solve things that SwiftUI just doesn't address (yet). So, for someone that's got some experience building iOS apps, what's a good way to learn?

I _could_ just build a bunch of toy projects on my own. I read through [Hacking with macOS] which taught me a tonne. But for myself, the best learning is [social learning] — by observing and interacting with others.

I've been lurking on the [Slack group] for [NetNewsWire] for months, and once I felt comfortable with the way the community worked together, I asked to tackle a [first issue]. Nothing especially complex for my level of experience, but that would have me digging through the codebase to figure out how things work, and learn how to make the changes for a framework (AppKit) that I'm less familiar with.

I _like_ digging through a codebase and teasing apart the way it works. The advantage of poking at NetNewsWire is that it's a nicely-organized project, clearly structured for long-term maintainability by anyone willing to contribute. 

_(If we're ever able to sit and enjoy an afternoon restorative together, ask me about having to port a 20 kLOC, uncommented, single-file code project to another language.)_

_(On second thought — don't.)_

It's one thing to learn how the Notifications API works by changing the way a feature works. But there's a lot to learn by looking at how others structure and organize their codebase, set up their build steps, share code between iOS and macOS targets, &cet. You just don't get that depth from simple how-to tutorials. Those tutorials have their place in the learning process, and they're a wonderful community resource! But you have to go deeper to go from "my first massive view controller app" to building a complex software project.

Another nice thing about contributing to NetNewsWire is that the community is wonderful. One thing I appreciated was that I was trusted to tackle the problem myself, rather than be told (more or less) what to do as a first-time contributor. Of course, any questions I had were answered, and whenever I asked for feedback it was honest and helpful. That's a testament to the community [Brent Simmons] has built around the project.

And so, I opened my [first pull request] this morning. In service of [showing messy work] (and because it's good Git hygiene, IMO), the work's broken up into a dozen fairly small commits. You can see some of the [comments I removed] after figuring out what exactly happens when you fetch the user's notification settings, for example.

I'm looking forward to the team's feedback on my proposed changes, and I'm looking forward to contributing more to the project!

[original PR]: https://github.com/Ranchero-Software/NetNewsWire/pull/2314
[Hacking with macOS]: https://www.hackingwithswift.com/store/hacking-with-macos
[social learning]: https://en.m.wikipedia.org/wiki/Social_learning_theory
[Slack group]: https://netnewswire.slack.com/
[NetNewsWire]: https://ranchero.com/netnewswire/
[first issue]: https://github.com/Ranchero-Software/NetNewsWire/issues/2058
[Brent Simmons]: https://inessential.com/
[first pull request]: https://github.com/Ranchero-Software/NetNewsWire/pull/2315
[showing messy work]: https://angelostavrow.com/post/messy-journeys-and-scruffy-role-models/
[comments I removed]: https://github.com/Ranchero-Software/NetNewsWire/pull/2315/commits/619ddc6ea2aa16647b6bd5c7b6355ff31bb7e347
