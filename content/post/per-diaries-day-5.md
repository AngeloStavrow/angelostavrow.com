---
title: "The Per Rewrite Diary: Day 5"
date: 2020-02-05T19:30:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, we're looking at a table view data source for the app.
tags: ["per", "per rewrite diary", "ios"]
---

> This post is part of a series about rewriting my iOS app, [Per]. Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here].

## Start with a Spike

I'm trying to spend less than an hour a day working on this rewrite.

One thing I've found is that trying to think through an elegant solution, write the code, and write up these little diary posts is... ambitious, especially on a daily cadence.

What does seem to work well is thinking about the problem I'm trying to solve, and then [spiking a solution] for it:

> Create spike solutions to figure out answers to tough technical or design problems. A spike solution is a very simple program to explore potential solutions. Build the spike to only addresses the problem under examination and ignore all other concerns. **Most spikes are not good enough to keep, so expect to throw it away.**

(Emphasis mine.)

I like iteratively solving little problems to maintain forward momentum. It lets me easily scale back the scope of what I'm trying to achieve if the problem feels too hairy to solve in an hour. And if I keep in mind that every line of code I write is equally likely to be thrown away, I don't have to feel attached to the way I'm tackling the problem.

Of course, the point of this rewrite is to remove technical debt, so if at the end of my hour I don't feel great about my solution, I can always choose to revisit it again tomorrow.

So, with that, on to today's problem.

## The Product List

The table view controller from [day 1] needs a data source, and that data source should contain a collection of the `Product` items I've been working on the last few days.

But we can't just add any ol' object that conforms to `Product` to an array and call it a day. Once we add the first such object to this collection —call it a `ProductList` (because... that's what I called it)— we're defining what _type_ of product it will hold, based on the type of units it uses: either something based on `UnitMass` for items sold by weight, `UnitVolume` for items sold by volume, or `nil` (`Product.units` is an optional, after all) for items that are sold by the unit.

So, if the first item I add has a `UnitMass` type, and then I try to add something without units (i.e., `nil`), the `ProductList` should reject it. That means it needs to remember the first type of `Product` added.

Here's what I came up with as today's spike. It's a struct that uses private properties, and a `mutating` function to add `ProductItem`s (which conform to `Product`) to an internal array as well as set and check the unit type integrity. It doesn't throw an error yet, it just prints a message to console and skips adding the item.

It also adds a function to return the internal array. So it's basically a public-get, private-set value type.

```
struct ProductList {
    private var _products: [ProductItem]
    private var _unitType: Unit?
    
    init() {
        _products = [ProductItem]()
    }
    
    mutating func add(_ item: ProductItem) {
        // We can't just add ProductItems willy-nilly because they have to have matching units.
        // So, we need to check two things:
        //
        // 1. Is there already a ProductItem in the _products array?
        // 2. If so, what is its unit type (nil, UnitMass, or UnitVolume)?
        let incomingUnitType = item.units ?? nil
        
        if (_products.count > 0) {
            // There's stuff already in the _products array, so check the unit type.
            if (incomingUnitType == nil && _unitType == nil) {
                _products.append(item)
                return
            } else if (incomingUnitType != nil && incomingUnitType!.isKind(of: type(of: _unitType!))) {
                _products.append(item)
                return
            } else {
                print("Can't add an item with type \(String(describing: incomingUnitType)) to a list of \(String(describing: _unitType)) items")
            }
        } else {
            _unitType = incomingUnitType
            _products.append(item)
        }
    }
    
    func getItems() -> [ProductItem] {
        return _products
    }
}
```

I don't love the `if/else if/else` stuff going on here and can probably make it more elegant, but I can reason about this type of logic really easily, so it makes it very fast, if somewhat ugly, to write.

The `add(item:)` function should also be marked as `throws`, and that logged message about mismatched types should throw an error for the client to catch. I'll add that tomorrow and see what I can do about cleaning up the logic. I feel like I'm not using a lot of the really beautiful bits of Swift —like `guard` and nil coalescing— to their full potential here, and that's due to a lack of familiarity.

If you've got feedback, it's always welcome!

[Per]: https://droppedbits.com/apps/per
[here]: /tags/per-rewrite-diary/
[spiking a solution]: http://www.extremeprogramming.org/rules/spike.html
[day 1]: /post/per-diaries-day-1/