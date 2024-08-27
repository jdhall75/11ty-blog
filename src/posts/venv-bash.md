---
title: Managing virtual environments with Bash 
date: 2024-08-23
tags:
  - bash
  - python
  - venv
  - pip
description: When you dont want to install another tool to manage your Python virtual environments, what do you do? Create some bash functions that help with that.
permalink: posts/{{ title | slug }}/index.html
---

I guess there are two methodologies out there to managing your virtual environments for Python.  The first is to add it in the .venv directory in the root of your project.  That way when you manuver into your project you can simply execute `. .venv/bin/activate` and get started.  The other is to manage them in a central location. In the base of your $HOME directory somewhere like .virtualenvs.  These are great, but they both have their pro's and con's.  Did I create a virtual environment for this project? If its buried in a directory outside your project it's hard to tell.  If you placed your .venv in the root of your project and forgot to add it to the `.gitignore`(Your using version control, right?).  You could end up with more in your repo that you wanted.  

You could employ a manager like Pyenv, Hatch, Pipenv, or Pyflow to take care of the heavy lifting, but that's another tool to learn and I a trying to simplify my worflow.  What if we had management of the environments built right in our shell.  That is when I ended up writing my own set of bash functions to manage my virtual environments. I am no bash guru, but they do work pretty nicely if I do say so myself.

I've had some thoughts on this since I started writing.  Like using the projects directory name for the virtualenv name.  This could save some typing.  Maybe setting up PROMPT_COMMAND to point to a function that checks the `$PYENV_HOME` for an existing virtual environment and activates it automatically.



```bash
# Init python venvs created in the .local/venvs dir
# activate <venv_name>
function chkarg() { if [ -z "$@" ]; then echo "${FUNCNAME[-1]} requires an venv name"; exit -1; else exit 0; fi }
function activate() {
    (chkarg $@)
    if [ -n "$VIRTUAL_ENV" ]; then
        deactivate
    fi
    source $PYENV_HOME/$@/bin/activate ;
}
function mkenv { (chkarg $@) && $(which python3) -m venv $PYENV_HOME/$@ ;}
function rmenv {
    (chkarg $@)
    if [ "$?" -ne 0 ]; then
        return -1
    fi
    ACTIVE=""
    if [ "$( basename $VIRTUAL_ENV)" == "$@" ]; then
        deactivate
    fi
    echo "removing $@"
    $(which rm) -fr $PYENV_HOME/$@ ;
}

#function rmenv { (chkarg $@) && echo "removing $@";}
function lsenv {
    ACTIVE=""
    if [ -n "$VIRTUAL_ENV" ]; then
        ACTIVE=$(basename $VIRTUAL_ENV)
    fi
    for d in $(ls -d $PYENV_HOME/* 2> /dev/null); do
        VENV_NAME=$(basename $d)
        if [ "$VENV_NAME" = "$ACTIVE" ]; then
            echo "* $VENV_NAME";
        else
            echo "  $VENV_NAME"
        fi
    done ;
}
```