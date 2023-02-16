---
layout: post
title: Target Deployment SOP
---

(you local flow of development does not have to be visible outside, so it is up to you when create your branch for PR / CodeReview).



I will use master branch name, but it is either main/master .



Create a PR from latest master branch

If your PR gets approved you can merge (the owner of PR - not Team Leader or any other role)

Quite ofter the master branch was updated with new commits. You need to rebase your changes on master. Not merge. git checkout master && git pull && git checkout my-pr-branch && git rebase master

maybe solve conflicts (but it is different topic)

push your changes but this time you need to force the push, cause it is not fast-forward. git push --force-with-lease

merge the PR

new version of code was deployed to DEV environment

Maybe ensure the “feature” works on DEV - it is “maybe” cause there are very different situation so I will just list the options:

test the flow on DEV

pass the tests to QA:

QA tests or:

QA writes automation

there is nothing to test (just improvements in code) and the suite of automation tests ensure nothing is broken

you fixed a bug and wrote a Unit Test for it. You believe it is enough and testing it manually will take too much time

Ensure it is deploy and possibly inform somebody who is interested in verifying or using it.

 maybe miro board for it?