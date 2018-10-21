---
title: "Thoughts On Indigo, Two Weeks Later"
date: 2018-10-21T11:00:00-04:00
draft: false
tags: ["indigo", "projects"]
---

While v1.0 of my first Hugo theme, Indigo, was [released two weeks ago][indigo-released], it was only added to the [Hugo themes gallery][indigo-gallery-link] (and [tweeted about][indigo-tweet] from the Hugo account) five days ago. Since then, I've learned a couple of things that I thought I'd share.

<!--more-->

## Test, Test, Test

This is kinda obvious, but Indigo shipped with a shockingly silly bug. My assumption was that the Hugo site would always be setup at the site root, rather than in a subdirectory. The result of this is that some assets are referenced from the site root too; for example, if your site is at `example.com/blog` the request will 404 on page load, meaning that the resource can't be found — the HTML will be set up to load the fonts from `example.com/fonts`, but they'll _actually_ live at `example.com/blog/fonts` and therefore won't load properly.

**Lesson learned #1:** Well, you know what they say about assumptions. I built the theme for my own use, and then didn't test it against other scenarios. This is such an egregious mistake when building stuff for people that I feel really silly having made it.

Worse, the auto-generated demo site in the theme gallery also runs from a subdirectory, so naturally all those wonderful fonts are nowhere to be found.

**Lesson learned #2:** you can (and should) actually build the theme gallery and see how your theme will work _before_ you submit it. I'm adding that step to my testing workflow for future work on the theme.

## Open source is about clear communication

Within a day or two of being added to the gallery, I had contributions! [Jens Sauer][sauerj] opened a reported a few issues (including the one I referenced above) and opened a couple of pull requests.

This was super cool! Nothing I've released before really attracted the attention of others, so I was excited. It also made me realize that there's a lot of value in being very clear about contributions, using issue and PR templates in GitHub, and generally making the process as friction-free and easy for people to help improve your project.

**Lesson learned #3:** While there is [a `CONTRIBUTING` document][contributing] in the repository, I feel that it's not as well fleshed-out as it could be—probably because I wasn't really expecting anyone to actually, you know, _contribute_. I've [opened an issue][28] to fix that.

## It's still scary

This isn't the first thing I've made and released to the public, open-source or not, and it's far from being the most complex. It's just a fun side little project, built mostly in HTML and CSS.

And yet — it was scary.

I'm not sure it ever stops being scary. Maybe it just gets a bit easier to intercept the fear and react a little more appropriately. So.

**Lesson learned #4:** I'm not really sure. Don't be afraid of the fear, I guess.

<!-- Link references -->

[indigo-released]: https://angelostavrow.com/post/introducing-indigo/
[indigo-gallery-link]: https://themes.gohugo.io/indigo/
[indigo-tweet]: https://twitter.com/GoHugoIO/status/1051911902966763521
[sauerj]: https://github.com/sauerj
[contributing]: https://github.com/AngeloStavrow/indigo/blob/master/CONTRIBUTING.md
[28]: https://github.com/AngeloStavrow/indigo/issues/28