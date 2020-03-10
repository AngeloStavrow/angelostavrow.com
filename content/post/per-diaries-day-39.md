---
title: "The Per Rewrite Diaries: Day 39"
date: 2020-03-10T08:45:00-04:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I try to work some more on the calculator.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## Back To Work, Sorta

Another night of terrible sleep has me struggling to reason with the best way to get the calculator keyboard to actually, y'know, calculate.

So I'm taking baby steps: first, figure out when the user is entering a LHS (left-hand side) vs RHS (right-hand side) operand for the equation. I can do that with a Boolean flag (`isSettingLHSOperand`). Okay, that's working as expected.

Then, I need to keep track of each of those operands (two `Double`s, `lhsOperand` and `rhsOperand`), as well as the last operator symbol ("+", "-", "⨉", "÷") entered (a `String` called `lastOperatorSymbol`). From there, I can get my solution based on the operator passed in and move ahead with my work.

Yup, it took me an hour to struggle through this, and I attribute that squarely on having had only about 4 hours of sleep in the last two days.

I'm going to keep harping on this because it's important: if you have difficulty sleeping, do what you can to remedy it. It's easier said than done, I know — but it's important.