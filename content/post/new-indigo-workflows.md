---
title: "Improved Hugo theme workflows coming"
date: 2019-08-22T10:00:00-04:00
draft: false
tags: ["indigo", "projects"]
---

It's been over 10 months since I [released] my theme for Hugo, called [Indigo].

Some changes have been made since launch, but it always seems to take so much longer to build out new features or fix bugs than I'd like, and a recent [breaking change] in Hugo 0.57 (temporarily rolled back in 0.57.2) that I have to [fix] really drove that point home.

<!--more-->

Part of the long delay between updates comes from the fact that Indigo is a project that I don't work on daily (or even weekly), but also because I don't have a great workflow for a lot of the stuff that needs to happen in the development process.

Currently, here's my development process for making a change to Indigo:

1. Open the issue and implement the changes
2. Test the changes on a local version of the Indigo [example site]
3. Test the changes in a local version of the Hugo themes gallery
4. Update the CHANGELOG and README docs as necessary
5. Open and merge the PR
6. Communicate the changes somehow
7. Update my own personal site (which involves pulling apart the CSS for the customizations I've made)

It's all very manual and tedious work. It also doesn't help that I've made the process of opening GitHub issues and PRs more onerous than they need to be for a project of this (small) scale.

I'm thinking about how to fix this. Here's the plan, as it stands right now:

- Create and publish a demo site based on Hugo's example site. This will be rebuilt with each release of Hugo to check for problems. This takes care of points 2 and 3 above, as it's sufficient for the theme to work with Hugo's example site.
- This demo site will also serve as a place for theme updates, as individual posts. I mean, it's a blog — can't think of a better way to publish updates than, y'know, a blog! That takes care of point 6 above.
- To make it easier for everyone to share feedback on the theme and keep up to date what's being worked on, I've created a [changemap]. I'm also going to relax the Issue and Pull Request templates shortly.
- I've opened an issue to pull [custom theme styling] out of the main CSS file, so that it's easier for folks to customize their sites — including myself. That'll take care of point 7 above.

I'm looking forward to this stuff because it'll mean I can build greater velocity to push out fixes and features, and I'll update here as these things get done. In the meanwhile, I aim to push out a fix for the breaking change ASAP.

[released]: /post/introducing-indigo/
[Indigo]: https://github.com/AngeloStavrow/indigo
[breaking change]: https://github.com/gohugoio/hugoThemes/issues/678
[fix]: https://github.com/AngeloStavrow/indigo/issues/52
[example site]: https://github.com/gohugoio/hugoBasicExample
[changemap]: https://changemap.co/indigo-team/indigo-theme-for-hugo/
[custom theme styling]: https://github.com/AngeloStavrow/indigo/issues/48