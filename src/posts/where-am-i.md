---
title: Where Am I
date: 2025-4-16
tags:
- python
- learning
description: Find where you are at in the filesystem
permalink: posts/{{ title | slug }}/index.html
---

## Modules
`os`
`pathlib`

## Finding The Path Of Your Script
### os module
```python
base_dir = os.path.dirname(__file__)
```

### pathlib
```python
base_dir = pathlib.Path(__file__).parent
```

## The Users Director
### os module
```python
# this can change if you change directories in the script
user_dir = os.getcwd()
```

### pathlib module
```python
# this can change if you change directories in the script
user_dir = pathlib.Path.cwd()
```

