---
title: "The Per Rewrite Diary: Day 11"
date: 2020-02-11T17:30:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, we're doing a little planning for the coming week.
tags: ["per", "per rewrite diary", "ios"]
---

> This post is part of a series about rewriting my iOS app, [Per]. Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here].

## Thinking Clearly

I'm only able to do a little bit today because _reasons_, but there's enough time to tackle the first of [yesterday's goals] for the week: a way to clear the product list.

First, the `Product` model needs a way to clear the list — Swift's `removeAll()` array method to the rescue!

```
mutating func clearItems() {
    _products.removeAll()
}
```

Then, I need a way to trigger this from the UI — and another `UIBarButtonItem` next to the add-product button should do it:

```
clearListBarButtonItem = UIBarButtonItem(barButtonSystemItem: .trash,
                                                      target: self,
                                                      action: #selector(handleClearListBarButtonItemTapped))
```

The `handleClearListBarButtonItemTapped` action is then pretty straightforward (remember from [day 7] that the `contentViewController` is the one that wraps the table view):

```
@objc func handleClearListBarButtonItemTapped(sender: UIBarButtonItem) {
    contentViewController.productList.clearItems()
    contentViewController.loadView()
}
```

That's about it! I went back and sprinkled some `.isEnabled` here and there to make sure that the button is disabled if there's nothing in the list, and that was enough to call this done.

Tomorrow, I'll start work on a form for entering product item details in the `ProductItemContentViewController`, which will have me diving into unfamiliar territory: creating layout in code.

[Per]: https://droppedbits.com/apps/per
[here]: /tags/per-rewrite-diary/
[yesterday's goals]: /post/per-diaries-day-10/
[day 7]: /post/per-diaries-day-7/