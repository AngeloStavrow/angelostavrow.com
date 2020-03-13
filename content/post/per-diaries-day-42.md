---
title: "The Per Rewrite Diaries: Day 42"
date: 2020-03-13T08:00:26-04:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I continue work on the calculator feature.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## Re-visiting The Calculator

On [Wednesday], I laid out how the calculator feature would work. Then, [yesterday], I added an equals button to the calculator keyboard, which changed how this all works. Here's how it _should_ go:

1. The user taps on a text field:
    - if it's empty, proceed to step 2.
    - if it's already got a first number (LHS operand) in it, proceed to step 3.
2. Enter the LHS operand via the keypad; it shows up in the text field.
3. Tap an operator symbol on the keypad; it shows up in the text field.
4. If the user then:
    - switches to a different field, remove the operator symbol from the text field and go back to step 1.
    - enters a second number (RHS operand), proceed to step 5.
5. As the user enters the RHS operand, it shows up in the text field.
6. If the user then:
    - switches to a different field, perform the arithmetic and replace the text field contents with the result.
    - taps the equals button on the keypad, perform the arihtmetic and replace the text field contents with the result as a new LHS operand, then append the new operator symbol to the text field.

This is proving a little bit more complicated to untangle than I'd expected. The first equation entered calculates correctly, but then I've got too many flags and temporary variables to keep track of, and inevitably things get mixed up.

Tomorrow, I'm going to try to sketch out a better approach for this.

[Wednesday]: /post/per-diaries-day-40/
[yesterday]: /post/per-diaries-day-41/