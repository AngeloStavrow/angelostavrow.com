---
title: "The Per Rewrite Diaries: Day 22"
date: 2020-02-22T14:30:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I throw... errors.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## Marking As Throw

Today's work involves a `FIXME` for one of my biggest software pet peeves: silent failures. If I try to add a `ProductItem` with, say, kilogram units to a `ProductList` of sold-by-volume `ProductItem`s (millilitres, pints, and such), that shouldn't work. The UI will protect against this by pre-selecting available units, but the product list doesn't (and shouldn't) know this — it should protect against this by throwing an error if someone tries this particular operation.

So first, I set up an `enum` that conforms to `Error`:

```
enum ProductListError: Error {
    case units_mismatch
}
```

Then replaced the current functionality of printing an error to the console:

```
print("Can't add an item with type \(String(describing: incomingUnitType)) to a list of \(String(describing: _unitType)) items")
```

with the throwing of an error:

```
throw ProductListError.units_mismatch
```

Great! Now that the bad path throws an error, I can handle it at the call sites.

## Do Try Catch

To do this is pretty straight forward; just wrap the call to the throwing function in a `do`-`try`-`catch` block. In the `ProductListContextViewController`, we have an `add()` method that's called by the `ProductDetailContentViewController` via the delegation pattern:

```
func add(_ item: ProductItem) {
    do {
        try contentViewController.productList.add(item, sort: true)
        clearListBarButtonItem?.isEnabled = true
        contentViewController.loadView()
    } catch ProductListError.units_mismatch {
        print("Can't add an item with type \(String(describing: item.units)) to the list.")
        return
    } catch {
        print("Something went wrong adding \(item); abandoning the attempt.")
        return
    }
}
```

This will `try` to add the item to the product list; if that fails, it'll `catch` the error thrown, print an error to the console, and `return`.

Yes, this is technically still failing silently. But now that it's at the UI layer, I can add a simple alert for now and use _that_ instead of the call to `print()`:

```
private func displayError(_ message: String) {
    let alert = UIAlertController(title: "Whoops!", message: message, preferredStyle: .alert)
    alert.addAction(UIAlertAction(title: NSLocalizedString("OK", comment: "Default action"), style: .default, handler: { _ in
      NSLog(message)
    }))
    self.present(alert, animated: true, completion: nil)
}
```

_Now_ we're failing verbosely! I'm adding a `TODO` here to refactor this spike solution for the alert into an extension on `UIViewController`, because our time for today is up.

Tomorrow, I'm going to start work on a custom table view cell for the product list.