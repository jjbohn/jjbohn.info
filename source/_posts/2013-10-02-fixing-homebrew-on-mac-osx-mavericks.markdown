---
layout: post
title: "Fixing Homebrew on Mac OS X Mavericks"
date: 2013-10-02 17:40
comments: true
categories: [mac os x, homebrew, mavericks, terminal]
---
I've had lots of problems with homebrew since upgrading to Mavericks. The same problems also existed when trying to `rbenv install` and really anything else that compiles. The root cause of the issue ended up being a conflict Mountain Lion Command Line Tools and the ones provided by Maverick.

There was an easy solution however. By running `xcode-select` (a tool provided to change the path to the current active developer directory) it will fix the issue.

Specifically, run this from your terminal:
```
xcode-select --install
```

This will bring up a dialog to install the Command Line Tools (which you will have to type in your password) and will fix the problem.
