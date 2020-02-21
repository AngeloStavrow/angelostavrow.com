---
title: "The Per Rewrite Diaries: Day 21"
date: 2020-02-21T09:00:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I refactor some UI code out of viewDidLoad().
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## Refactoring layout code

I'm a little short on time today, so I'll grab some low-hanging fruit for a quick win. [Yesterday], I wrote that "the `ProductDetailContentViewController` is kinda sloppy — some of its UI is from an embedded custom view, and some of it is created in `viewDidLoad()`, which should _at least_ be refactored out into separate `setupView()` and `setupConstraints()` methods."

That's easy enough. I'm marking those setup functions as `private`, and taking advantage of `NSLayoutConstraint.activate()` for setting up and activating constraints. I don't think it's necessarily _less_ readable than chaining `.isActive = true` to the end of each constraint call, but as an array I can fold the code in the editor so it takes up less space, which I something I take advantage of.

So that's one papercut fixed! Just over a week to go before the end of the month; let's see how close I can get to the current shipping features! Tomorrow I'll tackle one of the `FIXME` issues — specifically, making `ProductList.add()` throw on trying to add a mismatched-unit `ProductItem`.

[Yesterday]: /post/per-diaries-day-20