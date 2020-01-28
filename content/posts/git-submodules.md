---
title: "A use case for git submodules"
date: 2020-01-27T14:53:18-03:00
draft: false
---

Git submodules are a gray area to me: not because of how they work, but because of identifying a situation they suit up nicely, and that is, without feeling I'm complicating things unnecessarily. 

Some time ago I wrote [this article](http://somewhereintheweb.com), which is kind of a sloppy solution to a problem that deserved a better one. So, now I've got time to revisit it, and yes: this time we approach with (oh, you got it already!) git submodules!

Now, instead of running this hard to memorize and not so extensible command I came up with before:

```bash
find . -mindepth 1 -maxdepth 1 -type d -print -exec git -C {} pull \;
```

Now, I can just do: 

```bash
git submodules foreach command 
```
Where `command` would be anything we want to run across all the repos.   

So, this post is about solving what was already solved, this time with a better, more extensible approach: 
A simple yet useful approach with submodules.

## Git submodules 

So, this is what we'll be doing:

1. Create a new repo 
2. Add in every repo we want to manage as a submodule
3. Checkout a branch on every submodule/repo - one command!
4. Get the status of each submodule from top-level repo - one command!
5. Check all submodule repos status - one command!
6. Pull changes for every submodule - one command! 

Alright then, let's do it!

1. Create a new repo. cd into a new dir, and init a new repo. 

```
mkdir platform-repo && git init
```
2. Add every repo you want to control as a submodule (repeat as many times as repos you wish to add). Each repo will be mounted in a new directory (similarly as git clone does).

```
git submodule add repo_url
```

3. Now, we need to point our submodules to a branch. Depending on your git version, by default submodules have a detached head (meaning they don't point to any branch). I'm assuming you would be pointing them  to either `dev` or `master` branch, but you can point them to wichever branch you wish to.

```
git submodule foreach git checkout dev
```

4. Check the status of the submodules: from the 'top-level' repo, just run: 

```
git status
```
    It will show every submodule and its status. As you work on your submodules, you will see different statuses: new commits (since the moment you add them as submodules), untracked changes (not commited, nor staged), or yet pristine.  

     When a submodule shows new commits, it means it has them since the moment you added the submodule to the top-level repo. It works like this, because the top-level repo holds a reference to a commit in the submodule repo, and when it detects newer commits than the one it is holding its reference into, it shows the difference.
     
     If you want it to show no new commits, you just need to create a commit in the top-level repo. You can read more about that [here](https://unix.stackexchange.com/questions/214879/git-submodule-shows-new-commits-submodule-status-says-nothing-to-commit). 
     
     


5. If you want to see each submodule status as if it were an independant repo, you should:

```
git foreach submodule git status
```
    And you are golden. 

6. Pull changes for every submodule

```
git foreach submodule git pull --rebase --autostash origin branch_name
```
    This way, `--autostash` will assure to add your changes to stash, pull, then rebase on your branch and pop changes out of stash. And all of that automatically, so you don't need to commit or stash your changes manually. 

    This is useful because when running commands with the `git foreach submodule command` expression, if the command exits with an error code, it might cancel the whole foreach chain. This way we ensure it doesn't exit as an error, by reducing chances for failling of the pull.

## Other cases

For any other case, you can always cd into each submodule and see it still is a normal repository. So you can work from there as you usually do. Just keep that in mind when running git foreach submodule commands from the top level repo. I.E not all submodules might be in the same branch, etc.

## Conclusions

Here is another approach to work with many repositories at once. We could reach a similar thing if working with monorepos, but this wasn't an option at the place I was working at the moment I wrote this post. So, I found this approach, which you can maybe think about 'simulating a monorepo'. 

Just by adding a set of repos as submodules to a new one, you can work around it nicely. 

---
>You can read in depth about git submodules [here](https://git-scm.com/book/en/v2/Git-Tools-Submodules).