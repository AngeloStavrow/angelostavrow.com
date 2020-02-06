---
title: "The Per Rewrite Diary: Day 6"
date: 2020-02-06T07:15:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, we're connecting our ProductList to the main table view.
tags: ["per", "per rewrite diary", "ios"]
---

## Building the UITableView in code

If you try to simply add `var productTableView: UITableView = UITableView()` as a property and then set its `delegate` and `dataSource` as `self` without touching the other boilerplate methods, you'll crash the app with this error:

```
*** Assertion failure in -[UITableView _dequeueReusableCellWithIdentifier:forIndexPath:usingPresentationValues:], /BuildRoot/Library/Caches/com.apple.xbs/Sources/UIKitCore_Sim/UIKit-3901.4.2/UITableView.m:8624
*** Terminating app due to uncaught exception 'NSInternalInconsistencyException', reason: 'unable to dequeue a cell with identifier customcell - must register a nib or a class for the identifier or connect a prototype cell in a storyboard'
```

This makes sense! The commented-out `tableView(cellForRowAt:)` method needs to be uncommented, and I need to give this table view a UITableViewCell to work with. And it's not too hard, I just need to add this to the table view controller's `viewDidLoad` method:

```
productTableView.register(UITableViewCell.self, forCellReuseIdentifier: "productListCell")
```

## Adding the ProductList data

Now I can create a `ProductList` in the table view controller, populate it with some fake data, and start working with it!

![iOS Simulator showing three of rows of ProductItem data](/images/2020-02-06/simulator.png)

Getting that to display just requires three lines in `tableView(cellForRowAt:)` :

```
let productItem = productList.getItems()[indexPath[1]]
let productUnits = productItem.units?.symbol ?? "units"
cell.textLabel?.text = "\(productItem.quantity) \(productUnits) for \(productItem.price) costs \(productItem.pricePerUnit) per unit"
```

Remember that the `indexPath` is collection of two values (`[section, row]`), so we want to specify its row value (`indexPath[1]`) as our index for the array returned by `productList.getItems()`.

There's no formatting, and it's a static view of data that we can't add to, but we're slowly getting there! Tomorrow, I'll start working on a way to add product items to the table view.

[Per]: https://droppedbits.com/apps/per
[here]: /tags/per-rewrite-diary/