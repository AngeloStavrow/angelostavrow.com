---
title: "The Per Rewrite Diary: Day 3"
date: 2020-02-03T17:30:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, we un-stink a code smell.
tags: ["per", "per rewrite diary", "ios"]
---

> This post is part of a series about rewriting my iOS app, [Per]. Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here].

## What's that smell?

So [yesterday], I posted about making Per's `Product` model conform to `Comparable`, so that I can compare between `Product` objects — something that's pretty important in a price-comparison app.

Comparing the actual price-per-unit is pretty straightforward:

```
static func <(lhs: Self, rhs: Self) -> Bool {
    return lhs.pricePerUnit < rhs.pricePerUnit
}

static func ==(lhs: Self, rhs: Self) -> Bool {
    return lhs.pricePerUnit == rhs.pricePerUnit
}
```

We can't just leave it at that, though, because of Per's built-in unit conversion — you can input one product's quantity in pounds and another product's quantity in grams, and Per will figure out the best-value option automatically. This means that Per's `Product` model needs to understand how units compare; you can't compare the price per unit between one thing sold by the kilogram, and something else sold by the quart.

The `Product` model stores units as an optional `Unit`, and we're going to create the initializer such that it'll set that property as either some flavour of `UnitMass` (for items sold by weight) or `UnitVolume` (for items sold by volume), or otherwise `nil` (for dimensionless units). Yesterday, I created an ugly nested-conditional mess for figuring out whether `lhs.units` and `rhs.units` can be compared. It works, but it feels like a code smell:

```
static func <(lhs: Self, rhs: Self) -> Bool {
    if let lhsUnitType = lhs.units {
        // lhs.units is not nil
        if let rhsUnitType = rhs.units {
            // rhs.units is not nil
            if (type(of: lhsUnitType) == type(of: rhsUnitType)) {
                return lhs.pricePerUnit < rhs.pricePerUnit
            } else {
                return false
            }
        } else {
            // rhs.units is nil
            return false
        }
    } else {
        // lhs.units is nil
        if rhs.units != nil {
            // rhs.units is not nil
            return false
        } else {
            // rhs.units is nil
            return lhs.pricePerUnit < rhs.pricePerUnit
        }
    }
}
```

We can make this much, much cleaner:

```
static func ==(lhs: Self, rhs: self) -> Bool {
    // If both lhs and rhs units nil, then evaluate the boolean expression:
    if (lhs.units == nil && rhs.units == nil) { return lhs.pricePerUnit == rhs.pricePerUnit }

    // If they're not BOTH nil, but any ONE is nil, return false:
    guard let lhsUnitType = lhs.units else { return false }
    guard let rhsUnitType = rhs.units else { return false }

    // If neither is nil, but they're of the same type, evaluate the boolean expression:
    if (type(of: lhsUnitType) == type(of: rhsUnitType)) { return lhs.pricePerUnit == rhs.pricePerUnit }

    // If we get to this point, they're not of the same type, so return false:
    return false
}
```

That feels _much_ cleaner. Tomorrow, let's explore how that initializer reasons about unit types!

[yesterday]: /post/per-diaries-day-2