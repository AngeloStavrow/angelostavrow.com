---
title: "The Per Rewrite Diary: Day 7"
date: 2020-02-07T07:45:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, we're working on UI elements to add products to our product list.
tags: ["per", "per rewrite diary", "ios"]
---

> This post is part of a series about rewriting my iOS app, [Per]. Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here].

## Where's UINavigationItem?

On [day 1], we created a hierarchy of view controllers — coordinators, context, content, containers. In retrospect, this may have overcomplicated how I reason about where things go. Something like adding a `UIBarButtonItem` should be fairly straightforward:

```
addBarButtonItem = UIBarButtonItem(barButtonSystemItem: .add,
                                                  target: self,
                                                  action: #selector(handleAddTapped(sender:)))
navigationItem.rightBarButtonItem = addBarButtonItem
```

(Remember, I'm using stock UI components and functionality for now.)

So I added that to `viewDidLoad()` in my `ProductListCoordinatorViewController`, since its root view controller is a `UINavigationController`. But when I ran the app... no button.

I could change the navigation bar's color without issue, but adding a button to it? Nope. Turns out, I had to drop down a level, to my `ProductListContextViewController` and add the button _there_. Why? We can always turn to Hacking With Swift for an answer:

> Note: usually bar button items don't belong to the `UINavigationBar` directly. Instead, they belong to a `UINavigationItem` that is currently active on the navigation bar, which in turn is usually owned by the view controller that is currently active on the screen.

[[source]]

This makes sense, because you'll want the view controller that's active to make decisions about what navigation items are relevant. Could I have gone deeper still into the embedded `ProductListContentViewController`, which holds our table view? Probably! Does it make more sense? Maybe! I'm going to explore this when I implement a detail view controller for adding new products.

Right now, when you tap the ➕ button in the navigation bar, the app runs the following action:

```
@objc func handleAddProductBarButtonItemTapped(sender: UIBarButtonItem) {
    self.contentViewController.productList.add(createRandomProduct())
    self.contentViewController.loadView()
}
```

All this does is add a product to the table view's data source, and then call `loadView()` on the content view controller to update the table view with the new entries. And we're just creating a simple, unitless random product for now:

```
// MARK: - Temporary methods for testing
private func createRandomProduct() -> ProductItem {
    let dollars = Double(Int.random(in: 1...5))
    let cents = Double(Int.random(in: 0..<100)) / 100
    let price = dollars + cents
    let quantity = Double(Int.random(in: 25...50))
    
    return ProductItem(price: price, quantity: quantity)
}
```

And so we can now add products to the product list, and see them appear in the app's table view UI. Neat!

But... this app is about comparing price per unit, not just listing products. So, tomorrow, I'm going to add a sorting feature to the ProductList model that gets called whenever you add a new item.

[Per]: https://droppedbits.com/apps/per
[here]: /tags/per-rewrite-diary/
[day 1]: /post/per-diaries-day-1/
[source]: https://www.hackingwithswift.com/example-code/uikit/how-to-add-a-bar-button-to-a-navigation-bar