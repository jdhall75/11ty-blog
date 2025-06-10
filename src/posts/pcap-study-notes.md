---
title: PCAP Study notes - Exceptions
date: 2025-06-10
tags:
- python
- learning
- PCAP
description: Studying for the PCAP301
permalink: posts/{{ title | slug }}/index.html
---

## Exceptions
All Exceptions are derived from `BaseException`

### Concrete Exceptions
In Python, "concrete exceptions" refer to specific, built-in exception types that are raised by the interpreter or standard library functions in response to various error conditions. These contrast with more general exception types (like Exception itself) or user-defined exceptions.

#### Examples:
- TypeError
    - Class methods or functions will raise a `TypeError` when called without the correct amount of positional arguments.
```python
"hello" + 5 # Raises TypeError; string and int types cannot be added

import math
math.pow(2) # Raises TypeError
```

- ValueError
```python
int("abc") # Raises ValueError for incompatible value being passed to int()
```

- IndexError
```python
my_list = [1,2,3]
my_list[5] # Raises IndexError
```

- KeyError
```python
my_dict = {'a': 1}
my_dict['b'] # Raises KeyError
```

- NameError
```python
print(not_defined_variable) # Raises NameError
```

- ZeroDivisionError
```python
10 / 0 # Raises ZeroDivisionError
```

- FileNotFoundError
```python
with open('non_existent_file.txt', 'r') as fp: # Raises FileNotFoundError
    pass
```

- AttributeError
```python
my_str = "this is some text"
my_str.non_existent_method() # Raises AttributeError
```

- ImportError
```python
import non_existent_module # Raises ImportError
```

