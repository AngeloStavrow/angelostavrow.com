---
title: "The Per Rewrite Diaries: Day 30"
date: 2020-03-01T07:45:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I spike a solution for getting a picker view to show.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## Spiking a Picker View

As I mentioned [yesterday], I'm a little stuck trying to implement a `UIPickerView` to let you choose the units for a product. Maybe I was trying a bit too hard to be clever about this, so I'm going to try a spike solution today and see how that works out.

The `ProductDetailContentViewController` shows a `ProductFormDetailView` that contains the input fields for entering a new product. When the user taps the input field, I want to show a `UIPickerView` that presents some options based on the unit type of the list. That unit type is selected when you add the first product via a `UISegmentedControl`.

I'm going to start by making the `ProductDetailContentViewController` conform to `UIPickerViewDelegate` and `UIPickerViewDatasource`. That means I have to add a couple of methods, so I let Xcode add the stubs:

```
func numberOfComponents(in pickerView: UIPickerView) -> Int {
    <#code#>
}

func pickerView(_ pickerView: UIPickerView, numberOfRowsInComponent component: Int) -> Int {
    <#code#>
}
```

I also declare a `pickerView` and, in the view controller's `viewDidLoad()` method, have its delegate and data source set to `self`. For now, the data source will be an array of strings:

```
var pickerView: UIPickerView!
let pickerDataSource = ["Option 1", "Option 2", "Option 3", "Option 4", "Option 5"]

override func viewDidLoad() {
    super.viewDidLoad()

    /* --- Some setup for other views --- */

    pickerView = UIPickerView()
    pickerView.delegate = self
    pickerView.dataSource = self
}
```

Okay, with that, I can fill out those protocol stubs:

```
func numberOfComponents(in pickerView: UIPickerView) -> Int {
    return 1
}

func pickerView(_ pickerView: UIPickerView, numberOfRowsInComponent component: Int) -> Int {
    return pickerDataSource.count
}
```

There's an important protocol method missing here, though — the one that shows something in each row of the picker view:

```
func pickerView(_ pickerView: UIPickerView, titleForRow row: Int, forComponent component: Int) -> String? {
    return pickerDataSource[row] as String
}
```

That should be enough to get `Option 1`, `Option 2`, `Option 3`, etc., to show in the picker view. Now we just need to add that picker view to the view hierarchy.

Here's where things get a little bit complicated. We only want the picker view to show when the following criteria are met:

1. The `unitType` of the `ProductList` is _not_ dimensionless (i.e., is of `UnitMass` or `UnitVolume`)
2. The user taps on the units field in the `ProductDetailFormView`

Right now the units field is disabled, so I start by adding logic to the `UISegmentedControl`'s action such that it gets enabled if the user chooses something other than "units". I also add an `showPickerView()` delegate method on the view controller that its subviews can call, but all it does for now is print a success message to the console.

Now, I can add logic to the `textFieldDidBeginEditing()` event listener to show the picker view when someone taps on the units field:

```
func textFieldDidBeginEditing(_ textField: UITextField) {
    if textField.tag == 101 {
        delegate?.showPickerView()
    }
}
```

That calls the delegate method successfully, so I add the necessary code to show the picker view:

```
func showPickerView() {
    view.addSubview(pickerView)
    pickerView.translatesAutoresizingMaskIntoConstraints = false
    NSLayoutConstraint.activate([
        pickerView.leadingAnchor.constraint(equalTo: view.leadingAnchor, constant: 0),
        pickerView.trailingAnchor.constraint(equalTo: view.trailingAnchor, constant: 0),
        pickerView.bottomAnchor.constraint(equalTo: view.bottomAnchor, constant: 0)
    ])
}
```

A test in Simulator doesn't show the picker view at first... _because it's behind the keyboard_. That makes sense.

In the shipping version of Per, I set the keyboard to be the picker view — this avoids any jarring appearing/disappearing of the keyboard as you go from text field to text field. So in fact, that deeply simplifies things: I can pass the calling `UITextField` into the `showPickerView()` method and just set its `inputView` to the picker view:

```
func showPickerView(_ sender: UITextField) {
    sender.inputView = pickerView
}
```

And that works like a charm. When the units field is enabled and tapped, I see a picker view. When another text field is tapped, I see the decimal keyboard.

I'll add one final protocol method, so that the text field can be set to the selection in the picker view:

```
func pickerView(_ pickerView: UIPickerView, didSelectRow row: Int, inComponent component: Int) {
    productDetailFormView.unitsTextField.text = pickerDataSource[row]
}
```

This works, but is a bad solution: I'm reaching into the subview to directly manipulate one of its controls. The view controller should know nothing about its subviews, because if something changes in the subview, then that could break everything. I added a `FIXME` here to update this, and it'll probably involve a property observer on something like `VolatileFormData` to update text fields.

For now, this can wait — I want to continue building off a working solution. Right now the segmented control only shows for the first product you choose, so tomorrow I'll work on making this actually set the `ProductList`'s `unitType` property so that subsequent products use these units and show the picker view.

[yesterday]: /post/per-diaries-day-29/