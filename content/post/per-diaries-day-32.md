---
title: "The Per Rewrite Diaries: Day 32"
date: 2020-03-03T07:00:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I continue work on setting product units.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## Setting Units

[Yesterday], I set each entry in my unit `KeyValuePairs` to look something like this:

```
"kilograms": UnitMass.kilograms.symbol
```

And that way I could directly get the `String` representation of the unit's symbol (in this example, "kg") to drop into the picker view and text field.

Instead, I could make the entries look like this:

```
"kilograms": UnitMass.kilograms
```

When I need to get the string symbol for the unit, I can do that — no need to do that work ahead of time.

Now, with the addition of an optional tuple `selectedUnit: (String, Unit)`, I can handle setting the units for the first product (and thus the unit type for the entire `ProductList`) in the picker view's `didSelectRow:` delegate method:

```
func pickerView(_ pickerView: UIPickerView, didSelectRow row: Int, inComponent component: Int) {
    // FIXME: We're going through both unit data sources when we only need to go through one.
    _ = pickerWeightDataSource.contains { key, value in
        if (value.symbol == pickerTextFieldOutput[row]) {
            selectedUnit = (key, value)
            return true
        }
        return false
    }
    
    _ = pickerVolumeDataSource.contains { key, value in
        if (value.symbol == pickerTextFieldOutput[row]) {
            selectedUnit = (key, value)
            return true
        }
        return false
    }
    
    // FIXME: We're reaching into the subview to directly manipulate a textfield. THIS IS BAD!
    productDetailFormView.unitsTextField.text = pickerTextFieldOutput[row]
}
```

There are now a couple of `FIXME`s in that one delegate method. I don't love that I'm going through both the picker's weight _and_ volume data sources to see which symbol we've got; the method should be smarter than this.

So I can fix that this way, and —because Swift lets you define a function within a function— can clean up the duplicated code at the same time:

```
func pickerView(_ pickerView: UIPickerView, didSelectRow row: Int, inComponent component: Int) {
    func checkSymbolAndSetUnit(key: String, value: Unit) -> Bool {
        if (value.symbol == pickerTextFieldOutput[row]) {
            selectedUnit = (key, value)
            return true
        }
        return false
    }
    
    if (pickerTextFieldOutput[0] == pickerWeightDataSource[0].value.symbol) {
        _ = pickerWeightDataSource.contains { key, value in
            checkSymbolAndSetUnit(key: key, value: value)
        }
    } else {
        _ = pickerVolumeDataSource.contains { key, value in
            checkSymbolAndSetUnit(key: key, value: value)
        }
    }
    
    // FIXME: We're reaching into the subview to directly manipulate a textfield. THIS IS BAD!
    productDetailFormView.unitsTextField.text = pickerTextFieldOutput[row]
}
```

That feels _much_ cleaner. Since `pickerTextFieldOutput` is an array of all the unit symbol, I can test to see if the first element matches the symbol of the first element in my `pickerWeightDataSource` (a `KeyValuePairs` collection). If it is, find the right `UnitMass` and set that as the `selectedUnit`. If it's not, go through the `pickerVolumeDataSource` instead.

That sorts things out for setting the units on the first product and the unit type for the `ProductList`, but then I can't choose the units for subsequent products that I add to the list. I'll work on that tomorrow!

[Yesterday]: /post/per-diaries-day-31.md