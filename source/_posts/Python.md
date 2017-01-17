---
title: Python
copyright: true
date: 2016-10-04 00:19:16
tags: Python
---

## 接收输入
```python
foo = input('Enter a number:')
```

curly braces 大括号

## 特点
- 没有 `++`
- `True` 和 `False`
- `9 >= 9.0` => `True`
- `8.7 <= 8.70` => `True`
- `elif`
- reverse a list: [::-1]
- list comprehensions: [i**3 for i in range(5)]
- "Numbers:{0} {2} {1}".format(1, 2, 3)
- "{x},{y}".format(x=5, y=12)
- `b = 1 if a >= 5 else 43`

```py
if all([i > 5 for i in nums]):
  print("All larger than 5")
if any([i % 2 == 0 for i in nums]):
  print("At least one is even")
for v in enumerate(nums):
  print(v)
```

## Decorators
```py
def decor(func):
    def wrap():
        print("=================")
        func()
        print("=================")
    return wrap

@decor
def print_text():
    print("Hello world!")


print_text()
```

## Recursion
```py
# RuntimeError occurred in my brain!
def is_even(x):
    if x == 0:
        return True
    else:
        return is_odd(x - 1)

def is_odd(x):
    return not is_even(x)

print(is_odd(17))
print(is_even(23))

```


```py
def count_char(text, char):
    count = 0
    for c in text:
        if c == char:
            count += 1
    return count

filename = input("Enter a filename:")
with open(filename) as f:
    text = f.read()

text.lower()

for char in "abcdefghijklmnopqrstuvwxyz":
    perc = 100 * count_char(text, char) / len(text)
    print("{c} - {p}%".format(c = char, p = round(perc, 2)))
```

## 可变参数
```py
def my_func(x, y=7, *args, **kwargs):
  print(y)
  print(args)
  print(kwargs)

my_func(2, 3, 4, 5, 6, a=7, b=8)
```

## Tuple Unpacking
```py
numbers = (1, 2, 3, 4, 6, 7, 8,)
a, b, *c, d = numbers

# 交换
# since b, a on the right hand side forms the tuple (b, a) which is then unpacked
a, b = b, a
```


## 正则表达式
> Other metacharacters such as `$` and `.`, have no meaning within character classes.
> The metacharacter `^` has no meaning unless it is the first character in a class.
PS: no meaning 的时候可以当做普通字符使。

- `\d`: digits
- `\s`: whitespace
- `\w`: word characters `[a-zA-Z0-9_]`
- Versions of these special sequences with upper case letters - `\D`, `\S`, `\W` - mean the opposite to the lower-case versions.
- `\A` and `\Z` match the beginning and end of a string.
- `\b` matches the empty string between `\w` and `\W` characters, or `\w` characters and the beginning or end of the string.

```py
import re

patten = r"a(bc)(de)(f(g)h)i"

match = re.match(patten, "abcdefghijklmnop")

if match:
  print(match.group())
  print(match.group(0))
  print(match.group(1))
  print(match.group(2))
  print(match.groups())

# 命名组和忽略组
pattern = r"(?P<first>abc)(?:def)(ghi)"

match = re.match(pattern, "abcdefghi")
if match:
  print(match.group("first"))
  print(match.groups())
  print(match.group())
  print(match.group(0))
  print(match.group(2))

pattern = r"gr(a|e)y"

# special sequences
pattern = r"(.+) \1"

match = re.match(pattern, "word word")
if match:
  print("Match 1")

match = re.match(pattern, "?! ?!")
if match:
  print("Match 2")

match = re.match(pattern, "abc cde")
if match:
  print("Match 3")


# matches the word "cat" surrounded by word boundaries.
pattern = r"\b(cat)\b"

# 建议匹配邮箱
# A much more complex regex is required to fully validate an email address.
pattern = r"([\w\.\-]+)@([\w\.\-]+)(\.[\w\.]+)"
```

## 常识
> Programmers ofter place assertions at the start of a function to check for valid input,
> and after a function call to check for valid output.

## IDLE 快捷键
- `Ctrl + N`
- `Ctrl + S`
- `F5`

## 待整理

- Python 经常使用 `spam` 和 `eggs` 作为占位变量名，而其他语言通常使用 `foo` 和 `bar`
- extra zeros at the number's end are ignored.

- 术语 `foobar` 是一个常见的无名氏化名，常被作为“伪变量（metasyntactic variable）”使用
- `foo` 常被作为函数/方法的名称，而 `bar` 则常被用作变量名

## PEP 8 标准
- modules should have short, all-lowercase names; 
- class names should be in the CapWords style; 
- most variables and function names should be lowercase_with_underscores; 
- constants (variables that never change value) should be CAPS_WITH_UNDERSCORES;
- names that would clash with Python keywords (such as 'class' or 'if') should have a trailing underscore.

额外内容
- lines shouldn't be longer than 80 characters; 
- 'from module import *' should be avoided; 
- there should only be one statement per line.

PEP 20: Zen of Python
PEP 257: Style Conventions for Docstrings


PEP 8 also recommends using spaces around operators and after commas to increase readability.
However, whitespace should not be overused. For instance, avoid having any space directly inside any type of brackets.

## Packaging
```
SoloLearn/
  LICENSE.txt
  README.txt
  setup.py
  sololearn/
    __init__.py
    sololearn.py
    sololearn2.py
```

```py
# setup.py

from distutils.core import setup

setup(
    name='SoloLearn',
    version='0.1dev',
    packages=['sololearn',],
    license='MIT',

long_description=open('README.txt').read(),
)
```

### build a source distribution
```bash
# navigate to the directory containing setup.py
python setup.py sdist
```

### build a binary distribution
```bash
python setup.py bdist
```

### upload a package
```bash
python setup.py register
python setup.py sdist upload
```

### install a package
```bash
python setup.py install
```