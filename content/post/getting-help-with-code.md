---
title: "Getting help with code"
date: 2019-07-13T09:30:00-04:00
draft: false
tags: ["learning"]
---

Debugging code can be either _very rewarding_ or _extremely frustrating_. If you're feeling frustrated, here are a couple of tips that will make it easier for others to help you.

<!--more-->

## Bring a minimal, reproducible example

Create and share a sample project with as little code as possible to show what you're trying to do and how it's failing. This [Stack Overflow help article] goes into a lot of detail on this! You can put this in a [public GitHub repository] for helpers to look at. For web development, I'll create and share a [Glitch] project (naturally). [Swift playgrounds] can be a really great option for iOS/macOS development questions.

I do this even when I'm trying to debug my own code, to prove that the problem is what I think it is, and not a weird side effect from other code in the project. Sometimes, this step alone is enough to help you figure out what the issue was.

## Make your question as specific as possible

You're probably frustrated and tired and just want to get the code working as quickly as possible. It's worth it to take the time and put as much detail as possible into your question.

- What kind of project are you working on? Be specific: you might be writing JavaScript, but you might run into very different problems depending on whether it's part of a Node app, a React app, or plain ol' vanilla JavaScript in a static HTML page.
- Are you getting following a tutorial? Share the link. Maybe things have changed since the tutorial was written.
- Get back to the point where your project last worked, and explain step-by-step what you did from that point. Just like in a good bug report, describe _what you expected_ as well as _what is actually happening_. Remember to share the error messages you see!
- Provide details on what you tried to fix the issue. Links to tutorials or Stack Overflow answers that you tried can be helpful too.

This probably sounds like a lot of work, but there's value to it. First, writing this stuff out is a kind of [rubber duck debugging], letting you step outside of the editor and into a conversation about what's going wrong. Sometimes, taking the time to do this will help you figure out the problem yourself. That's because you're awesome!

And if it doesn't, you're still awesome! But the level of challenge you're facing is probably a little more advanced the skills you've built up so far. You'll get there, and asking for help is a great way to do that. 

The details you're providing by following the tips above is information that your helpers will probably ask you for anyhow, so by putting in that work you skip right to the good part: learning from others.

## Make room for answers

Sucessfully debugging a problem is as much about finding peace as it is investigation. As soon as I catch myself obsessing on why a particular line of code that _should be working_ is not doing what I expect, I minimize the code edito and leave the room.

I'll go for a short walk, or do some dishes, and let my mind wander. Sometimes that alone helps me figure out the problem!

But even if it doesn't, it changes where my head is at. I go from a place where I'm trying to _bend a thing to my will_ to a place where I'm _ready to receive new ideas_. I start writing down the points listed above. I create a sample project.

And then I can ask my questions from a position of patience and openness, because I've gotten rid of the frustration that's blocking me.

What tips do you have for getting help with code?

[Stack Overflow help article]: https://stackoverflow.com/help/minimal-reproducible-example
[public GitHub repository]: https://github.com/
[Glitch]: https://glitch.com/
[Swift playgrounds]: https://developer.apple.com/swift-playgrounds/
[rubber duck debugging]: https://en.wikipedia.org/wiki/Rubber_duck_debugging