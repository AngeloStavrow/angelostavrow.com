---
title: "The Per Rewrite Diary: Day 18"
date: 2020-02-18T18:00:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I continue work on reading values from a child view's text fields.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per]. Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here].

## Of delegates and datasources

The `ProductDetailFormView` has a set of three `UITextFields` for entering details (quantity, price, and units) when the user wants to add a new product. Figuring out which text field has been updated is easy enough — give them each a tag and then in the `UITextFieldDelegate`'s `textFieldDidEndEditing()` method, check for the tag and... do what with it? It's possible that the user go back and make changes so it's helpful to have a temporary way to track the latest value from the form.

That temporary object can then be read from and turned into a new `ProductItem` object when the user hits the **Add** button. So how do I set it up?

I started by creating a simple `VolatileFormData` struct in the `ProductDetailContentViewController`:

```
struct VolatileFormData {
    var price: String
    var units: String
    var quantity: String
}
```

Then, I give the `ProductDetailFormView` a `datasource` property:

```
class ProductDetailFormView: UIView, UITextFieldDelegate {
  var delegate: ProductDetailContentViewControllerDelegate?
  var datasource: VolatileFormData?

  // The rest of the class implementation goes here
}
```

Change the `ProductDetailContentViewControllerDelegate` to have an `updateVolatileFormData()` method that can be called when a text field's editing-ended event is triggered with the new values, and when the **Add** button is tapped, a new `ProductItem` will be created from the (parsed `String`) data in the `VolatileFormData` struct. Right?

## H*ckin' completion blocks

Not exactly. Why? Well, the action for the **Add** button looked something like this like this:

```
@objc func addButtonTapped(_ sender: UIButton!) {
  delegate.add(createProductFromFormData())
  self.dismiss(animated: true, completion: nil)
}
```

This means I'm trying to create the `ProductItem` based on what was in the temporary form-data object when the **Add** button was tapped, and then I dismiss the `ProductDetailFormView`.

So here's what happens in that case (keeping in mind that for now, units are not considered):

1. User taps on the quantity field, enters a quantity
2. User taps on the price field, the quantity field fires the editing-ended event, and `volatileFormData` is updated
3. User enters price, taps on the **Add** button
4. The view controller fires the `addButtonTapped` action, which tries to create the `ProductItem`
5. If it succeeds, the view controller dismisses itself, so the price field fires the editing-ended event, and `volatileFormData` is updated

See the issue?

The form data doesn't get updated until _after_ the view controller tries to create the product. But the `dismiss(animated:)` method includes an optional `completion:` block that can be run _after_ the view controller dismisses itself. Seems like a good time to create the `ProductItem`, right?

So now the action looks like this:

```
@objc func addButtonTapped(_ sender: UIButton!) {
    self.dismiss(animated: true, completion: {
        self.delegate.add(self.createProductFromFormData())
    })
}
```

And thus the flow looks like this:

```
1. User taps on the quantity field, enters a quantity
2. User taps on the price field, the quantity field fires the editing-ended event, and `volatileFormData` is updated
3. User enters price, taps on the **Add** button
4. The view controller fires the `addButtonTapped` action, so the view controller prepares to dismiss itself
5. The price field fires the editing-ended event, and `volatileFormData` is updated
6. The view controller is now gone, so the completion block fires and creates the new `ProductItem`
```

Hurray!

I think this is a good time to stop and take stock of where the app is at. There's been a lot of forward progress, but it makes sense to have a look at the little paper cuts that are building up. Before going any further, I think it's worth reviewing all the code that's been written so far, and see how it can be cleaned up, re-organized, and —most importantly— thoroughly tested. So, tomorrow, I'm not writing any code; I'm going to create a list of `TODO`s, `FIXME`s and `HACK`s.

[Per]: https://droppedbits.com/apps/per
[here]: /tags/per-rewrite-diary/