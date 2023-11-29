---
title: Pythons dict.setdefault method
date: 11-29-2023
tags:
  - Python
  - little-known
description: A way to keep unique keys in a dictionary without overwriting values or extensive checking. 
permalink: posts/{{ title | slug }}/index.html

---
I was this days old when I learned about the setdefault method in the dict data type.  This method keeps you from overwriting values in a dictionary if they have already been set. I can see instant use for this.  You could implement read only keys or have a primary key like in a database.

The syntax for this is very similar to the `update` method, but only updates the dictionary if the key doesnt exist.

The help tells us:
```
dict.setdefault = setdefault(self, key, default=None, /)
    Insert key with a value of default if key is not in the dictionary.
    
    Return the value for key if key is in the dictionary, else default.
```

In practice it looks like.
```python
mydict = {}
mydict.setdefault('first', 1)     # mydict == {'first': 1}
mydict.setdefault('first', '1st') # mydict == {'first': 1}
```

