---
title: "The Per Rewrite Diaries: Day 46"
date: 2020-03-17T06:30:00-04:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I start working on some input view oddities.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## Two Steps Forward, One Step Back

Papercuts are the little scratches you get as you plow headlong through the prickly underbrush of implementing features.

One such papercut that I sorted out this morning was a weird `UIToolbar` [iOS 13 bug] requiring me to set a size on the input accessory view's frame lest I get that annoying "unable simultaneously satisfy constraints" warning. Previously, you could instantiate the toolbar with a simple

```
let inputAccessoryView = UIToolbar()
```

but now you'll need to do something like:

```
let inputAccessoryView = UIToolbar(frame: CGRect(x: 0, y: 0, width: 100, height: 100)).
```

I'm running into a weird bug, however, where the input view changes size on _some_ devices between my custom calculator keyboard and the picker view:

!["Screenshots from an iPhone 8 and iPhone 11 showing relative differences in input view height"](/images/2020-03-17/input-view-bug.png)

Notice how those input views are the same size on an iPhone 8 (on the left), for example, but _very_ different on an iPhone 11 (on the right).

I'll dig into this some more tomorrow.

[iOS 13 bug]: https://forums.developer.apple.com/thread/121474#384249