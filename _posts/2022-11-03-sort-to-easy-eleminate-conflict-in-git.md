---
layout: post
title: Sort to easy eliminate conflicts in git
---

We will solve a problem where there are many concurrent Pull Request that adds something to the end of file or an array.

You want to add a new hired employee to have access to a VPN or create him account in your company github/gitlab.

There is a devops culture and to achieve that you have to create PR with new email.

existing list:

``` javascript
var allowedUsers = [
    "stephen.king@example.com",
    "george.r.r.martin@example.com",
    "j.k.rowling@example.com"
]
```

Now there is new month and we have 5-20 new hires. Distributed amond teams and now each team lead is adding person(s) at the end of file.
Agatha Christie is joining ...

``` javascript
var allowedUsers = [
    "stephen.king@example.com",
    "george.r.r.martin@example.com",
    "j.k.rowling@example.com",
    "agatha.christie@example.com"
]
```

And if at the same time "Jules Verne" will join then we have a conflict.

Now somebody will have to resolve that conflict that will lead to:

* for sure somebodys time and interuption and longer waiting for somebodys else access to work environment
* maybe some mistakes

There is also problematic coma at the end ...

In some of the formats of programming languages you can have the comma at the end or even without commas (just new lines), but still you have a conflict in one line.

## The solution is to keep the entries always sorted

``` javascript
var allowedUsers = [
    "george.r.r.martin@example.com",
    "j.k.rowling@example.com",
    "stephen.king@example.com"
]
```

then adding Agatha ...

``` javascript
var allowedUsers = [
    "agatha.christie",
    "george.r.r.martin@example.com",
    "j.k.rowling@example.com",
    "stephen.king@example.com"
]
```

will not conflict with adding Jules

``` javascript
var allowedUsers = [
    "george.r.r.martin@example.com",
    "j.k.rowling@example.com",
    "jules.verne@example.com",
    "stephen.king@example.com"
]
```

## When it hit me

Once we were with couple people debugging a problem for 1,5 day. When we found a root cause it was exactly that in one configuration file, one conflict was not correctly resolved.

But it hits me a bit every time somebody in the team have to resolve small conflicts that otherwise could be avoided.

## Inspiration

[Adding new project to **Good First Issue**](https://goodfirstissue.dev/) where the file [repositories.toml](https://github.com/deepsourcelabs/good-first-issue/blob/master/data/repositories.toml) is sorted (and this is required by build process)

[//]: # "one argument. maybe you want to know in which order something was added. But do you really need it?"

[//]: # "https://github.com/git-warsztaty/misc-files repo where I have PR s. Add pictures to the post."
