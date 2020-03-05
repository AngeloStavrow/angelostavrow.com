---
title: "The Per Rewrite Diaries: Day 34"
date: 2020-03-05T07:00:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I revisit the list of papercuts.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## Revisiting the Papercut List

[Two weeks ago], I paused on writing code and took stock of the "papercuts" that I wanted to deal with before moving forward. Now that my work on automatic unit conversion is wrapped up, it seems like a good time to do it again.

That said, there wasn't much to add to the list beyond stuff I'd already noted here:

- The picker view code in `ProductDetailContentViewController` could be factored out into a separate class to make it a bit more maintainable. This isn't totally _necessary_, but it makes things a little cleaner and easier to maintain.
- One thing that _is_ necessary is to change the way I set units text field in the `ProductDetailFormView` when the picker's `didSelectRow:` value. I'm thinking of just adding a `setUnitsTextFieldValue()` method to the form view that can be called here.

This joins the outstanding issues:

- The **Add** button should only be enabled when the form has sufficient information to create a product.
- The `displayError()` method should be available to all view controllers.
- The form view's `UITextFieldDelegate` code should be refactored into a separate class.

In the last two weeks, I've closed three papercut issues:

- Created a custom table view cell for the product list.
- Marked `ProductList.add()` as `throws`.
- Refactored the UI layout code in the product detail content view controller.

So, I started with six papercuts, closed three, and am left with five. As Brent Simmons says, [bug math is weird].

Again, these aren't necessarily _bugs_, but they are quality-of-life improvements in the sense that it'll make it easier to maintain and reason about the code.

There's also one UX issue to think about. The product list view has buttons in the navigation bar, and the product detail view has them under the form. I _really_ dislike nav-bar buttons on anything but a 3.5" iPhone screen because of reachability issues, so I'd like to rethink how to combine this into, say, a floating button that changes functionality as you go from screen to screen. This isn't a goal for this function-focused initial rewrite, though.

In fact, beyond these papercuts there's only one feature left to implement to have feature parity with the currently shipping version of Per: adding a very simple calculator in an input accessory view.

Tomorrow I'll work on enabling the **Add** button only when there's enough form content to create a new `ProductItem`.

[Two weeks ago]: /post/per-diaries-day-20/
[bug math is weird]: https://inessential.com/2020/01/27/bug_math