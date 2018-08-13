+++
date = "2016-06-17T12:00:00-04:00"
draft = false
title = "On moving to Hugo on GitLab Pages"
aliases = [ "/blog/2016/6/17/on-moving-to-hugo-on-gitlab-pages" ]
+++

This week, I took a hiatus from the [refactoring fun][1] I've undertaken to (finally) move the blog over to a static site generator.

[I've written before][2] about my plans to move to [Hugo][3], and that's now complete. Since the beginning of the year, all posts have been written as Markdown files that I can easily grab and format with the necessary front matter. Before then, I hadn't written very much, and in fact I've dropped a couple of old posts that I felt didn't add to the site in any way.

The site-generation files exist in a [GitLab repo][4]. I treat this site as I would any other software project; [issues][5] are opened to add or modify things (including posts), a branch is created to address these issues, and when the work is done, the changes are merged into `master`.

Whenever something happens on `master`, GitLab's CI kicks in and a shared runner picks up the job, builds the site, and uploads the `public/` folder to [GitLab Pages][6]. This is one of the easiest ways to set up continuous deployment for the site, but when I started trying it out [there was an open issue][7] to resolve for performance&mdash;it could take hours before the changes went live. That's gotten much better since GitLab 8.9RC3 was deployed, and I only make one post per week so it's not that big a deal&mdash;but if you need to keep things up-to-the-minute fresh it's not a bad idea to set up a project-specific runner instead to build and deploy your site.

The official deploy date of the site is&mdash;as you can see from the [`1.0` milestone][8]&mdash;Friday, July 1<sup>st</sup>.

One more thing: **the RSS feed for this site will be changing.** *If* you subscribe to the feed, **please update your link** to

```
http://makebeforebreak.com/index.xml
```

More tk!

[1]: /tags/tghr/
[2]: /blog/2016/1/21/on-moving-preparations
[3]: http://gohugo.io
[4]: https://gitlab.com/AngeloStavrow/makebeforebreak.com
[5]: https://gitlab.com/AngeloStavrow/makebeforebreak.com/issues
[6]: https://pages.gitlab.io
[7]: https://gitlab.com/gitlab-org/gitlab-ci-multi-runner/issues/1390
[8]: https://gitlab.com/AngeloStavrow/makebeforebreak.com/milestones/1
