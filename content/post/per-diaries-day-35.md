---
title: "The Per Rewrite Diaries: Day 35"
date: 2020-03-06T08:30:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I fix the Add button's enable/disable behaviour.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## Fun With textDidChangeNotification

The first papercut I'm tackling since revisiting the list yesterday is a `FIXME` that affects the **Add** button in the product detail form view. We want the button to be disabled _unless_ the form has "valid input", which is to say, it's got _at least_ a price and quantity set.

The [`UITextFieldDelegate`](https://developer.apple.com/documentation/uikit/uitextfielddelegate) lets you hook into various events related to a text view, and the one we're most interested here is when the text was changed. I kicked off this work by running a callback on `textFieldDidEndEditing(_:)` method that would call a method in the form view's delegate, `updateVolatileFormData()` — this struct provides a temporary datastore for the price, quantity, and (optionally) units as `String`s from each text field.

Instead, on each change of the text in the form, I can call that same update method — and do a little bit of additional checking. _If_ the form data is complete, enable the **Add** button; _otherwise_, disable it.

So how do we hook in to this event? At the end of my form view's `setupView()` method, I subscribe to `textDidChangeNotification`:

```
func setupView() {
    /* Set up the controls and other views here */

    NotificationCenter.default.addObserver(
        self,
        selector: #selector(textDidChange(_:)),
        name: UITextField.textDidChangeNotification,
        object: nil
    )
}
```

And then I create that selector:

```
@objc func textDidChange(_ notification: Notification) {
    if let textField = notification.object as! UITextField? {
        switch textField.tag {
        case 100:
            datasource?.quantity = textField.text ?? ""
        case 101:
            datasource?.units = textField.text ?? ""
        case 102:
            datasource?.price = textField.text ?? ""
        default:
            print("Unknown tag")
        }
        
        delegate?.updateVolatileFormData()
    }
}
```

Notifications include an `object` property that you can pass to subscribers, and in this case, that `object` payload is the `UITextField` that triggered the notification (which is why I force-cast `notification.object` as a `UITextField?` here — remember that I'm subscribing to `UITextField.textDidChangeNotification`, so I'm pretty sure that cast is guaranteed to succeed).

Now, here's a fun fact.

For reasons that I'm not clear on, these notifications are not fired when you change the content of a text field like so:

```
someTextField.text = "blah blah blah"
```

Instead, if you want to set the value of your text field programmatically, you'll have to do it this way:

```
someTextField.text = ""
someTextField.insertText("blah blah blah")
```

_That_ will fire the text-changed notification. Because I'm using a picker view whose `didSelectRow:` delegate method sets the value of the units text field, I had to change it from the former to the latter approach.

I guess this makes sense because inserting/removing text counts as _changing_ the text that's already there, whereas the straight assignment in the first approach doesn't because that `String` object may not even exist yet? I'm not entirely sure, but it's been [a source of confusion](https://stackoverflow.com/a/59829100/1234545) in iOS for a while.

Anyways, with this work done, you can't crash the app if you hit the **Add** button before you've got appropriate input values in your form. The last bit of work to get this to feature parity with the shipping version of Per is adding an `inputAccessoryView` for navigating fields and enabling the simple calculator feature, so I'll kick off work on that tomorrow, then sort out all of the remaining papercuts before I move on to new v2.0 features (beyond being able to add multiple products, that is).