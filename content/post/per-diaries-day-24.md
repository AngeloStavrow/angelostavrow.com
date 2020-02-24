---
title: "The Per Rewrite Diaries: Day 24"
date: 2020-02-24T18:30:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I continue work on a custom table view cell.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## Design Thinking

There's not really much to report today. I spent the day thinking about how to best present information in each `ProductListCellView` so that it's easily understood; right now `ProductItem`s are sorted in the list by best-value, but there's no indication of _how much better_ the value is. I'm experimenting with some ideas and I think I've got an idea, though that'll mean adding something to the `ProductList` that stores either the best or worst value for your money, and updates it with each new product that's added.

Then, each cell could show how much worse (or better) it is than that baseline value. So I'll tackle that tomorrow.