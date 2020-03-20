---
title: "The Per Rewrite Diaries: Day 49"
date: 2020-03-20T08:30:00-04:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I sort out the input views, sort of.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## Just Stick It In A Stack View

I've discovered that —at least the way I'm calling things— no matter what I do, I just can't resize a `UIPickerView` after it's been instantiated, and instantiating it with the frame I want just gets ignored. I need to dig further into this because I'm clearly doing something wrong, but as I'm only working on this an hour a day, that's slow going.

So, in the spirit of making progress and not getting sidetracked with details that ultimately aren't _that_ important, whenever I have to show my picker view, I call this method:

```
func showPickerView(frame: CGRect, sender: UITextField) {
      let stackView = UIStackView(frame: frame)
      stackView.axis = .vertical
      
      let safeInsetView = UIView(frame: CGRect(x: 0, y: 0, width: frame.width, height: view.safeAreaInsets.bottom))
      
      stackView.addArrangedSubview(pickerView)
      stackView.addArrangedSubview(safeInsetView)
      sender.inputView = stackView
  }
```

Fuck it. It works, so long as I set my calculator keyboard's frame height to `216` (the height of a picker view) plus the bottom safe inset area.

I hate magic numbers in my code, so I'll continue investigating this, but for now... it's fine.

I've decided that tomorrow will be my last day posting these. Fifty days of uninterrupted blogging... not bad. But I'm also pretty much at feature parity with the shipping version now, so I can start adding features instead.

So tomorrow, a wrap-up, and then back to your regularly scheduled programming.