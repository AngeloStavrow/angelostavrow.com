---
title: "Per Diaries Day 25"
date: 2020-02-25T07:30:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I wrap up work on a custom table view cell.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## Nicer table view cells

[Yesterday], I mentioned that each cell should show how much more you're paying over the best-value option. I got that all implemented today, and here's how I did it.

First, I added a property to the `ProductList` called  `bestValue` that's updated with the lowest `pricePerUnit` in the array of `ProductItem`s when we add to it. It's a pretty straightforward affair:

```
mutating func add(_ item: ProductItem, sort: Bool = false) throws {
	// ...the rest of the function
	bestValue = _products.sorted().first?.pricePerUnit ?? 0.0
}
```

I also added a couple of properties in my `ProductListCellView`:

- a `valueForMoney` string that, when updated, sets a `UILabel`
- an `isBestValue` flag that, when updated, sets the color of the above label's text

I set these in the `ProductListContentViewController` when adding a new cell, but after checking whether this particular product we're adding to the list is the best-value option:

```
if (productItem.pricePerUnit == productList.bestValue) {
    cell.valueForMoney = "Best value!"
    cell.isBestValue = true
} else {
    cell.valueForMoney = "You're paying \(Int(100 * (productItem.pricePerUnit - productList.bestValue) / productList.bestValue))% more!"
    cell.isBestValue = false
}
```

Here's what it looks like now:

!["A screen capture of the iPhone Simulator showing a list of products compared by price per unit"](/images/2020-02-25/product-list.png)

Much better! I'm hard-coding the currency symbol, which is bad — I need to add a proper formatter for the quantity and price values, and handle localization (something that the current shipping version of Per doesn't do, which is also bad). I've opened an issue for this.

## A display bug

Something that irks me in the above screenshot is a display bug that I don't quite understand how to resove. You can see empty cells in the table, and that looks ugly. I'd rather they be hidden.

According to all of my research, when the table view is empty, I should be able to hide any empty cells by setting:

```
productTableView.tableFooterView = UIView(frame: .zero)
```

That… doesn't work, and I'm not sure why. If you've got any ideas, let me know!

Tomorrow, I'll start work on unit conversion!

[Yesterday]: /post/per-diaries-day-24