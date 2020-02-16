---
title: "The Per Rewrite Diary: Day 16"
date: 2020-02-16T17:00:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I resolve some layout issues with the form view.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per]. Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here].

## Constraints

[Yesterday], I started subclassing `UIView` to get the setup and layout of the product-details form out of its parent view controller, but was having a heck of a time trying to get it to look the way it was supposed to.

Here's the situation. The view controller doesn't know the size of the child view that is being added, so that child view gets initialized with a [`.zero` frame](https://developer.apple.com/documentation/coregraphics/cgrect/1455437-zero) — that is, it's placed at the origin (0,0) of the view controller's bounds, with height and width equal to zero.

The child view has its layout constraints pinned to the top, left, and right of its `layoutMarginsGuide`. Cool.

Of course, as I realized, it's not enough to say `view.addSubview(childView)` and be done with it when we're doing our layout in code; the child view has its constraints relative to its `layoutMarginsGuide`, but it doesn't have any constraints set up in relation to its parent view!

That's mostly straightforward —add constraints to the top, left, and right of the parent view— until you try to pin another child view to the bottom of the form. Remember, that form was initialized with a `.zero` frame, so to UIKit, it's technically got zero height unless you add that constraint. And, at least with the standard UI controls I'm using, that height can be determined by the `intrinsicContentSize.height` property.

Pop quiz: what's the intrinsicContentSize of a `UIStackView`?

_Trick question! A stack view has no intrinsic size of its own. You've got to figure that out based on the intrinsic size of the controls_ within _the stack view._

Which is exactly what I did: I exposed a `formHeight` property in the child view that is computed by a function called `getHeight()`:

```
func getHeight(of stackView: UIStackView) -> CGFloat {
if stackView.arrangedSubviews.count < 1 { return 0.0 }

if (stackView.axis == .horizontal) {
    var heights = [CGFloat]()
    
    stackView.arrangedSubviews.forEach { subView in
        if (subView.isKind(of: UIStackView.self)) {
            heights.append(getHeight(of: subView as! UIStackView))
        } else {
            heights.append(subView.intrinsicContentSize.height)
        }
    }
    
    return heights.max() ?? 0.0
} else {
    var totalHeight: CGFloat = 0.0
    
    stackView.arrangedSubviews.forEach { subView in
        if (subView.isKind(of: UIStackView.self)) {
            totalHeight += getHeight(of: subView as! UIStackView)
        } else {
            totalHeight += subView.intrinsicContentSize.height
        }
    }
    
    totalHeight += CGFloat(stackView.arrangedSubviews.count - 1) * stackView.spacing
    
    return totalHeight
}
```

When you pass in a stack view, this function will either return the tallest arranged subview in a `.horizontal` stack view, or the sum of heights of all arranged subviews in a `.vertical` stack view, plus the spacing between them. If, as it walks through the arranged subviews, it finds another subview, it'll recursively call itself on _that_ stack view. It works really well for this use case!

So, now I can set the `heightAnchor` constraint to the the value of the child view's `formHeight` property, and I'm set — the layout looks fine, and tomorrow I can work on getting the values from the text fields to create the new `ProductItem`.

[Per]: https://droppedbits.com/apps/per
[here]: /tags/per-rewrite-diary/
[Yesterday]: /post/per-diaries-day-15/