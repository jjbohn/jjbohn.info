---
layout: post
title: "Installing Ruby with rbenv"
date: 2014-07-20 15:19
comments: true
categories: ruby beginners
description: “How to install rbenv and multiple versions of Ruby”
---

## The need for multiple versions of Ruby

As you work on different Ruby project over time, you’ll find that different
projects projects require different versions of Ruby. To handle this, a few
tools have been created. My personal favorite is rbenv. I recommend all rubyists
use it even if you’re just starting out. If makes installing new versions, tying
certain versions to certain projects, and much more a breeze.

## rbenv vs rvm

Besides rbenv, there is another ruby version manager that has been out for some
time and at one point, was the de facto standard. That tool was
[RVM](https://rvm.io/). RVM is still fairly popular although it’s popularity is
waning in favor of rbenv. Why is rbenv more popular? Because it does less.
Programmers generally like software that does one thing and does it well. In
terms of managing multiple versions of Ruby, rbenv just does less. More
specifically, from the rbenv wiki:

#### rbenv _does…_

* Provide support for specifying __application-specific Ruby versions__.
* Let you __change the global Ruby version__ on a per-user basis.
* Allow you to __override the Ruby version__ with an environment
 variable.

#### In contrast with RVM, rbenv _does not…_

* __Need to be loaded into your shell.__ Instead, rbenv's shim approach works by adding a directory to your `$PATH`.
* __Override shell commands like `cd` or require prompt hacks.__ That's dangerous and error-prone.
* __Have a configuration file.__ There's nothing to configure except which version of Ruby you want to use.
* __Install Ruby.__ You can build and install Ruby yourself, or use [ruby-build](https://github.com/sstephenson/ruby-build) to automate the process.
* __Manage gemsets.__ [Bundler](http://gembundler.com/) is a better way to manage application dependencies. If you have projects that are not yet using Bundler you can install the [rbenv-gemset](https://github.com/jf/rbenv-gemset) plugin.
* __Require changes to Ruby libraries for compatibility.__ The simplicity of rbenv means as long as it's in your `$PATH`, [nothing](https://rvm.io/integration/bundler/) [else](https://rvm.io/integration/capistrano/) needs to know about it.

## Installing rbenv

If you’re on a Mac, the recommended way to install rbenv is via
[Homebrew](http://brew.sh/). If you don’t have Homebrew yet, I highly recommend
it.

To install rbenv via Homebrew, just run:

```
brew install rbenv ruby-build
```

Now that you have rbenv installed, you need to add a line to your profile so
everything loads up correctly when you start your terminal. The location of your
profile varies depending on how your system is set up, but it will typically be
either `~/.profile`, `~/.bash_profile`, or `~/.bashrc`. If you’re using Zsh, it
will probably be `~/.zshrc`. Add the following line to your profile:

```
eval “$(rbenv init -)”
```

To test that it works, run:

```
type rbenv
```
You should get something like the following back if it worked:

```
rbenv is a shell function
```

## Installing a new ruby with rbenv

Awesome, you should now have rbenv installed and you’re ready to roll. Well,
almost ready. rbenv by itself doesn’t do much good. Let’s add a ruby version. As
of this writing, the latest version of Ruby is 2.1.2. _Pro tip: you can view all
versions available to rbenv by running the follow from your terminal:_

```
rbenv install -l
```

To install Ruby 2.1.2, run the following:

```
rbenv install 2.1.2
```

And, if you want to make that version your default global version, you can run:

```
rbenv global 2.1.2
```

## Wrapping up

That’s it! I hope this helps you get set up with rbenv. There are a few other
things that you’ll be able to do, such as installing alternate versions of Ruby
(like Rubinius). If you want to learn more about rbenv, check out the
[README on github](https://github.com/sstephenson/rbenv/blob/master/README.md).
