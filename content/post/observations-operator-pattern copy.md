---
title: "Mental model of maintaining a long-term git fork"
date: 2022-08-19T03:10:00Z
draft: false
---

## The problem

Sometimes you fork a project and cannot upstream your changes, but despite this, you want to stay up to date with upstream releases. You may also want to add some automation to reduce your maintenance burden.

## How to model the problem

We can break the work into a mental model of 4 steps:

1. Be notified when there is an upstream release
2. Pull in upstream changes
3. Pull in downstream changes
4. Create a downstream release

This model helps me understand why some work can be automated, and other work cannot.

## Automating the work

It is straightforward to automate 1 and 2. For example, you can make a periodic task that looks for new commits in the upstream branch, and creates a PR from the newest upstream tag in that branch. If the PR passes tests, it can automatically merge. You can be assigned to the PR, and the notification will let you know about the upstream release.

You can also automate 3 by applying some fixed set of downstream changes as part of the same PR. But the usual workflow for downstream changes is a PR that undergoes review.

If you do not, or cannot automate 3, then you should not automate 4. Consider this: An upstream release happens. You automatically open a PR to pull in upstream changes. You may still need to pull in downstream changes before you create a downstream release. You do not pull in downstream changes automatically. Therefore, you should not automatically create a downstream release as soon as you pull in upstream changes.

## My plan

I will try the following:

Run a period task that creates a PR into the downstream branch from the newest upstream tag in the upstream branch, configure the PR to automatically merge once tests pass, and assign me to the PR.

Once I am notified about the PR, I will pull in any downstream changes that are pending, and then create a downstream release.
