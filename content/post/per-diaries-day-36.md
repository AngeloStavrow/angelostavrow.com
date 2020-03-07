---
title: "The Per Rewrite Diaries: Day 36"
date: 2020-03-07T15:51:06-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I start working on the calculator feature.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## Custom Input Views

In the currently-shipping version of Per, I use an `inputAccessoryView` that adds a couple of buttons for things like navigating between fields and, more importantly, keys for performing simple arithmetic.

I started off my work today in re-implementing this and then realized that, hey, rather than doing this, why not instead replace the system's `.decimalPad` keyboard that I'm currently using with a calculator-type keyboard?

It could look something like this:

```
 --- --- --- ---
| 1 | 2 | 3 | + |
 --- --- --- ---
| 4 | 5 | 6 | - |
 --- --- --- ---
| 7 | 8 | 9 | ⨉ |
 --- --- --- ---
| . | 0 | ⌫ | ÷ |
 --- --- --- ---
```

In case the poor ASCII art doesn't make it very clear, the intention is having the decimal keypad, along with an extra column of arithmetic keys along the right.

One thing I got some comments on was that people didn't really take note of the calculator feature, likely because the keys in the input accessory view were too subtle. Doing it this way should be more prominent and hopefully improve feature adoption.

I started working on a `CalculatorKeyboard` class based on [this Stack Overflow answer] and it's working nicely well so far. It also gives me the opportunity to skin the keyboard to better match the rest of the app's design later on, though the picker view will probably look out of place if I go too far on this. Can you even skin a picker view? I haven't really looked into this.

With the system decimal keypad's functionality replaced, I'll start work on adding the calculator keys and their functionality tomorrow.

[this Stack Overflow answer]: https://stackoverflow.com/a/57275689/