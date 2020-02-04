---
title: "The Per Rewrite Diary: Day 4"
date: 2020-02-04T18:00:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, we're digging deeper into the main product model's initializer.
tags: ["per", "per rewrite diary", "ios"]
---

> This post is part of a series about rewriting my iOS app, [Per]. Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here].

## Funny subtitle goes here

On [day 2] of the rewrite, I put together a computed property for `pricePerUnit` in an extension to the `Product` protocol that looked like this:

```
var pricePerUnit: Double {
    get {
        let formatter = NumberFormatter()
        formatter.numberStyle = .currency
        let formattedPricePerUnit = formatter.string(for: NSNumber(value: price / quantity))
        return Double(truncating: formatter.number(from: formattedPricePerUnit!)!)
    }
}
```

As I mentioned at the time, I still don't like the way I'm doing the necessary work  of rounding digits using the `NumberFormatter` conversion to a `String` and back, but it works as a spike solution _for now_ — we can come back to it another day.

I also added two static functions to the extension [yesterday] so that the protocol better conforms to `Comparable` — not only do we want to compare the `pricePerUnit` property, but we also want to compare the types of `Unit`s and make sure they're comparable.

Here's where I need to be careful. I can use `quantity` and `units` properties to initialize a [`Measurement`], which gives me the ability to convert this to base units via the appropriately-named `baseUnits()` method on `Measurement` — this gives me a common unit to compare all items of the same unit type (`UnitMass` and `UnitVolume` are the two we care about). I can add the following logic to the `pricePerUnit` computed property getter and we're good to go:

```
var pricePerUnit: Double {
    get {
        var amount: Double = 0
        let formatter = NumberFormatter()
        formatter.numberStyle = .currency
        
        if let unitType = units {
            if (unitType.isKind(of: UnitMass.self)) {
                // This product is sold by weight.
                let measurement = Measurement(value: quantity, unit: unitType as! UnitMass).converted(to: UnitMass.baseUnit())
                amount = measurement.value
            } else if (unitType.isKind(of: UnitVolume.self)) {
                // This product is sold by volumne.
                let measurement = Measurement(value: quantity, unit: unitType as! UnitVolume).converted(to: UnitVolume.baseUnit())
                amount = measurement.value
            }
        } else {
            amount = quantity
        }
        // We're force-unwrapping here but these variables should never be nil. 😬
        return Double(truncating: formatter.number(from: formatter.string(for: NSNumber(value: price / amount))!)!)
    }
}
```

With that, we can set up a simple ProductList model and get it hooked up as a `UITableViewController` data source tomorrow!

[Per]: https://droppedbits.com/apps/per
[here]: /tags/per-rewrite-diary/
[`Measurement`]: https://developer.apple.com/documentation/foundation/units_and_measurement
[day 2]: /post/per-diaries-day-2/
[yesterday]: /post/per-diaries-day-3/