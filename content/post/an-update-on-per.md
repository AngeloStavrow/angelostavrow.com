---
title: "An Update on Per"
date: 2020-07-07T14:00:00-04:00
draft: false
summary: Work on the next release of Per continues unabated! Here's an update on how that's going.
tags: ["per", "per rewrite diary", "ios"]
---

Just because I closed out the [Per rewrite journal] in late March, doesn't mean I'm not still working on the [app]! Here's what's been happening.

## Minimum Dogfoodable Product — 2020-04-30

At the end of April, I closed out last issue in the _Minimum Dogfoodable Product_ (MDP) milestone. This represents the first internal release that gets the rewritten app to feature parity with v1.2, the currently-shipping version [in the App Store]. At that point, Christina and I replaced the shipping version of the app on our iPhones to start, as the milestone name implies, dogfooding it.

## Minimum Launchable Product — 2020-07-06

With that done, I moved on to issues in the _Minimum Launchable Product_ (MLP) milestone. This release represents an internal beta that collects bugfixes from using the MDP along with a set of launch-blocking features to be implemented before releasing the first external beta. That work wrapped up yesterday, so now we're on to the _Minimum Viable Product_ (MVP)!

## Minimum Viable Product — TBA

The MVP, once work is complete, will be the first externally-available (via [Testflight]) release of the app, and includes all features planned for the public release. The plumbing and framing was completed in the MLP work, so now —new bugs notwithstanding— it's about UI and UX polish. I've been building Per with mostly-stock UIKit components and typography, such that I get things like accessibility and dark-mode support (mostly) for free. But as Christina's pointed out, there's lots of little UX improvements that can be made, and a little bit of UI/personality would be nice, too.

## So, New Features?

You've already seen [one] of the new features: increasing the number of compared products beyond two.

New features will, for the most part, live behind a small in-app purchase. Because the current version is free, I don't want anyone to lose out on existing functionality; free users will get to compare up to three products, with paying customers being able to compare unlimited products (or some technologically-constrained number that approximates "unlimited").

The in-app purchase will also unlock something I've _personally_ been wishing for: a way to save a product for later. Like many, I know where to go to get the best price on certain staples, but I don't always remember exactly what that price or quantity is. So, when I come across some sale on that product at another store, I can't be sure if it's a better deal or not. 🤔

Well, with the next release of Per, you'll be able to save that information for later. It's been a fun feature to implement, including setting up persistence with SQLite/[FMDB], but it still needs some more work to get it where I'd like. I hope you'll find it useful!

Will there be other features? Yes, eventually, but probably not for this release. So, as always, more TK.

<!-- Links -->
[Per rewrite journal]: https://angelostavrow.com/tags/per-rewrite-diary/
[app]: https://droppedbits.com/apps/per/
[in the App Store]: https://apps.apple.com/us/app/per-shop-smart-save-money/id922693504?ls=1
[Testflight]: https://testflight.apple.com
[one]: https://angelostavrow.com/post/per-diaries-day-1/
[FMDB]: https://github.com/ccgus/fmdb