---
title: "Moving Commits with Git Cherry-Pick"
date: 2020-08-07T13:00:00-04:00
draft: false
summary: "Or: how to fix your pull-request screw-ups."
tags: ["git"]
---

On the weekend, while [working on NetNewsWire], I ran into a source-control situation that made me spend a lot of time learning about how to move Git commits from one branch to another.

For whatever reason, I made my changes on the `main` branch and, once the work was done, I opened a [first pull request] against the `NetNewsWire/main` branch. [Maurice Parker] kindly reminded me that the PR should be opened against the `mac-candidate` branch, so I closed that PR and took a look at the situation.

Here's what my fork of the NetNewsWire repository looked like:

!["Branch diagram of origin vs. upstream commits for original pull request"](/images/2020-08-07/original-pr.png)

The branch named `origin` is my fork of the original, `upstream`, NetNewsWire repo. Here's what the branch diagram _needed_ to look like:

!["Branch diagram of origin vs. upstream commits for required pull request"](/images/2020-08-07/required-pr.png)

Right.

So, here's something you should know about my experience with Git: so long as I stick to a very simple, branch-commit-merge strategy, everything's fine. I've even figured out how to keep my forked `origin` updated and in sync with `upstream` repos, like a good contributor.

But anything more complicated than that? Ugh. 😬

I knew what I needed to do — I had to move my commits from my `main` branch, in order, onto my `candidate` branch, and then open a new PR. After spending about a half hour making a good, solid, mess of my fork, I _finally_ stumbled upon the answer: Git cherry-pick.

Like almost anything on the official Git documentation site, I _personally_ find the [docs on cherry-picking] unhelpful at best, inscrutable at worst — I'm more of a visual person, I guess, so I found this [StackOverflow answer] to be much more useful in figuring out what it means to cherry-pick a commit in Git.

The process is actually pretty straightforward:

```bash
# Switch to the branch you want to move the commits to:
% git checkout mac-candidate

# Cherry-pick each commit, in order, using their commit hash,
# until you've moved them all to the intended branch:
% git cherry-pick 12ab34c
```

That's... about it. Like I said, though, I'm a visual person, so I found it helpful to do this in [GitKraken] (you can use [this referral link] if you're interested in trying it out), where I can just right-click on the commit I want to move, choose cherry-pick from the context menu, and watch as it gets applied to my `mac-candidate` branch. It definitely made it easier to make sure I didn't miss any commits or screw up the order.

Once that was done, all I had to do was open a [new pull request] (against the correct branch, this time) and the work was done!

[working on NetNewsWire]: /post/learning-with-open-source-projects/
[first pull request]: https://github.com/Ranchero-Software/NetNewsWire/pull/2314
[Maurice Parker]: https://vincode.io/
[docs on cherry-picking]: https://git-scm.com/docs/git-cherry-pick
[StackOverflow answer]: https://stackoverflow.com/a/9339460
[GitKraken]: https://www.gitkraken.com/git-client
[this referral link]: https://www.gitkraken.com/invite/ojj1co6A
[new pull request]: https://github.com/Ranchero-Software/NetNewsWire/pull/2315