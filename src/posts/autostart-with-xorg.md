---
title: Autostarting applications in Xorg
date: 2023-01-01
tags:
- Linux
- awesomewm
- shell
description: Starting applications on login from the display manager
permalink: posts/{{ title | slug }}/index.html

---

I recently had an issue figuring out how to get picom start while booting into
Awesomewm. After doing much research I found that I should be able to handle
the initialization files for Xorg.  All the files that are involved in setting
up your X session are simply shell scripts.

Anyway, here's the solution to getting picom running on login.  Not much here,
but it works.


## .xsession
```sh
#!/bin/sh

# Make sure this is before the 'exec' command or it won't be sourced.
[ -f /etc/xprofile ] && . /etc/xprofile
[ -f ~/.xprofile ] && . ~/.xprofile

exec awesome

```

## .xprofile
```sh
#!/bin/sh


# turn the bell off -- annoying
xset -b b off

# start picom
picom -b
```

