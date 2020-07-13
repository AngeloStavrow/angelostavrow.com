---
title: "Web Monetization and Indigo"
date: 2020-07-13T12:00:00-04:00
draft: false
summary: Indigo, my IndieWeb-powered theme for Hugo, now includes web monetization.
tags: ["indigo", "projects"]
---

Today, I published the [v1.4.0] release of [Indigo], my theme for the [Hugo] static site generator (which powers this site as well).

## What's Web Monetization?

[Web Monetization] is a draft Web API spec that lets the user agent (i.e., your browser) stream payments to a website. I got to speak with a few people on their team, and the idea of a privacy-focused, patron-driven way for creators to generate revenue really spoke to me.

I then promptly got caught up in a bunch of other stuff, so when a couple of posts that were recently shared by Chen Hui Jing ([1], [2]) showed up in my feed reader, it reminded me that I wanted to add it to Indigo (as well as my own site).

Hui Jing explains it all in great detail, but in essence, the way this works is as follows:

- Someone signs up as a creator with a web monetization provider, like [Coil], and creates a wallet with an associated provider.
- The wallet provider generates a payment pointer for the creator.
- The creator adds this payment pointer to a specific `<meta name="monetization">` tag to the `head` of their site.

Once that's set up, anyone that's got a Coil subscription and is using a compatible browser will now micropayments to that creator whenever they visit that site. Hui Jing's site is monetized, as are [some others you may know].

## How to enable web monetization in Indigo

First, make sure you update the theme to v1.4.0, just as you would update any Hugo theme:

```
$ cd path/to/your/hugo-site
$ cd themes/indigo
$ git pull origin v1.4.0
```

Then, open your site's **config.toml** file and, under the `[params]` section, add:

```
paymentPointer = "$your.payment.pointer/here"
```

Build and deploy your site, and you're ready to go! If you've got any comments or questions, don't hesitate to reach out and let me know.

[v1.4.0]: https://github.com/AngeloStavrow/indigo/blob/master/CHANGELOG.md#140---2020-07-13
[Indigo]: https://github.com/AngeloStavrow/indigo
[Hugo]: https://gohugo.io
[Web Monetization]: https://webmonetization.org/
[1]: https://chenhuijing.com/blog/lets-talk-about-web-monetization/
[2]: https://chenhuijing.com/blog/monetizing-your-blog-with-coil/#%F0%9F%A6%8A
[Coil]: https://coil.com/
[some others you may know]: https://coil.com/explore