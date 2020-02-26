---
title: "The Per Rewrite Diaries: Day 26"
date: 2020-02-26T08:00:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I plan out the automatic unit conversion feature.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## The plan

The shipping version of Per pre-dates the [Units and Measurement] libraries, so I had to implement all unit conversion carefully myself. Now that it's built into Foundation, I don't have to do that work anymore (thanks Apple!) and instead can focus on the app itself.

Unit conversion itself is handled in the `ProductItem` model, specifically in the `pricePerUnit` computed property (as discussed on [day 4]). Unit-matching enforcement (i.e., so the user doesn't try to compare price-per-pound by price-per-litre) is handled by the `ProductList` (as discussed on [day 5]). All this validation is happening at the model layer, so then it's just a question of implementing the UI to do the following:

1. When adding the _first_ product, an additional bit of UI to select between weight units, volume units, or dimensionless units. In the shipping version, this is a segmented control, and that's probably fine here as well.
2. Once selected, the units input field (currently a disabled text field) becomes some kind of control where you can choose the units. In the shipping version, this is implemented as a `UIPicker`, but there's just something about that control I don't like. Maybe some kind of popover?
3. In any other products added, the UI should default to only showing the the units that match those of the first product. That's easy enough to check for, via the `ProductList`'s `_unitType` optional property. It's currently marked as private, but that's not really necessary for the getter — only the setter.

Tomorrow, I get cracking on this work!

[Units and Measurement]: https://developer.apple.com/documentation/foundation/units_and_measurement
[day 4]: /post/per-diaries-day-4/
[day 5]: /post/per-diaries-day-5/