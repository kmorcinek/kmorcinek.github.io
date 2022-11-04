---
layout: post
title: Why should you version your system settings in git
---

I am versioning my local setting for many years. It is easy to setup as I described [Versioning my home with git on mac](https://kmorcinek.github.io/Versioning-my-home-with-git-on-mac/)

## When it shines

Image the scenario (I had it)

* you have to setup connection to lets say AWS. Somebody helped you to setup this
* but after now using it for couple of month you cannot use it again. Something is broken. So another colleaque helps you, adding new configurations, maybe updating some of existing ones.
* after some time the way your company authenticates to AWS changes (now using saml2aws) and you have to update some of "settings". There is Knowledge Base (in Confluence) for it, but it is yet another way for doing it.
* in meanwhile the roles where updated and you have also adjust

Now when all of this happens without control (everything is just a text file) you can loose track of what changed. When having explicit commits for all changes I was able to spend much less time on debugging the issues when connecting to AWS.

## Other usefull scenarions

* I tryied to install something (oh-my-zsh non standard plugin) that required more settings changes in the system. I failed to install it correctly. As it was not crucial for me I could either remove all the changes `git reset --hard`, or create z commit and then revert `git revert HEAD` (I prefer revert so I know what I was trying) without corruping my setup.
* I am making a presentation about git and I don't want to use any git aliases and use full commands. I am just making one commit that removes all the aliases from `~/.gitignore`. Later I have that commit on a branch `remove-all-git-aliases` and can you it for next presentation. After the presentation I remove the commit from master.

## Some obvious things to version

* ~/.gitignore
* ~/.gitconfig
* setting for console/terminal (width, font, etc)
* .zshrc (and any other *rc configuration files for the tools you use)
* setting of you favourite editor (lets say Visual Studion Code)
* all the scripts that you develop for your own (or shared with your team) productivity

## What else I include there

* README.md - I always have a readme in my repos, there is always something you can write for future you or anybody using this repository. Describe any problems you encountered.
* things regarding vim, it is not standard so it have to be added explicit, ie. `~/.vimrc`, `~/.ideavimrc`, etc
* settings for Karabiner (stable keyboard customizer for macOS)
