---
title: Iter protocol for CLLI generation
date: '2025-04-09'
tags: [python, network]
decription: Sometimes you just need a list of names, here are a couple of ways that you can generate them.
---

The faker package has always amazed me with the things that you can generate with it.  There has been occasion where I just needed one thing or a list of things and maybe faker didn't support it.

With a few of Pythons utilities and predefined sets you can do a lot. Being a network engineer I often need a list of hostnames for testing a database or a script that parses said hostnames.  We will take a look at the Iterator Protocol, fancy way to say we are creating a generator..

The iterator protocol is implemented in a class using 2 dunder methods, `__iter__` and `__next__`

