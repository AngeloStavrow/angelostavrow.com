+++
date = "2016-03-11T12:00:00-04:00"
draft = false
title = "On writing for GitLab"
aliases = [ "/blog/2016/3/11/on-writing-for-gitlab" ]

+++

Yesterday, an article I wrote on setting up [GitLab CI for your iOS projects](https://about.gitlab.com/2016/03/10/setting-up-gitlab-ci-for-ios-projects/) was posted on the GitLab blog.

Now, I'm a relative newcomer to continuous integration, but I know that it's valuable. And I also know that GitLab's feature is free, regardless of whether your project is public or private.

I also know that the best way to learn something is to show someone else how to do it—so I launched BBEdit and took notes as I worked my way through the setup, with the intention of posting something here about my experiences.

Then I discovered GitLab's [call for writers](https://about.gitlab.com/2016/01/26/call-for-writers/).

## GitLab runs GitLab on GitLab to continuously improve GitLab

GitLab is more than just a version-control system. It's a community. I deployed their Community Edition on a work server back around v3 and it's been in use ever since, partly because it's nice to have your own internal DVCS, and partly because of the predictable roll-outs of new versions (every month, on the 22nd), which makes scheduled maintenance easy to plan for.

But the key reason is that it's always been more than just a DVCS. I've watched GitLab grow from its humble beginnings to, in my mind, one of the most interesting open-source projects out there.

And you can see the value of this kind of Extreme Dogfooding in the bug report response time and the cool new features that come around with every point release. But you also see it in their [strategy document](https://about.gitlab.com/strategy/). And their [team handbook](https://about.gitlab.com/handbook/).

## It all starts with an issue

Whether talking about [a bug report](https://gitlab.com/gitlab-org/gitlab-ce/issues/12712) or a blog post, [everything starts with the opening an issue](https://about.gitlab.com/2016/03/03/start-with-an-issue/).

After applying to their call for writers, I heard back from Heather, GitLab's Developer Marketing Manager, who assigned me to [an issue on the topic](https://gitlab.com/gitlab-com/blog-posts/issues/29). I proposed an outline of the post, and after some discussion and clarification on the topic, I forked the GitLab-website repo and began work on my draft.

Everything is done publicly, in the open. This means that everyone can contribute, and you can easily send links to conversations and drafts to peers for comment and review.

## WIP it good

And so the writing commenced. A few days later, I [opened a Merge Request](https://gitlab.com/gitlab-com/www-gitlab-com/merge_requests/1567) with a commit of my first draft. By adding `[WIP]` (**W**ork **I**n **P**rogress) to the MR title, GitLab prevents it from being accidentally merged to the master branch until the diff has been discussed and reviewed by the team.

Heather started with a first review of the post for general formatting and style according to the [Style Guide](https://gitlab.com/gitlab-com/blog-posts/blob/master/STYLEGUIDE.md), made a patch to correct some line breaks, and then assigned it for technical review to Kamil, author of the GitLab Runner.

Kamil made some comments and asked some questions; I responded and committed an updated draft, which updated the MR. These changes were reviewed, and the process continued until the post was ready.

At that point, all that was left to do was [remove the `[WIP]` comment from the MR](https://gitlab.com/gitlab-com/www-gitlab-com/merge_requests/1567#note_4185659), and the post was ready to go up on GitLab's blog.

Which it did, yesterday.

## Closing thoughts

Whenever I take part in some community event, I ask myself one question:

Would I participate again?

In the end, that's all that matters. I want to feel like I'm making a contribution, that I'm learning, and that the community I'm working with is working towards some positive change.

Contributing to GitLab, even with just a small blog post, feels that way. In fact, it's only increased my interest in moving my own projects to their service, and working with them towards improving GitLab however I can, be it bug report or blog post.

Actually, maybe I should start learning Ruby to contribute even more. 🤔