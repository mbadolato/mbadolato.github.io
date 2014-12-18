---
layout: post
title: "Notification when switching to Git Branches With Different composer.lock Files"
date: 2013-08-23 23:24
comments: true
categories: [php, composer, git, tips]
---

At my office, we frequently find ourselves switching between Git branches and often we forget to run ```composer install``` on the new branch to make sure our vender dependencies are at the proper versions for the branch we changed to.

For example, ```master``` is stable and has an older version of a dependency, but a feature branch I'm working on uses an updated version of that dependency. 

Let's say I'm currently working on a feature branch when a bug report comes in and I need to fix the problem ASAP. If I stash my current feature branch's files, then create a new bug-fix branch off of master, my ```composer.lock``` file is different and my ```vendor/``` directory still contains the newer version of the dependency. This occasionally causes problems.

A while back I found a helpful script to use as a post-checkout Git hook. Now whenever I switch branches, I get a notification telling me my dependencies are different than the branch I just switched from.

<img src="/images/misc/branch_switch.png" />

To use this script on a single project, put the following in your project's ```.git/hooks/post-checkout``` file (create it if it does not already exist). Make sure to set the file permissions to executable.

I prefer to keep the script in my global ```git-core/templates/hooks/post-checkout``` file so it's accessible for all of my projects. __Tip:__ After installing the file, run ```git init``` in an existing project to update the hooks and bring the global file down locally to your project.

```bash /usr/local/share/git-core/templates/hooks/post-checkout
#!/bin/bash
 
PREV_COMMIT=$1
POST_COMMIT=$2
 
NOCOLOR='\x1B[0m'
REDCOLOR='\x1B[37;41m'
 
if [[ -f composer.lock ]]; then
    DIFF=`git diff --shortstat $PREV_COMMIT..$POST_COMMIT composer.lock`
    if [[ $DIFF != "" ]]; then
        echo -e "$REDCOLOR composer.lock has changed. You must run composer install$NOCOLOR"
    fi
fi
```

_Please note: This script is modified to properly display color codes on a Mac/BSD system. If you're on Linux and having display issues, change the instances of ```\x1B``` to ```\e``` and it will work for you._