---
title: "The Per Rewrite Diaries: Day 41"
date: 2020-03-12T06:31:00-04:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I expand the calculator keyboard.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## Untangling Stack Views

One thing I always have struggled with is visually reasoning about nested arrays — which, if you kinda squint and look at from the right angle, is very similar to what you've got with nested stack views.

The actual code to set up a stack view? Fairly straightforward, really. Apple did a nice job with this API. But untangling my own set up of the various horizontal and vertical stack views to create the keyboard was a bit... gross. I should have probably started by thinking about the top-level stack view in terms of columns rather than rows — blindly copy-pasting a solution from Stack Overflow often gets you to a solution while creating other problems, I guess.

At any rate, that's cleaned up now. I ended up exploring three options, and I'm not in love with any of them:

!["Three options of calculator keyboard layouts"](/images/2020-03-12/keyboard-options.png)

For now I'm going with the one on the far right, with the equals button in its own column.

(Yes, I'm testing with the iPhone 8 Simulator right now. I always tend to test with an older device's screen size, though I don't have a good reason for why — but I will have to take care to ensure the layout makes sense with the home bar indicator thing on the iPhone X/Xs/11.)

Tomorrow, I get back to the actual calculator functionality!