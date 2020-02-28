---
title: "The Per Rewrite Diaries: Day 27"
date: 2020-02-27T17:00:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I start work on the automatic unit conversion feature's UI.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## Slow down and sleep

To test what unit type (if any) the list should be limited to, I changed the private `_unitType` property in the `ProductList` to

```
private(set) var unitType: Unit?
```

This value is set when the first `ProductItem` is added to the list. Then I can add it as a property with the same access level to the `ProductDetailContentViewController`:

```
protocol ProductDetailContentViewControllerDelegate: AnyObject {
    var numberOfProductItems: Int { get }
    // Other protocol stuff
}
```

This exposes it to the view controller's subviews, but here's something I was perplexed by. In the view controller's `viewDidLoad()` method, I (via a call to a `setupViews()` method) instantiate a subview, and set its delegate. But when I was trying to access that delegate from within the subview's initializer, it would return `nil`. It took me longer than I care to admit to realize that the delegate isn't initialized until _after_ my subview is:

```
//... other viewDidLoad() stuff in the view controller
productDetailFormView = new ProductDetailFormView(frame: .zero);
productDetailFormView.delegate = self
```

In retrospect, this is obvious. You've got to wait until the view's initializer does its thing, and then the view's delegate is set — so don't try to access it in the view's initializer.

What's fascinating to me is that —while I know all this— it was incomprehensible why this was happening while I was in the thick of programming, until I gave up and took a break. Why? Probably because I had a lousy night's sleep, and was rushing to get something done early this morning.

The takeaway: get a good night's sleep, and don't rush your work.

Tomorrow, I continue working on this.