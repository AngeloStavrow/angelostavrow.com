+++
date = "2016-05-20T12:00:00-04:00"
draft = false
title = "The Great HoneyJar Refactoring"
tags = [ "tghr" ]
aliases = [ "/blog/2016/5/20/the-great-honeyjar-refactoring" ]

+++

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Thinking about a series of posts where I take HoneyJar, the first iOS app I wrote, and refactor it out of its burning-dumpster-fire state.</p>&mdash; Angelo Stavrow (@AngeloStavrow) <a href="https://twitter.com/AngeloStavrow/status/732279293464678400">May 16, 2016</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Over the next few weeks, I've decided to write a series of posts about taking my first iOS app, [HoneyJar](https://droppedbits.com/honeyjar), and refactoring it from its current (terrible) state into something, well, less-terrible.

And I plan on doing it [_in public_](https://gitlab.com/AngeloStavrow/HoneyJar), too.

I'm calling this project **The Great HoneyJar Refactoring**.

HoneyJar is a future-value calculator. As explained on the product page, you set a payment amount, a rate of return, and a period of time, and HoneyJar will give you a scenario showing how your money will grow if the amount entered amount was

- a one-time lump-sum payment
- a series of equal weekly payments
- a series of equal monthly payments

I haven't really touched it after I released 1.0 save for a few minor updates. It could certainly use some love, as there's a whole lot of [technical debt](https://en.wikipedia.org/wiki/Technical_debt) going on in that codebase. Just off the top of my head, without having poked around the repository for months, I can list the following required fixes:

1. Refactor the main UIViewController into several lighter classes. The thing is a mess.
2. Along a similar line, convert the project to use Storyboards and Autolayout.
3. Add a test suite.
4. Add localization (I'll start with English and French, since I speak those languages pretty fluently).
5. Ensure accessibility.
6. Port the thing to Swift, or at least write new code in Swift.
7. General UI/UX improvements (e.g., make the result presentation clearer).
8. Drop support for devices running iOS 8 and before.

I'm going to call this version 2.0 and I've [opened a milestone in GitLab](https://gitlab.com/AngeloStavrow/HoneyJar/milestones/1) to track my progress. The associated issues will be added over the next day or two, so you can see how what I'm working on at any given time. I'll also be setting up CI and other (non-code) project settings.

So, I'm open-sourcing the (embarrassing) version as it exists right now&mdash;_please be nice_&mdash;and I plan on tackling one issue every couple of weeks or so, until the milestone is closed and 2.0 is released. Each time I close an issue, I'll write up a post on the whys and wherefores of the changes.

I haven't yet decided if I'll accept any merge requests during this project, but I certainly look forward to feedback on my work. To make it easier for people, to follow along, I will be mirroring the repository [on GitHub](https://github.com/DroppedBits/HoneyJar), too&mdash;GitLab makes it easy to push changes to a remote repo. GitLab is my `origin` of choice, though, thanks to the built-in continuous integration.

> **Update (2016-05-25):** I've moved the GitHub mirror from my personal account to the organization account, since it's Dropped Bits that's handling the App Store stuff.

More tk!
