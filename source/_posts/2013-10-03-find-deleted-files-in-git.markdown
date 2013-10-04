---
layout: post
title: "Find Deleted Files in Git"
date: 2013-10-03 19:42
comments: true
categories: [git]
---
Occasionally I'll remove some files from a git repository that I need to get back for one reason or another. Specifically, recently I was working on a project where I was working with a bunch of audio files that I reorganized and moved to separate directories (a.k.a. organized a bunch of stuff that was all thrown into a big heap of files). One of the many things that I love about git is that you are able to easily recover these files or directories, do what you need to them, then move them around some more, commit the "undelete" or just "redelete" them.

To accomplish this, you use the plumbing command `git rev-list` ([more info on plumbing vs. porcelain](http://git-scm.com/book/ch9-1.html)). Git's rev-list command is extremely important and ultimately used by multiple porcelain commands. Here's a great description of the basics from the _man_.

> List commits that are reachable by following the parent links from the given commit(s), but exclude commits that are reachable from the one(s) given with a ^ in front of them. The output is given in reverse chronological order by default.

> You can think of this as a set operation. Commits given on the command line form a set of commits that are reachable from any of them, and then commits reachable from any of the ones given with ^ in front are subtracted from that set. The remaining commits are what comes out in the command’s output. Various other options and paths parameters can be used to further limit the result.

So, if I have a file that I deleted a month or so ago called _foo.rb_ but for some reason I need to bring it back into my local repo. I can easily bring it back by running the following:
```
git checkout `git rev-list -n 1 HEAD -- foo.rb`^ foo.rb
```

So, that's obviously a bit of a long-winded shortcut. Let's break it down.

It's really two commands in one. Let's start with inside the ticks.
```
git rev-list -n 1 HEAD -- foo.rb
```

The _-n 1_ says "give me one revision". _HEAD_ represents the tip of the current branch. The bare double dashes (or _--_) represents my current branch. When all put together we are saying, print the latest commit hash starting from the commit I am currently on, in the branch that I am currently on, that modified _foo.rb_. The caret (_^_) is a shortcut that represents the previous commit to whatever it appends.

Once you grok that, the next part is pretty easy. Once interperted by the shell, the command is just a normal checkout from an alternate commit.

If none of that makes sense, feel free to comment and I'll try to clarify. When in doubt though, just trust me :).
