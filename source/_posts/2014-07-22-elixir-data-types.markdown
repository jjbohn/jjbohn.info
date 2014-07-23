---
layout: post
title: "Elixir Data Types"
date: 2014-07-22 20:23:31 -0500
comments: true
categories: [elixir, beginners, functional]
---

I’ve been playing with Elixir lately and wanted to persist a few things that I
thought were interesting about some of it’s core data types. It’s very basic,
but if you’re just getting started with Elixir like I am, you might find these
notes interesting.

Elixir has a few of the basic types you’d expect to see in any other language.
For example, Elixir has integers, floats and strings. Besides those types, there are
atoms (aka. symbols), lists and tuples.

Atoms look just like symbols in Ruby. They are constants whose name is also it’s
value.

```elixir
iex> :foo
:foo
iex> :foo == :bar
false
iex> is_atom(:foo)
true
```

Another interesting note is that booleans are just atoms.

```elixir
iex> is_atom(true)
true
iex> false == :false
true
```

Lists in Elixir are [linked lists](http://en.wikipedia.org/wiki/Linked_list). A
linked list is data structure in which each node contains a reference to the
next node.

![Linked
list](http://www.cs.usfca.edu/~srollins/courses/cs112-f07/web/notes/linkedlists/ll2.gif)

