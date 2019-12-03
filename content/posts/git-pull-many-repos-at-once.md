---
title: "Git pull multiple repositories at once"
date: 2019-12-03T14:16:22-03:00
draft: false
---

I usually work with many repos at the same time and generally group them wihtin a same directory, named after the company. 

In example, if I'm working for "Great Employer INC." I'll have a directory probably named `greatemployerinc` and one directory for each repo I work within. Something like:

```
greatemployerinc
├── greatemployerinc-web
├── greatemployerinc-auth
├── greatemployerinc-core-service
├── greatemployerinc-email-service
├── ...more repos
└── greatemployerinc-gateway

```

Instead of navigating each one of them with `cd` and then manually run `git pull branch`, I came out with the following command that I run from `greatemployerinc` -the repos containing directory.

```
find . -mindepth 1 -maxdepth 1 -type d -print -exec git -C {} pull \;
```

It will find any subdirectory in the current one (greatemployerinc), without going deeper than the first level (delimited by `-mindepth 1` and `-maxdepth 1`) and execute  a `git pull` in the corresponding git dir (if its a repo). 

>If you want to see more on the `git -C` option, you can check [this post on SO](https://stackoverflow.com/questions/5083224/git-pull-while-not-in-a-git-directory) 

Keep in mind it will run a *git pull* on the current branch that each repository is actually stepping on. Also, consider the pull might fail if you have pending changes (it will be shown on the console output, anyway) and you might need to handle those cases manually, or considering adding an `--autostash` option to the provided command. 

## Wait, I can't memorize that command!
Then you and I have something in common :) 

To avoid memorizing it, I created an alias. If you are working with ZSH you can just add this line to your `~/.zshrc` file

```
alias multipull="find . -mindepth 1 -maxdepth 1 -type d -print -exec git -C {} pull \;"
```

Then close your current terminal and open a new one (so the changes on `.zshrc` take effect), go to your equivalent `greatemployerinc` directory and do:

```
multipull
```

And drink your favorite infusion while your repos pull from remote :) 