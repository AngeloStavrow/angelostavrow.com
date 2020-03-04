---
title: "The Per Rewrite Diaries: Day 33"
date: 2020-03-04T08:00:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I wrap up work on the unit conversion feature.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## Optionally Conditionally Yours

Here's where we're at right now.

When the form to add a product to the list is first shown, Per inserts a segmented control that allows the user to choose whether the product in question is sold by weight, by volume, or by units. When adding a second or third product, this isn't shown, to enforce that all products are comparable on a price-per-unit basis.

This first product sets the `unitType` property (either `UnitMass` or `UnitVolume`) for the product list.

After that initial product is added, showing the form should obey the following rules:

1. If the product list's `unitType` is `nil` (dimensionless units), keep the unit text field disabled.
2. Otherwise, enable the unit text field and show the appropriate picker view (either with weight or volumne units, based on the list's `unitType`).

To do this, I made a couple of changes to the the product detail content view controller (which creates and presents the form view) delegate:

1. Added a `listUnitType: Unit?` property that gets the product list's `unitType`;
2. Added a `setPickerTo(_ unit: Unit?)` method that looks at the type of unit passed in, and then sets the picker view's data source appropriately.

The added method also allowed me to refactor the delegate method that was called by the segmented control that I discussed [here].

Now, when the form view is created, I can add a check in the `delegate` property observer:

```
weak var delegate: ProductDetailContentViewControllerDelegate? {
    didSet {
        if self.delegate?.numberOfProductItems == 0 {
            insertUnitTypeSelectorControl()
            formHeight = getHeight(of: formStackView)
        } else {
            if let listUnitType = self.delegate?.listUnitType {
                unitType = listUnitType
                unitsTextField.text = listUnitType.symbol
                unitsTextField.isEnabled = true
            }
        }
    }
}
```

That checks for and conditionally sets an optional `unitType` property on the form when the delegate is set, _if_ it's not the first product being added _and_ the product list has a `unitType`. If the checks pass, it enables the units text field and sets it to some value.

Then in the `textFieldDidBeginEditing(:)` delegate method, I can check the `unitType` property when a user taps on the units text field and call `delegate?.setPickerTo(unitType)` to setup the picker view's units.

And with that, automatic unit conversion now works! Tomorrow, I'm going to go through all of this and make a list of what new [papercuts] have come up.

[here]: /post/per-diaries-day-31/
[papercuts]: /post/per-diaries-day-20/