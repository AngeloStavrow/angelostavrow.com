---
title: "The Per Rewrite Diaries: Day 40"
date: 2020-03-11T07:30:00-04:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I rethink the calculator feature.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## What A Difference A Dream Makes

I got a pretty good night's sleep last night, so I'm feeling much better today as far as being able to think clearly. Before I just start writing code, here's the plan for how the calculator should work from a user's point of view:

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
    - taps an operator symbol on the keypad, perform the arihtmetic and replace the text field contents with the result as a new LHS operand, then append the new operator symbol to the text field.
    
This works out for the current state of the calculator keyboard, but I'm not thrilled about that last step: it feels like there should be an equals button ("=") that the user can press at any time to perform the arithmetic and update the text field to the result.

Another thing that the current shipping version of Per does is only allow one operator symbol to show at any given time. In other words, you have to solve the two-operand equation before you can continue, and this is enforced by disabling the operator symbols. Operator precedence is tricky to reason about (ever second-guessed yourself answering a "skill-testing question" on a contest?) so Per removes that option entirely.

Combining these approaches, I could add an equals button to complete arithmetic, and also disable other operator symbols (except for the equals key) whenever the user is entering a RHS operand. The only ugly part here is how to add a single button to the currently well-balanced 16-key grid. Here's what I'm thinking, using one of my terrible ASCII layouts:

```
 ----------------------
|                      | + |
|                       ---
|                      | - |
|                       ---
|                      | ⨉ |
|     NUMERIC KEYS      ---
|                      | ÷ |
|                       ---
|                      |   |
|                      | = |
 ---------------------- ---
```

So, a separate set of stack views for the numeric keys, and then another set of stack views over to the right for the operator keys and a double-tall equals key.

It's alwayhs better to work from a plan. Looking forward to kicking off the implementation tomorrow!