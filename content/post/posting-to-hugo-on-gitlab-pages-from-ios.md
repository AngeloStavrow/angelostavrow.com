+++
date = "2016-07-08T12:00:00-04:00"
title = "Posting to Hugo on GitLab Pages from iOS"

+++

In moving this blog to a static site generator, one concern was whether I'd still be able to work on and/or publish a post from my phone. I tend to do a fair bit of mobile drafting while I'm in the subway or waiting in line, and will sometimes publish content when I'm away from my computer. YOLO.

### Requirements
It's a given&mdash;since that's how the site has been setup&mdash;that you need a GitLab.com account, and [Hugo set up with GitLab CI](https://gitlab.com/pages/hugo) to build the site and deploy it to GitLab Pages.

On my iPhone, I have [Working Copy](https://itunes.apple.com/ca/app/working-copy-powerful-git/id896694807?mt=8&uo=4), which I use for git operations on the repository. It's an excellent app, and when you pair it with [Editorial](https://itunes.apple.com/ca/app/editorial/id673907758?mt=8&uo=4)'s powerful workflow and text editing capabilities, you've got a great toolkit for repository management as well as writing.

I like using issues as jumping off points for new posts (including [this one](https://gitlab.com/AngeloStavrow/makebeforebreak.com/issues/4), so I also use [Git Trident](https://itunes.apple.com/ca/app/git-trident-for-github-gitlab/id983962683?mt=8) to manage this in GitLab from my phone. Matt's working hard on making this the best app for handling GitHub and GitLab issues on your iOS device, and I'm especially looking forward to offline access.

### Workflow

This post was a test for how to add content to the site from my phone, and here's how I'm going to do it from now on:

0. Usually (but not always), an issue tagged `post idea` is the basis for a new post. When I'm ready to write, I assign the issue to myself and set a due date for it.
1. I then create a new branch as `n_post_post-subject_yyyy-mm-dd` in Working Copy, where `n` is the issue number and `yyyy-mm-dd` is the post date. If no issue exists, I drop the `n_` prefix in the branch name. I try to avoid setting a title, as this can change over the course of the post. "Post subject" is already pretty well defined, since I know what I want to write about.
2. Next, I create a new text file in Working Copy under `/content/post`, and open it in Editorial via the share sheet to start writing. I have a [TextExpander](https://smilesoftware.com/TextExpander) snippet to add the TOML front matter; it's also set as a draft, to ensure it doesn't get published accidentally.
3. Then comes the hard part: write the post. Commit as necessary with Working Copy's workflow for Editorial. I commit and push for a "cloud save", or if I think I might want to continue elsewhere (i.e., on my desktop, or in a web editor<sup id="fnref:1"><a href="#fn:1" rel="footnote">1</a></sup>.
4. When it's all said and done, I set `'draft = false` in the front matter, commit and push with message `Closes #n`, where `n` is the issue number.
5. When I'm ready to publish, I launch Safari and go to GitLab.com to open a new merge request. Merging the draft branch into `master` will trigger GitLab CI; once the build passes, the post will be published. I also delete the original branch.
6. That's it! The post is published. Enjoy a tasty, refreshing beverage!

I wrote the majority of this post on my iPhone, although I'll admit that it does feel a bit silly to tap away on a 4.7" screen when I've got a 24" monitor and full-size keyboard to work with. Still, for drafting and editing, it's great, and in a pinch (or, say, on an iPad with an external keyboard), you can keep your blog running from your iOS device without issue.

<div class="footnotes">
  <ol>
  	<li class="footnote" id="fn:1">
    <p>Web editor, you say? Why yes! You can edit a file in GitLab, then commit the changes. Neat, huh?<a href="#fnref:1" title="return to article"> ↩</a></p>
    </li>
  </ol>
</div>