---
title: "The Per Rewrite Diaries: Day 31"
date: 2020-03-02T21:00:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I work towards getting the picker view to set product units.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## Improving the Picker View

[Yesterday] I got a spike solution to show the `UIPickerView` showing when weight or volume units were selected in the form detail view. The form view doesn't actually set the `ProductList`'s `unitType` _yet_, so today I made some progress toward that.

First, the UI needs to give the user the ability to select the units for the product they're entering. This means populating the picker view's data source with the appropriate units based on what the selection is for unit type (as set by a `UISegmentedControl`).

This means adding some kind of backing store for some subset of each type of units. I initially did this by adding two `Dictionary` objects with a `String` description of the unit as a key, and the `symbol` for that particular unit type (i.e., `UnitMass.kilograms.symbol`) as the value, figuring that both can be shown in the picker view, and the symbol alone shown in the form's unit text field.

Then, when the user makes a unit-type selection, a delegate method is called that:

1. Creates an array where each element is the concatenation of the key and value for each dictionary entries;
2. Sets that array as the data source for the picker view;
3. Creates a second array of just the value for each dictionary entry; and
4. Calls the picker's delegate's `didSelectRow:` method to set the unit text field's value.

## Turns out

Here's a fun thing I forgot about! If you call `map()` on a `Dictionary`, the resulting array isn't guaranteed to be in the same order as the input collection.

If that's important, you can use a `KeyValuePairs` collection, which was renamed from `DictionaryLiteral` in Swift 5 (here's the proposal: [SE-0214]).

So here's what these not-really-a-dictionary dictionaries look like:

```
var pickerView: UIPickerView!
var pickerDataSource = [String]()        // For setting picker options in titleForRow:
var pickerTextFieldOutput = [String]()   // For setting units text field in didSelectRow:

let pickerWeightDataSource: KeyValuePairs = [
    "kilograms": UnitMass.kilograms.symbol,
    "grams": UnitMass.grams.symbol,
    "pounds": UnitMass.pounds.symbol,
    "ounces": UnitMass.ounces.symbol
]
    
 let pickerVolumeDataSource: KeyValuePairs = [
    "liters": UnitVolume.liters.symbol,
    "centiliters": UnitVolume.centiliters.symbol,
    "milliliters": UnitVolume.milliliters.symbol,
    "gallons": UnitVolume.gallons.symbol,
    "quarts": UnitVolume.quarts.symbol,
    "pints": UnitVolume.pints.symbol,
    "fluid ounces": UnitVolume.fluidOunces.symbol
]
```

Again, this is an ongoing spike solution, so it doesn't take localization into account — beyond the (American-)English descriptive names as keys,  Foundation actually provides for separate US and Imperial volume units, so that you can convert from e.g. `UnitVolume.gallons` and `UnitVolume.imperialGallons` — this will be sorted out later.

Here's the functional stuff for setting the picker view's data source and the unit text field's value:

```
// Swap data source contents based on `UISegmentedControl` selection
func setUnitType(_ sender: UISegmentedControl, target: UITextField) {
    var currentlySelectedRow = pickerView.selectedRow(inComponent: 0)

    switch(sender.selectedSegmentIndex) {
    case 0:
        pickerDataSource = pickerWeightDataSource.map({ key, value in "\(key) (\(value))" })
        pickerTextFieldOutput = pickerWeightDataSource.map({ key, value in "\(value)" })
        pickerView.reloadAllComponents()
    case 2:
        pickerDataSource = pickerVolumeDataSource.map({ key, value in "\(key) (\(value))" })
        pickerTextFieldOutput = pickerVolumeDataSource.map({ key, value in "\(value)" })
        pickerView.reloadAllComponents()
    default:
        pickerDataSource = [""]
        return
    }

    if pickerView.numberOfRows(inComponent: 0) <= currentlySelectedRow {
        currentlySelectedRow = pickerView.numberOfRows(inComponent: 0) - 1
    }
    pickerView.delegate?.pickerView?(self.pickerView, didSelectRow: currentlySelectedRow, inComponent: 0)
    target.becomeFirstResponder()
}
```

So that's working nicely. Changing selection between weight and volume units works gracefully, and the text field updates as soon as the picker view is shown or changed, so that it's never in a weird state (as can sometimes happen in the current shipping version of Per).

Tomorrow, I'll actually hook this sucker up to set the `ProductList`'s unit type, which should —I _think_— be all I need to get automatic unit conversion working. Then, I can focus on refactoring this stuff into something a little less hack-y.

[Yesterday]: /post/per-diaries-day-30/
[SE-0214]: https://github.com/apple/swift-evolution/blob/master/proposals/0214-DictionaryLiteral.md