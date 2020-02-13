---
title: "The Per Rewrite Diary: Day 13"
date: 2020-02-13T07:30:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I start work on laying out the product detail view.
tags: ["per", "per rewrite diary", "ios"]
---

> This post is part of a series about rewriting my iOS app, [Per]. Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here].

## Stacking the Deck

[Yesterday] I threw together a little ASCII-rendered layout of nested stack views and mentioned that I'd start implementing it by putting the existing **Add** and **Cancel** buttons in a horizontal stack view. So the goal for today is to spike something like this in my `ProductDetailContentViewController`:

```
+==================+
||  ADD  || CANCEL|| <- Horizontal stack view
+==================+
```

Eventually, the goal is to refactor this out into context, container and content view controllers, but right now that's overthinking it. Remember, the goal of this rewrite is a "one small change per day" approach.

I noticed a UI bug, too: when the user taps the **Add** button, the action's completion block enables the clear-list button. This means that if you tap the add button to create the first product on the list, and cancel out of that action instead of adding something, that clear-list button _still_ gets enabled.

I can't add this logic to the completion block, though — it'll check the length of the product list before the product item actually gets added, rather than after.

Completion blocks are insidious in this way. You think they're going to run, y'know, _after completion_, but always ask yourself: after completion _of what_?

We want the logic for enabling the clear-list button to be triggered by _actually adding an item_, so we can instead move it to the `add(item:)` delegate method in the product list context view controller. This gets called by the product detail content view controller when the user taps the **Add** button, which is all we need!

Good! So today I started implementing the layout by putting the product detail view's buttons in a horizontal stack view, and fixed a little UI bug. Tomorrow, I'm going to continue by adding a vertical stack view for all the labels and text fields.

[Per]: https://droppedbits.com/apps/per
[here]: /tags/per-rewrite-diary/
[Yesterday]: /post/per-diaries-day-12/