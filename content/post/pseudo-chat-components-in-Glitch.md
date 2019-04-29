---
title: "Pseudo-chat components in Glitch"
date: 2019-03-28T15:15:00-04:00
draft: false
tags: ["projects", "glitch"]
---

[Ash Furrow’s blog](https://ashfurrow.com/blog/) is always a great read, and [Monday's thoughts](https://ashfurrow.com/blog/learning-from-other-programming-communities/) on learning from other developer communities was no exception.

<!--more-->

That post came from [conversations on Twitter](https://twitter.com/ashfurrow/status/1108692951348191232), which is a thing I see a lot on [Micro.blog](https://micro.blog): rather than some long string of short updates, people will go off and write up their thoughts in a long(er)-form post, and share it with conversation participants.

I’m big on syndicating out from your own little corner of the web, but Ash’s post reminded me of another thing that’s just _really really cool_ about hosting your own content: the brilliant pseudo-chat in the middle of the post that adds interactivity to the post.

{{< tweet 1110171153203646465 >}}

While it’s not some AI-powered chatbot, a scripted exchange like this still breathes life into an otherwise static page. It fulfills part of the promise of the web: going beyond text and image and experimenting with entirely new ways to engage with your audience.

Of course, we’re not all able to whip up something like this for our own blog posts and websites, which can put this kind of stuff out of reach of many bloggers.

For those authors, I created a Glitch app that recreates the experience for you to embed in your site, like so:

<div class="glitch-embed-wrap" style="height: 557px; width: 100%;">
  <iframe
    allow="geolocation; microphone; camera; midi; vr; encrypted-media"
    src="https://glitch.com/embed/#!/embed/embedded-chat?path=README.md&previewSize=100"
    alt="embedded-chat on Glitch"
    style="height: 100%; width: 100%; border: 0;">
  </iframe>
</div>

The content in the chat above is directly from Ash’s post, but you can [remix the app](https://glitch.com/edit/#!/remix/embedded-chat) and create your own conversation script by modifying `chatScript` in **script.js**, then embed it in your own website (or wherever). To remix it, just click on the button below:

<a href="https://glitch.com/edit/#!/remix/embedded-chat"><img src="https://cdn.glitch.com/2703baf2-b643-4da7-ab91-7ee2a2d00b5b%2Fremix-button.svg" alt="Remix on Glitch" style="height: 40px; width: 163px" /></a>

I’m also starting to curate a collection of Glitch apps like this, that act like a drop-in solution for augmenting blog posts in fun and useful ways. It’s a collection of just this one project at the time of writing, but you can find it [here](https://glitch.com/@AngeloStavrow/augmented-blogality) — and if you have any suggestions, let me know!