---
title: "The Per Rewrite Diary: Day 10"
date: 2020-02-10T08:15:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, we're doing a little planning for the coming week.
tags: ["per", "per rewrite diary", "ios"]
---

> This post is part of a series about rewriting my iOS app, [Per]. Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here].

## Thinking ahead

I mentioned [yesterday] that I'd work on understanding a Simulator bug that I'm seeing when dismissing a detail view controller today. I'm stepping back from writing code today, though, to get a feeling for where I am and what I hope to accomplish this week.

I'm really happy with progress so far! I've gone from an empty Xcode project to something with a couple of views and some models and the skeleton of a useful app, more or less. I'm learning (I mean, aren't we all, always?) and have gotten a lot done in about an hour a day for the last week and a half. I want to keep up this hour-a-day cadence, but I think it's worth thinking about how to best approach that hour too.

The app isn't yet _usable_, so my goal for this week is to have it in installed on my iPhone and using it for doing _at least_ dimensionless price comparisons. That shouldn't be too hard to do, as I'm almost there — just need to add some labels and text fields to the detail view controller.

BUT! There's also a complete lack of tests, so I'd like to slow down the forward momentum and add some coverage for unit tests. I'm rusty with testing a Swift app, so that's something I'll spend time learning as part of my Per time this week, too.

Here are the goals for this week:

### A way to clear the product list

Right now, I can add products to the list for comparison, but the only way to clear them out is to force-quit the app (or wait for it to be terminated in the background). A way to clear the list is important.

### A product detail view that lets me enter quantity, units, and price

Right now the product detail view only adds a randomly-generated `ProductItem` and that's... not very helpful, so I want to add a basic form that lets me enter the price, quantity, and units for a product. I don't want to worry about dealing with units yet, though, so I'm going to default to dimensionless units.

### Unit tests for the product and product-list models

At a minimum, having solid testing coverage for your models is pretty important, and I will look at testing the view controllers later as they're likely to be refactored _a lot_ between now and the end of this project. I've never really explored UI testing on iOS, so that's going to be something that I worry about later.

More to come tomorrow!

[Per]: https://droppedbits.com/apps/per
[here]: /tags/per-rewrite-diary/
[yesterday]: /post/per-diaries-day-9/