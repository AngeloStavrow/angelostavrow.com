---
title: "The Per Rewrite Diary: Day 2"
date: 2020-02-02T18:15:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, it's about the product model.
tags: ["per", "per rewrite diary", "ios"]
---

> This post is part of a series about rewriting my iOS app, [Per]. Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here].

## I'm a model, you know what I mean

View controllers are fun to work on and all, but at the end of the day, they're only meant to mediate between the user and some data model. In Per, we've got two models that I can think of right now. Today, we focus on one.

## The Product

_Start with a protocol_, say Apple. The Product model in Per is fairly straightforward, but sure, let's do the protocol-oriented programming thing. Here's what our protocol looks like:

```
protocol Product: Comparable {
	var quantity: Double { get set }
	var units: Unit? { get set }
	var price: Double { get set }
	var pricePerUnit: Double { get }
```

Why is `units` an optional? Because we only care if the units are of type `UnitMass` or `UnitVolume` right now — otherwise we treat the product as dimensionless, i.e., plain ol' units.

The `Product` has to conform to `Comparable` so that we can, uh, compare multiple `Product`s — and here, we're starting with a simplistic implementation in an extension:

```
extension Product {
    var pricePerUnit: Double {
        get {
            let formatter = NumberFormatter()
            formatter.numberStyle = .currency
            let formattedPricePerUnit = formatter.string(for: NSNumber(value: price / quantity))
            return Double(truncating: formatter.number(from: formattedPricePerUnit!)!)
        }
    }
    
    static func <(lhs: Self, rhs: Self) -> Bool {
        // Naïve implementation, doesn't account for unit type
        return lhs.pricePerUnit < rhs.pricePerUnit
    }
    
    static func ==(lhs: Self, rhs: Self) -> Bool {
        // Naïve implementation, doesn't account for unit type
        return lhs.pricePerUnit == rhs.pricePerUnit
    }
}
```

This doesn't account for the fact that if a client tries to compare something of `UnitLength.meters` against `UnitType.pounds`, it should fail. Instead, I'm simply looking at comparing the price per unit, which is a computed property in the `Product` extension. I don't love the way I'm rounding the value of `pricePerUnit` here using a `NumberFormatter`, so if you have any suggestions, do let me know!

Tomorrow, I'll work on an initializer for the actual `struct` that implements the protocol, and aim to better handle the unit comparison.

[Per]: https://droppedbits.com/apps/per
[here]: /tags/per-rewrite-diary/