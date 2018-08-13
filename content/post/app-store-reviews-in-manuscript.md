---
title: "App Store Reviews in Manuscript"
date: 2018-03-13T04:00:00-04:00
draft: false
---


I've got some iOS apps as side projects (they're in dire need of updates, but that's neither here nor there). To keep things organized, I use a [Manuscript][manuscript] (_née_ FogBugz) account that I opened six years ago for planning work and, more relevant to this post, to capture (almost) all customer feedback. There's a custom mailbox set up with an email address in my business domain, to which customers can send questions and comments about the apps. 
These emails generate cases that I use for correspondence, and I can then open feature requests, bug reports, and known issues as linked cases in the relevant project.

I say "almost" all customer feedback, though, because not everything gets captured in Manuscript.

What’s missing? You don't get notification emails from Apple for new App Store reviews, so I can't pipe these into Manuscript. Wouldn't that be nice? Instead, you have to check for new reviews manually, across App Stores in all countries where your app is sold, or rely on yet another service. I don't know about you, but I've got enough inboxes to deal with.

Enter [Manuscript integrations][integrations], powered by [Glitch][glitch]. At their core, integrations let you create microservices in Glitch that communicate between Manuscript and some external service.

And thus came the idea for this project: since every app on the App Store (and Mac App Store) has an RSS feed of its reviews, I could create an integration that checks your app’s feed and, if there’s something new, pull it into a new case in Manuscript.

I'll be working on this in [Glitch][glitch-app], and tracking it in [GitHub][github-repo]. As of the time of this writing, the Glitch project is just a remix of the [sample Manuscript integration][hello-manuscript], but the work has been planned out; I’ve set a release date of March 25<sup>th</sup> for v1.0 of the integration, giving me two weeks to build and test the thing. While I’m not especially concerned about the ship date slipping, not having any ship date _at all_ usually means that a project doesn’t get any priority on my calendar.

Last week was all about defining the project itself and the work to be undertaken, and this week is about getting started on the actual code. If you're interested, you can read over the draft [functional spec][fspec] and [technical spec][tspec]. It's open sourced under the MIT license. I’ll be working on this in the open, which I have feelings about (read: _anxiety and trepidation_), and I’ve also set up a development journal in [Day One][day-one], where I add short weekly retrospectives on wins and losses for the week.

More to come as I make progress against the milestone!

<!-- Links -->
[manuscript]: https://manuscript.com
[fogcreek]: https://fogcreek.com
[integrations]: https://medium.com/make-better-software/how-to-get-started-making-manuscript-integrations-6da236b68f98
[glitch]: https://glitch.com
[glitch-app]: https://glitch.com/~app-store-reviews-in-manuscript
[github-repo]: https://github.com/AngeloStavrow/app-store-reviews-in-manuscript
[hello-manuscript]: https://glitch.com/edit/#!/hello-manuscript
[fspec]: https://github.com/AngeloStavrow/app-store-reviews-in-manuscript/wiki/functional-spec
[tspec]: https://github.com/AngeloStavrow/app-store-reviews-in-manuscript/wiki/technical-spec
[day-one]: http://dayoneapp.com
