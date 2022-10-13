---
layout: post
title: Installing Jekyll on Mac Os
---

## Install ruby & jekyll

I did steps from: https://jekyllrb.com/docs/installation/macos/

## Start by coping existing jekyll starter blog

Then I followed steps from https://github.com/barryclark/jekyll-now

After renaming my repository the blog showed up on https://kmorcinek.github.io/ !

### Problem with webrick gem

But I have problem with running

```zsh
jekyll serve
```

redirected me to:
https://github.com/jekyll/jekyll/issues/8523

Tryied `gem install --user-install webrick` , but it installed webrick for ruby 3.1.0 instead of 3.1.2, my comment: https://github.com/jekyll/jekyll/issues/8523#issuecomment-1277059466

### Other I have tried

From page:

https://mac.install.guide/ruby/7.html

I did:

Here's how to speed up gem installation by disabling the documentation step:

```zsh
$ echo "gem: --no-document" >> ~/.gemrc
```

## Using blog despite local developement issues

I will just write posts in markdown and after pushing to github, I can check the layout etc.
