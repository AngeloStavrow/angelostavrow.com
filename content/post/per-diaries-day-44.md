---
title: "The Per Rewrite Diaries: Day 44"
date: 2020-03-15T16:15:00-04:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I wrap up the calculator feature.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## One Step Closer

A couple of little calculator-related papercuts were sorted out today.

1. The math operator keys ("+", "–", "⨉", "÷") now enable themselves when there's a left-hand-side operand for the equation, and disable themselves when both operands are present (or nothing is in the text field at all).
2. Tapping into the next text field will solve for any equation entered into the current text field (or, if you've got something like "`12+`" entered, it'll clear the "`+`" symbol from the text field).
3. Deleting through an equation no longer crashes the app when you delete the operator symbol.

The only thing I want to finish up before installing going back to clean up any other papercuts is to add an input accessory view —the toolbar that sits just on top of the iOS keyboard, like a hat— that includes a previous and next button to skip between fields, which I'll add tomorrow.