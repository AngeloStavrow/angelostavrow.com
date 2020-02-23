---
title: "The Per Rewrite Diaries: Day 23"
date: 2020-02-23T17:45:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I start work on a custom table view cell.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## Custom Table View Cells

Right now, the `ProductListContentViewController` is using a very basic `UITableViewCell` to show basic details of each `ProductItem` entry. That's fine, but it's going to get messy when that cell gets a custom layout. Better to contain that in a separate view!

So, to begin with, I just wanted to refactor out that code into a new class. Creating one that conformed to `UITableViewCell` gave me some stubbed methods to add, and I added some properties to the class. First, a `UILabel` that I can later customize:

```
private let productDetailLabel: UILabel = {
    let label = UILabel()
    return label
}()
```

Then a `product` that, when set, updates the `productDetailLabel`:

```
var product: ProductItem {
    didSet {
        let productUnits = product.units?.symbol ?? "unit"
        productDetailLabel.text = "\(product.quantity) \(productUnits)s for \(product.price) costs \(product.pricePerUnit) per \(productUnits)"
    }
}
```

Right now, this means that I have to initialize the `product` property with some default value when it's initialized:

```
override init(style: UITableViewCell.CellStyle, reuseIdentifier: String?) {
    self.product = ProductItem(price: 1.00, quantity: 1.99, units: nil)
    super.init(style: style, reuseIdentifier: reuseIdentifier)
    addSubview(productDetailLabel)
    setupConstraints()
}
```

The `setupContstraints()` method does the same thing that it does in other `UIView`s — sets the view to _not_ translate its autoresizing mask into constraints, and then activates the constraints I want. Pretty straightforward. Then it's just a question of using this custom cell in `ProductListContentViewController` instead of the default style:

```
override func viewDidLoad() {
    // The other viewDidLoad stuff is done here
    // [...]
    // Then the custom cell is registered:
    productTableView.register(ProductListCellView.self, forCellReuseIdentifier: "productListCell")
}

override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    let productItem = productList.getItems()[indexPath[1]]
    let cell = productTableView.dequeueReusableCell(withIdentifier: "productListCell", for: indexPath) as! ProductListCellView
    cell.product = productItem
    return cell
}
```

That's it! Something that I tend to forget when laying things out in code is to set `translatesAutoresizingMaskIntoConstraints` to `false` so that everything works as expected, but I'm getting the hang of doing that first in any `setupConstraints()` method now.

Tomorrow, I'll customize this a bit further with multiple lines.