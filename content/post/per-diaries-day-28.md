---
title: "The Per Rewrite Diaries: Day 28"
date: 2020-02-28T05:34:04-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I continue work on the automatic unit conversion feature's UI.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## Two days to go

Okay, so tomorrow's the end of the month. I'd like to get to the point where automatic unit conversion is working, but I don't think I'll get there in two hours. One way or the other, I didn't get to the point where the app functionally recreates the features of the currently shipping version of Per, but that's okay! I made a _huge_ amount of progress and I'm really happy about that.

Today, I implemented the UI for picking the unit type when you add your first item to the product list. When the view controller sets the view's delegate, I have a `didSet` property observer that checks the delegate's `numberOfProductItems` property and, if there aren't any, inserts a `UISegmentedControl` where users can choose whether they want weight, volume, or dimensionless units. And it works really well!

Tomorrow, I'll set it up such that choosing a non-dimensionless option in the segmented control lets the user choose a weight or volume unit. Given that tomorrow is Saturday, I _may_ stretch it past the usual hour to get the unit-conversion feature sorted out. There's a lot less complexity this time around for several reasons, so I don't think it'll take too long.