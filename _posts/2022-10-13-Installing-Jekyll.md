---
layout: post
title: Installing Jekyll on Mac Os
---

## Install ruby & jekyll

I did steps from: [Jekyll on macOS](https://jekyllrb.com/docs/installation/macos/)

## Start by coping existing jekyll starter blog

Then I followed steps from [Jekyll Now](https://github.com/barryclark/jekyll-now)

After renaming my repository the blog showed up on [kmorcinek.github.io])https://kmorcinek.github.io/) !

### Problem with webrick gem

But I have problem with running

```zsh
$ jekyll serve
```

redirected me to:
[Jekyll serve fails on Ruby 3.0](https://github.com/jekyll/jekyll/issues/8523)

The solution was to run

```zsh
$ bundle add webrick
```

but I don't know where to run it :(

---

Tryied `gem install --user-install webrick` , but it installed webrick for ruby 3.1.0 instead of 3.1.2, my comment: <https://github.com/jekyll/jekyll/issues/8523#issuecomment-1277059466>

### Other I have tried

From page:

[Install Ruby 3.1 · macOS](https://mac.install.guide/ruby/7.html)

I did:

Here's how to speed up gem installation by disabling the documentation step:

```zsh
$ echo "gem: --no-document" >> ~/.gemrc
```

## Using blog despite local developement issues

I will just write posts in markdown and after pushing to github, I can check the layout etc.
