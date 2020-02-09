---
title: "The Per Rewrite Diary: Day 9"
date: 2020-02-09T18:30:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, we're adding auto-sorting to our product list.
tags: ["per", "per rewrite diary", "ios"]
---

> This post is part of a series about rewriting my iOS app, [Per]. Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here].

## Delegate, Delegate, Delegate

So I'm now working on a simple UI to add product details to the table view. Presenting a new product detail _content_ view controller from the product list _context_ view controller isn't too hard — when the ➕ button is tapped in the navigation bar, we just need to responding by pushing a new view controller onto the navigation stack:

```
@objc func handleAddProductBarButtonItemTapped(sender: UIBarButtonItem) {
    let productDetailVC = ProductDetailContentViewController()
    self.present(productDetailVC, animated: true, completion: nil)
}
```

But we also need to be able to add a item to the product list, so we create a `delegate` property in this new product detail content view controller:

```
weak var delegate: ProductListContextViewController!
```

The tells the detail VC, "hey, delegate any work back to the product list context VC"; in this case, Per will collect the details of the new product you want to add in the `ProductDetailContentViewController` and pass them back when to its delegate when you tap an "Add" button:

```
@objc func addButtonTapped(_ sender: UIButton!) {
    delegate.add(createRandomProduct())
    self.dismiss(animated: true, completion: nil)
}
```

(I've just got a placeholder "Add" button that calls the `createRandomProduct()` method I mentioned on [day 7]).

Now, I can go back to the product list context view controller and add itself as a delegate to the product detail content view controller:

```
@objc func handleAddProductBarButtonItemTapped(sender: UIBarButtonItem) {
    let productDetailVC = ProductDetailContentViewController()
    productDetailVC.delegate = self
    self.present(productDetailVC, animated: true, completion: nil)
}
```

And we add the method called:

```
func add(_ item: ProductItem) {
    self.contentViewController.productList.add(item, sort: true)
    self.contentViewController.loadView()
}
```

So, to summarize, here's what happens:

- The `ProductListContextViewController` creates a `ProductDetailContentViewController` and tells it, "hey, I'm your delegate!" before presenting it.
- The `ProductDetailContentViewController` presents a UI for collecting product info (price, quantity, units), and then calls the `add()` method of its delegate (the `ProductListContextViewController`) before dismissing itself.
- The `ProductListContextViewController` receives the call to its `add()` method and updates the product list and table view accordingly.

There's something weird about how my detail view controller dismisses itself, though — sometimes it's a smooth animation, and sometimes it just disappears. I'll figure out why tomorrow.


[Per]: https://droppedbits.com/apps/per
[here]: /tags/per-rewrite-diary/
[day 7]: /post/per-diaries-day-7/