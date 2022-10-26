---
layout: post
title: Versioning my home with git on mac
---

In Windows it is much simpler. You just start repo in your use directory.
(Artur - to verify, maybe there is the same problem?)

In Linux/Mac it is one problem. your home directory (`~/`) is the root for everything. When I create a folder:

```zsh
$ cd ~
$ git init
$ mkdir tests
$ cd tests
$ git status
```

Then folder `/tests` thinks it is under version control.

## Create a bare repo in different folder

But when we will create **bare** repository then we can remove that problem.

### Init

How to init:

```zsh
$ git init --bare ~/.githome
$ touch README.md
$ git --work-tree="${HOME}" --git-dir="${HOME}/.githome" add README.md
$ git --work-tree="${HOME}" --git-dir="${HOME}/.githome" commit -m init
```

### Alias

For later usages I suggest an alias (placed in `.zshrc`):

```zsh
alias gh='git --work-tree="${HOME}" --git-dir="${HOME}/.githome"'
alias githome='git --work-tree="${HOME}" --git-dir="${HOME}/.githome"'
```

(both are the same, I prefer shorter version, but maybe you prefer to be more explicit)

I don't have to type the full command every time. I can just use:

```zsh
$ gh add .zshrc
$ gh commit -m 'version .zshrc'
```

### Where to backup the code

Above is the way how to store it in local bare repository. Of course it will be much better when you push your changes to some private repo. I suggest creating private repository on Github or Gitlab (it is free).

### Not show the untracked files

```
$ gh config --local status.showUntrackedFiles no
```

The `--local` switch tells to apply this settings only to this repo and not for the whole system.

[//]: # "To test it I can create bare repo in other folder and have update alias for pointing to that repo (when I want to test it in ~ home dir). I can test the same in any other directory."

[//]: # "I have tried to also make an alias for 'gitk' but failed on that."

Thanks [Jakub Kozioł](https://github.com/pingwindyktator) for showing me that 4 yours ago.

More robust explanation:
[The Bare Repo Approach to Storing Home Directory Config Files (Dotfiles) in Git using Bash, Zsh, or Powershell](https://dev.to/bowmanjd/store-home-directory-config-files-dotfiles-in-git-using-bash-zsh-or-powershell-the-bare-repo-approach-35l3)