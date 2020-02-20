---
title: "The Per Rewrite Diaries: Day 20"
date: 2020-02-20T09:00:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I prepare a list of papercuts.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## A List of PaPerCuts

We're talking about "papercuts" and the name of the app ("Per") is in "papercut" — get it?

Okay, sorry — bad joke. What do I mean by papercuts? These are the little rough edges and tiny bugs that make it annoying to use something. Pile up enough of them, and you have a [death by a thousand papercuts](https://english.stackexchange.com/a/326717) — the failure of an effort due to a multitude of fairly minor bits of unpleasantness.

### Models

The `ProductItem` and `ProductList` models feel fairly complete, but the list model only prints a message to the console if you try to add a product item whose units don't match what's already in the list (e.g., adding a product whose units are grams to a list full of items that are otherwise sold by volume). It should instead, throw an error.

### Coordinator View Controllers

`ProductListCoordinatorViewController` takes advantage of extensions on both `UIViewController` and `UIView` to embed a child view controller into the full frame of the parent view controller. I'll be on the lookout for other places where I can use that. Otherwise, this is a tiny, 20-line view controller. Perfect.

### Context View Controllers

The only context view controller is `ProductListContextViewController`, which also leverages the same extensions that the coordinator does. It's also a fairly small view controller, only 56 lines, but I want to rethink the use of `UIBarButtonItem`s for adding to and clearing the list of products — this could be refactored into some kind of child view.

### Content View Controllers

The `ProductListContentViewController` is a table view controller using a very basic cell to display results; that should absolutely be refactored into a custom view.

The `ProductDetailContentViewController` is kinda sloppy — some of its UI is from an embedded custom view, and some of it is created in `viewDidLoad()`, which should _at least_ be refactored out into separate `setupView()` and `setupConstraints()` methods. The **Add** button should only be enabled if there's text in both the quantity and price fields for now.

There's also maybe something to think about in combining or re-using the UI from the add-item/clear-slist `UIBarButtonItem`s and the **Add**/**Cancel** buttons in the `ProductDetailContentViewController`.

### Custom Views

Finally, we come to the `ProductDetailFormView`. This one needs work — if I want to enable/disable the **Add** button based on how much of the form is filled out, I'll need to do more `UITextFieldDelegate` work, which means duplication a lot of the logic that identifies which text field is being edited. That's not too hard to refactor.

I'm still setting aside any handling of units until I better understand how it works in practice. But it would be helpful to start adding some formatting for the textfields, especially currency. It feels like it's worth refactoring any text field delegation into a separate class here, to better encapsulate these changes.

### Other Thoughts

There are comments that explain things that probably don't need to be explained in the body of a code block, and should instead be made part of the function's documentation.

There are also no tests, whatsover.

I'm not adding these as papercuts, though — that's a whole other thing to tackle.

## Working down the list

Xcode's jump bar (or whatever it's called) along the top of the actual code-editing view in the Editor is really useful for this kind of work; if you use comments that start with `// TODO:` or `// FIXME:`, you'll see them listed there among your functions, letting you navigate to them very quickly.

!["The Xcode jump bar shows FIXME and TODO comments"](/images/2020-02-20/jump-bar.png)

Alternatively, you can mark them as `#warning("TODO: Description of task")` or `#error("FIXME: Description of problem")` to have these come up as either a ⚠️&nbsp;**warning** or an 🛑&nbsp;**error** in the Issue navigator.

!["Xcode's Issues navigator shows comments marked as #warning or #error"](/images/2020-02-20/issues-navigator.png)

It feels awkward to me to set these as compiler warnings —that's absolutely _not_ what they are— so I'm sticking with the comments. Instead, I'm going to add them as issues to work on.

Tomorrow, I start working on this list!