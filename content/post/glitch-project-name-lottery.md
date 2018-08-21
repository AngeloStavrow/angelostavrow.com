---
title: "Glitch Project Name Lottery"
date: 2018-08-21T05:45:07-04:00
draft: false
tags: ["glitch", "javascript", "projects"]
---

[Glitch](https://glitch.com) creates randomized project names when you remix a project, and they look something like `adjective-noun.glitch.me`. You can always rename these later, but oftentimes they’re just too amusing to bother (I’m looking at you, `victorious-beer`).

So, what if you decided that you were going to remix the node starter app, and then had to spend a couple of hours building an app based purely on the random name you were assigned?

Well, you’d end up with something like [Therapeutic Caribou](https://glitch.com/~therapeutic-caribou).

I’d been poking at [TextBlob](https://textblob.readthedocs.io/en/dev/) in Python over the weekend. Why not find an npm module that handles sentiment analysis to create a little app that takes in what you’re saying and replies based on how positive or negative the sentiment is. I’m using the [wink-sentiment](http://winkjs.org/wink-sentiment/) module to do this in analyzer.js, but it should be fairly easy to swap out any other analysis module in the `rankSentiment()`  function.

And while we’re at it, why not make it so that you’re just talking to the web app, rather than typing? I’ve been working through Wes Bos’ [JavaScript30](https://javascript30.com) course and, as luck would have it, just went through the native [SpeechRecognizer](https://developer.mozilla.org/en-US/docs/Web/API/SpeechRecognition) project.

Given that this is something that I threw together in a couple of hours, it’s very far from complete — our therapeutic caribou is a pretty lousy conversationalist, there are a couple of bugs around SpeechRecognizer events, and the design is… well, pretty unpleasant. Additionally, I’ve only tested this in Chrome, so I have no idea if it works as expected in other browsers.

With all that said, it was still a lot of fun to throw something together that let me play with a bit of both client- and server-side JavaScript, some neat native technologies like SpeechRecognizer, and some interesting concepts like sentiment analysis.

If you try playing the Glitch Project Name Lottery, let me know what you come up with!