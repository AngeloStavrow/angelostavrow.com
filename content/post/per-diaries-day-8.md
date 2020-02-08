---
title: "The Per Rewrite Diary: Day 8"
date: 2020-02-08T08:15:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, we're adding auto-sorting to our product list.
tags: ["per", "per rewrite diary", "ios"]
---

> This post is part of a series about rewriting my iOS app, [Per]. Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here].

## Sorting order

In the original version of Per, you entered the details for two products, and hit a big red "Compare" button to see which option gave you the most value for your money. That isn't going to be a great way to deal with more than two products — imagine having to initiate a comparison every time you add something?

Instead, the app can simply float the best option(s) to the top of the list every time you add a new product! That makes feedback really fast and really easy.

Turns out, it's really easy to implement this, because of a couple of things working together.

First, because I made the `Product` protocol conform to `Comparable` based on the `pricePerUnit` computed property on [day 3], all I really need to do is call `_products.sort()` whenever I add a new product to the `ProductList`. Remember that `sort()` sorts an array in place, whereas `sorted` returns a sorted array.

But I can't just add `_product.sort()` at the end of the `add(item: ProductItem)` method I wrote on [day 5] without creating a bug: if there's already something in the `_products` array, I (unnecessarily) `return` after appending a new `ProductItem`, so those early returns have to be removed.

Furthermore, I'd be changing the semantics of `add(item: ProductItem)` as well — this method no longer just _adds_ an item to the product list, it adds an item _and then sorts the list_. I am the only person working on this, so I could leave it as is, but I would rather be kind to my future self and create a new method that I call instead:

```
mutating func add(_ item: ProductItem, sort: Bool = false) {
    
    // The original add(item:) method implementation goes here

    if (sort) {
        _products.sort()
    }
}
```

Adding this new `sort` parameter with a default value of `false` to the `add(item:)` method means I don't have to change calls to the method _unless_ I want the list sorted.

I only call it in one place right now —the `handleAddProductBarButtonItemTapped` action in the product list context view controller, from [yesterday]— and it _should_ sort the list when called there, because we want to sort the list before updating the product list table view:

```
self.contentViewController.productList.add(createRandomProduct(), sort: true)
```

And that's it! We can now add (randomly-generated) items to the product list and they'll sort themselves automatically.

Tomorrow feels like a good day to start working on the UI for adding an _actual_ product's details, so that I can get the app doing what it's supposed to do — compare products.

[Per]: https://droppedbits.com/apps/per
[here]: /tags/per-rewrite-diary/
[day 3]: /post/per-diaries-day-3/
[day 5]: /post/per-diaries-day-5/
[yesterday]: /post/per-diaries-day-7/