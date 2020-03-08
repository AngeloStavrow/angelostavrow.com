---
title: "The Per Rewrite Diaries: Day 37"
date: 2020-03-08T10:00:00-04:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I continue working on the calculator keyboard.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## More Fun With Custom Input Views

[Yesterday] I started work on replacing the system decimal-pad keyboard with something more like a calculator (which lives in a class called `CalculatorKeyboard`).

What I'm learning is this: the easy part is adding the keys. The hard part is accounting for their behaviour.

For example: the operator keys (i.e., &plus;, &minus;, &times;, &divide;) and delete key shouldn't be enabled if there's no text in the text field. But I can't actually hook into the `UITextField` to see what the current text is programmatically; instead, I need to track that with each keypress.

Which seemed straightforward... until I realized that the user could tap the clear button in the textfield, and I'd have no way of knowing beyond constantly checking `target.hasText` or having something happen in the text field's `textFieldShouldClear(_:)` delegate method (I chose the latter option).

Or that the user _could_ type something into the field, navigate to another field, and then come back to that first field. If the initializer for the calculator keyboard doesn't account for this by having an argument for whatever the current text of the text field may be, then its internal representation of what is being type will be out of sync with the textfield's `text` property.

I also want the operator keys to be disabled just after one is tapped, but then _re-enabled_ if you add a number after that. That way, you can't enter multiple operators between operands in the equation.

So today's work largely focused on handling this kind of behaviour. I always feel a bit uncomfortable working on this stuff because I might miss an edge case somewhere, but using property observers to trigger updates helps a lot. It changes my mental model of the class to a state machine of sorts, which is a lot easier to sketch — and I've always found that if I can sketch out a decision tree or flowchart or whatever, I can write the relevant code fairly easily.

Tomorrow, I start work on implementing the actual calculation!

[Yesterday]: /post/per-diaries-day-36/