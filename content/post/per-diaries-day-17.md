---
title: "The Per Rewrite Diary: Day 17"
date: 2020-02-17T18:30:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I work on reading values from a child view's text fields.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per]. Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here].

## Delegating to delegates

With the form laying out the way I'd like, I now have to get the data that a user enters into the form from that view to the view controller it's embedded in. And what's the best way to do this? The delegate pattern!

(I'm kinda wiped out after a rough night, so this isn't going to be a big, in-depth post — and today wasn't an especially productive day of writing code, either.)

But here's the gist of it, implemented kinda hastily just to remind myself how this all works. There's a view controller (in this case, `ProductDetailContentViewController`) that adds a child view that has a bunch of `UITextField`s in it called `ProductDetailFormView`. Each of the text fields have a delegate that capture events like `textFieldDidEndEditing()` so you can _do stuff_, like formatting numbers or whatever. And you want your view controller to be able to get that data from the child view, but you have no way of knowing _what_ is in that child view.

So you create a delegate protocol for the view controller that the child view can talk to!

Very simply, here's what I've got right now:

```
protocol ProductDetailContentViewControllerDelegate {
    func createProductFromFormInput(_ product: String)
}

class ProductDetailContentViewController: UIViewController {
  // The class implementation
}

extension ProductDetailContentViewController: ProductDetailContentViewControllerDelegate {
    func createProductFromFormInput(_ product: String) {
        // Just print whatever we get back for testing purposes
        print(product)
    }    
}
```

Side note: I like handling delegate conformance in an extension like this because it keeps the class implementation itself nice and clean. If it's a simple protocol you could probably just have it all in your class, but that just feels like yet another path to massive view controllers if you're not careful.

Anyhow, in my child view, I just need to add this as a delegate:

```
class ProductDetailFromView: UIView, UITextFieldDelegate {
  var delegate: ProductDetailContentViewControllerDelegate?

  // The rest of the class implementation

  func textFieldDidEndEditing(_ textField: UITextField) {
    delegate?.createProductFromFormInput(textField.text)
  }
}
```

At a very high level, having the text field delegate call the `createProductFromFormInput()` method of the `ProductDetailContentViewControllerDelegate` when editing finishes raises a flag saying, "hey, I'm done with this text field, whoever owns this can do something with it now!" And so the extension I showed you above will catch that flag and say, "okay, cool, got it — let me print it to the console!"

Oh, one silly little thing that I forgot: when you declare your child view in the view controller's `viewDidLoad()` method, don't forget to tell it what its delegate is. 😅

Something like this:

```
class ProductDetailContentViewController: UIViewController {
  // Some of the class implementation

  override func viewDidLoad() {
    super.viewDidLoad()

    productDetailFormView = ProductDetailFormView()
    productDetailFormView.delegate = self

    // The rest of the viewDidLoad() stuff
  }

  // The rest of the class implementation
}
```

Now this isn't _super_ helpful because it just sends back whatever is in a text field when that text field loses focus. There are three different text fields in the form, so tomorrow I'll tackle differentiating between them, so that the quantity field sends back a quantity, the price field sends back a price, and the units field sends back units.

[Per]: https://droppedbits.com/apps/per
[here]: /tags/per-rewrite-diary/