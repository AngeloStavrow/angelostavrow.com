---
title: "Stylin' Sidebar.js in Glitch"
date: 2018-10-28T15:00:00-04:00
draft: false
tags: ["glitch", "css", "meta", "projects"]
---

My main social network these days is [Micro.blog](https://micro.blog/), where I post short updates, thoughts, and images, then syndicate those posts out to the big social media silos. I have a hosted blog on the Micro.blog service for this, but because I want this site to be the canonical source for _all things Angelo_, I wanted to pull in that content here.

<!--more-->

If, like me, you use a [static site generator](https://gohugo.io) to build this site, you have to dynamically build a page on load with the latest updates. To do so, you can [call some JavaScript from Micro.blog](http://help.micro.blog/2016/sidebar-js/), which is exactly what I do on my [updates](https://angelostavrow.com/updates/) page. That’ll dump a bunch of unstyled content into your site, so I had to add some custom CSS to have it fit in with the rest of the site’s theme.

[I was asked for a tutorial on doing this](https://micro.blog/crossingthethreshold/956021), so I built out a quick little playground in Glitch with some comments in the `style.css` file to show how I’ve done this.

<div class="glitch-embed-wrap" style="height: 420px; width: 100%;">
  <iframe src="https://glitch.com/embed/#!/embed/microblog-sidebarjs-css?path=README.md" alt="microblog-sidebarjs-css on glitch" style="height: 100%; width: 100%; border: 0;"></iframe>
</div>

If you want your own copy of the playground, hit the Remix button and have a look at the README and CSS files for some guidance!