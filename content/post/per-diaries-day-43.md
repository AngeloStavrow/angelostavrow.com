---
title: "The Per Rewrite Diaries: Day 43"
date: 2020-03-14T10:30:00-04:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I get basic arithmetic working.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## I 💖 Property Observers

Slowly unravelling work on Per's calculator feature has been interesting. I had worked on getting the UX/UI of the calculator working _before_ getting the actual arithmetic sorted out, such that certain keys would be enabled or disabled based on state, and then tried to add the math functionality on top of that, which made for a very messy pile of code.

I went through and removed anything that enables or disables keys, and cleared out the big, ugly `updateResult()` method that handled the actual (attempt at) arithmetic. I'm pushing a lot of the work into a property observer, and that's probably not going to be the final solution for this class, but it's really handy for simplifying how a thing should react to a change.

At any rate, here's how it's working now.

Every time a key in the keyboard is tapped, a character is appended to a `currentString` property — either a digit, a decimal, or an arithmetic operator. The main use for this string is to track what's in the relevant `UITextField`.

I'm using the `didSet:` observer on that property to parse that string every time it's updated, such that I can set a pair of `Double`s for the left- and right-hand-side operands. This is done by checking a flag that tells me if an operator button was tapped. If that's false, we're setting the LHS operand; if it's true, we're setting the RHS operand.

When you tap an operator button, the class also makes a note of what that symbol was. Then, when you tap the equals button, it calls a much-simplified `updateResult()` method that looks at the value of the LHS and RHS operands, and performs the arithmetic based on the last operator button tapped (so, if you had tapped the `+` key, it'll add the operands) before resetting a few properties. The solution to that calculation is set as the new value of `currentString` (which sets this as a new LHS operand), and is also returned as a string (which the caller uses to update the target text field).

Oh, if you're creating a custom input view for your app and need a way to clear the text field, here's a handy recursive function you can use:

```
func clearTextField(_ sender: UIButton) {
    guard let isNotEmpty = target?.hasText else { return }
    if (isNotEmpty) {
        target?.deleteBackward()
        clearTextField(sender)
    }
}
```

Tomorrow, I'm going to add back automatic enabling and disabling of various keys based on the state of the keyboard.