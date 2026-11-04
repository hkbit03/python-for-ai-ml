# What is Python?

Python is a **high-level , interpreted, dynamically typed, general-purpose language**.


|  Term             | What it means | 
| -------------     | ---------- | 
|  High Level       | You dont manage memory, or hardware details.    |
|  Interpreted       | Code is compiled to bytecode and run by a virtual machine (CPython), with no separate compile step for you. Executes line by line so that Debugging is easier in this programming language which also makes this slower than other languages.    |
|  Dynamically typed       | Types are checked at runtime, and a variable can hold any type    |
|  Strongly Typed       | Python won't silently mix incompatible types (`"1" + 1` is an error)    |
|  Multi Paradigm       | Procedural, object-oriented, and functional styles all work    |
|  Garbage collected       | Memory is freed automatically (reference counting plus a cycle collector)    |

**CPython** is the standard implementation, the one you download from python.org. Others exist (PyPy, Jython), but "Python" almost always means CPython.

## Running Python

```bash
python --version        # check the version (or python3 on some systems)
python                  # opens the REPL (interactive prompt)
python script.py        # runs a file 
python -c "print('hi')" # runs a one-liner
python -m venv .venv    # creates a virtual environment
pip install requests    # installs a package.
```

In the REPL, `_` holds the last result, `help(obj)` shows docs, and `dir(obj)` lists attributes. 

***Always use a virtual environment per project so dependencies don't collide***

**The Zen of Python**
```python
import this
```
Core ideas: readability counts, explicit is better than implicit, simple is better than complex.


## Basic Syntax

### Statements and indentation

Python uses **indentation** instead of braces.
```python
if True:
    print("indented block") # 4 spaces by convention
print("outside")
```

- Be consistent. Never mix tabs and spaces.
- A block after `:` (`if`, `for`, `def`, `class`, `while`, `try`, `with`) must be indented and non-empty (use `pass` as a placeholder).
- Wrong indentation raises `IndentationError`.

### Line Continuation

```python
total = 1 + 2 + \       # backslash works but is discouraged
        3

total = (1 + 2 + 
    3)              # prefered: inside (), [], {} lines continue freely

items = [
    "a",
    "b",
]
# trailing commas are fine, and good style in multiline literals
```

### Multiple statements per line

```python
x = 1; y=2 # legal but discouraged
```

### Comments and docstrings

```python
# This is a Comment

x = 5 # Inline Comment

def area(r):
    """Return the area of a circle. <-docstring: documentation, not a comment"""

    return 3.14159 * r ** 2

print(area.__doc__)
```

Python as no multi-line comment syntax. Use several `#` lines. A bare triple-quoted string works as a block comment but is really just an unused string, so avoid it for that purpose.

### Case sensitivity

`name`, `Name`, and `NAME` are three diffeent identifiers. Keywords like `True`, `False`, `None` are capitalized, while `if`, `for`, `while` are lowercase.

### Keywords

Reserved words you can't use as names

*Keywords in Python are reserved words that have predefined meanings and specific purposes in the Python Programming Language. They define the syntax and structure of Python programs and cannot be used as identifiers (such as variable names, function names or class names).*

```python
import keyword
print(keyword.kwlist)
# False None True and assert async await break class continue def details
# elif else except finally for from global if import in is lambda nonlocal
# not or pass raise return try while with yield
```

Dont shadow built-ins either (`list`, `str`, `sum`, `id`, `type`, `max`, `input`). It's legal but breaks things.

```python
list = [1,2]
list('abc') # TypeError: 'list' object is not callable
```

### Output and Input

```python
print("a", "b", "c")                # a b c
print("a", "b", sep="-")            # a-b
print("no newline", end="")         # suppresses the newline
print("x", file=sys.stderr)         # to another stream (needs import sys)

name = input("Name: ")              # ALWAYS returns a str
age = int(input("Age: "))           # convert explicitly
```

### The `__main__` guard

```python
def main():
    print("running")

if __name__ == "__main__":   # True only when run directly, not when imported
    main()
```

### Style (PEP 8)

|  Thing              | Convention | 
| -------------     | ---------- | 
|  Variables, functions       | `snake_case`    |
|  Classes       | `PascalCase`    |
|  Constants       | `UPPER_SNAKE_CASE`    |
|  Indentation       | 4 spaces    |
|  Line Length      | -79 to 88 characters   |
|  Operators      | Spaces around them: `x = a + b`   |
|  Imports      | At the top, one per line   |


## Variables

### What a variable really is?

A **variable** in Python is a **named reference to an object stored in memory**. It acts as an identifier that allows a program to access, manipulate, and reuse data during execution.

```python
x = 10        # create int object 10, bind the name x to it
y = x         # y now refers to the SAME object
x = 20        # x is rebound to a new object; y still refers to 10
print(y)      # 10
```
```python
id(x)         # the object's identity (its memory address in CPython)
```

#### Variable Naming Conventions
- Letters, digits, underscores. Cannot start with a digit.
- Case-sensitive. No spaces. Not a keyword.

```python
user_name = "ana"    # good
_private = 1         # leading underscore: "internal" by convention
__dunder__ = 2       # reserved for Python's special names
2fast = 1            # SyntaxError
my-var = 1           # SyntaxError (it's parsed as subtraction)
```
Pick descriptie names: `total_price`, not `tp`

#### Dynamic Typeing

```python
x = 5            # int
x = "five"       # now str. Legal, since the NAME has no type, the OBJECT does
type(x)          # <class 'str'>
```

Optional type hints document intent, but Python does not enforce them at runtime (tools like `mypy` check them)

```python
age: int = 30
name: str = "Ana"
def greet(n: str) -> str:
    return f"Hi {n}"
```

#### Assignment forms

```python
a = b = c = 0              # chained: all three name the same object
x, y = 1, 2                # tuple unpacking
x, y = y, x                # swap, with no temp variable needed
first, *rest = [1, 2, 3, 4]       # first=1, rest=[2, 3, 4]
*init, last = [1, 2, 3]           # init=[1, 2], last=3
a, (b, c) = 1, (2, 3)             # nested unpacking
_, second, _ = (1, 2, 3)          # _ by convention means "ignore"

count = 0
count += 1                 # augmented: -=, *=, /=, //=, %=, **=, &=, |=, ^=, <<=, >>=

```

Chained assignment with mutable objects shares one object, which is a classic trap.

```python
a = b = []
a.append(1)
print(b)         # [1]  (same list!)
```

#### Constants

Python has none enforced. By convention:
```python
MAX_RETRIES = 3 # UPPER_SNAKE_CASE signals "dont change this"
```

#### Deleting a name

```python
del x # removes the NAME; the object is freed when nothing refers to it.
```

#### Scope (breif preview)

Python resolves names using **LEGB: Local -> Enclosing -> Global -> Builtin**.

```python
x = "global"
def f():
    x = "local"      # creates a NEW local; does not modify the global
    print(x)
f(); print(x)        # local / global

def g():
    global x         # explicitly use the global
    x = "changed"
```

Assigning to a name anywhere inside a function makes it local for the whole function. Reading it before assigning raises `UnboundLocalError`.

## Data Types Overview

```python
type(42)                 # <class 'int'>
isinstance(42, int)      # True  (preferred over type(x) == int)
isinstance(42, (int, float))   # True: accepts a tuple of types
```

| Category  | Types                             | Mutable? |
|-----------|-----------------------------------|----------|
| Numeric   | `int`, `float`, `complex`, `bool` | No       |
| Text      | `str`                             | No       |
| Binary    | `bytes`                           | No       |
| Binary    | `bytearray`                       | **Yes**  |
| Sequence  | `list`                            | **Yes**  |
| Sequence  | `tuple`, `range`                  | No       |
| Mapping   | `dict`                            | **Yes**  |
| Set       | `set`                             | **Yes**  |
| Set       | `frozenset`                       | No       |
| Singleton | `NoneType` (`None`)               | N/A      |

Everything is an object, including ints, functions, and classes.

## Numbers
`int`

Arbitrary precision. There is no overflow, only available memory
```python
2 ** 100                  # 1267650600228229401496703205376
1_000_000                 # underscores for readability (3.6+)
0b1010                    # binary  -> 10
0o17                      # octal   -> 15
0xFF                      # hex     -> 255
int("ff", 16)             # 255  (parse in a given base)
bin(10), oct(15), hex(255)    # '0b1010', '0o17', '0xff'
(255).bit_length()        # 8
```

`float`

A 64-bit IEEE 754 double, which means binary approximation.
```python
3.14
1e3          # 1000.0
2.5e-3       # 0.0025
0.1 + 0.2    # 0.30000000000000004  (not a Python bug; it's how binary floats work)
float("inf"), float("-inf"), float("nan")
```
```python
import math
math.isclose(0.1 + 0.2, 0.3)     # True. Never use == on floats
nan = float("nan")
nan == nan                        # False! Use math.isnan(nan)
```


`complex`
```python
z = 3 + 4j
z.real, z.imag, abs(z)       # 3.0, 4.0, 5.0
```

`bool`

`bool` is a subclass of `int`: `True == 1`, `False == 0`.
```python
True + True          # 2
sum([True, False, True])   # 2  (handy for counting)
isinstance(True, int)      # True
```

## Arithmetic Operators

| Op          | Meaning                          | Example  | Result |
|-------------|----------------------------------|----------|--------|
| `+` `-` `*` | Basic arithmetic operations      | `7 + 2`  | `9`    |
| `/`         | True division (**always float**) | `7 / 2`  | `3.5`  |
| `//`        | Floor division                   | `7 // 2` | `3`    |
| `%`         | Modulo                           | `7 % 2`  | `1`    |
| `**`        | Exponentiation (power)           | `2 ** 3` | `8`    |

Floor division rounds toward negative infinity, not toward zero:
```python
-7 // 2      # -4   (not -3)
-7 % 2       # 1    (result takes the sign of the divisor)
7 // -2      # -4
divmod(7, 2) # (3, 1)   quotient and remainder together
4 / 2        # 2.0  (float, even when exact)
```

The identity `a == (a // b) * b + a % b` always holds.

**Useful numeric built-ins**
```python
abs(-5)            # 5
round(3.14159, 2)  # 3.14
round(2.5)         # 2   <- banker's rounding: ties go to the nearest EVEN number
round(3.5)         # 4
pow(2, 10)         # 1024
pow(2, 10, 1000)   # 24  (modular exponent)
min(3, 1, 2); max(...); sum([1, 2, 3])
```
```python
import math
math.floor(-2.5)   # -3
math.ceil(2.1)     # 3
math.trunc(-2.9)   # -2  (toward zero)
math.sqrt(16)      # 4.0
math.pi, math.e
math.gcd(12, 18)   # 6
```

Exact arithmetic when you need it

```python
from decimal import Decimal
Decimal("0.1") + Decimal("0.2") == Decimal("0.3")   # True (use for money)

from fractions import Fraction
Fraction(1, 3) + Fraction(1, 6)     # Fraction(1, 2)
```

Always build `Decimal` from a string: `Decimal(0.1)` captures the float's error.

**Operator precedence (high to low)**

```python
**
+x  -x  ~x               (unary)
*  /  //  %
+  -
<<  >>
&
^
|
==  !=  <  >  <=  >=  is  in  not in
not
and
or
if-else (ternary)
:=
```

`-2 ** 2` is `-4` because `**` binds tighter than unary minus. When unsure, add parentheses.

## Strings

Covered in depth in the next page. The essentials:
```python
s = "Python"
s[0], s[-1], s[1:4], s[::-1]
len(s); s.upper(); f"Hello {s}"
```
Strings are immutable sequences of Unicode code points.

## `None`

The single "no value" object, of type `NoneType`.

```python
x = None
x is None          # True  (use `is`, not ==)
bool(None)         # False

def f():
    pass
print(f())         # None: functions without a return give None
```
Use it for "not set yet" or "no result"


## Collections

Each gets its own topic later. Here's the identity card for each:
```python
# list: ordered, mutable, allows duplicates, mixed types OK
nums = [1, 2, 3]
nums.append(4); nums[0] = 99

# tuple: ordered, IMMUTABLE
pt = (3, 4)
single = (5,)          # the comma makes the tuple, not the parentheses!
not_tuple = (5)        # just the int 5

# dict: key -> value, insertion-ordered (3.7+), keys must be hashable
user = {"name": "Ana", "age": 30}
user["email"] = "a@x.com"
user.get("phone", "n/a")

# set: unordered, unique, hashable items only, fast membership
tags = {"a", "b", "a"}     # {'a', 'b'}
empty_set = set()          # {} is an empty DICT, not a set

# frozenset: immutable set
# range: lazy immutable sequence of ints
range(5)               # 0,1,2,3,4
range(2, 10, 2)        # 2,4,6,8
```

### Choosing a Type


| Need                          | Use     |
|-------------------------------|---------|
| Ordered, changeable            | `list`  |
| Fixed record, or a dictionary key | `tuple` |
| Lookup by key                  | `dict`  |
| Uniqueness / membership tests  | `set`   |
| Counting loop                  | `range` |


## Type Conversion (casting)

Explicit

```python
int("42")        # 42
int("  42  ")    # 42  (whitespace OK)
int(3.9)         # 3   (TRUNCATES toward zero, doesn't round)
int(-3.9)        # -3
int("3.5")       # ValueError! Go through float: int(float("3.5"))
int("abc")       # ValueError
float("3.5")     # 3.5
str(42)          # '42'
bool("")         # False
bool("0")        # True  (non-empty string)
list("abc")      # ['a', 'b', 'c']
tuple([1, 2])    # (1, 2)
set([1, 1, 2])   # {1, 2}
dict([("a", 1), ("b", 2)])   # {'a': 1, 'b': 2}
chr(65), ord("A")            # 'A', 65
```

Safe parsing

```python
try:
    n = int(text)
except ValueError:
    n = None
```

### Implicit (automatic) conversion
Only for numeric widening: `bool -> int -> float -> copmplex`
```python
1 + 2.0      # 3.0
True + 1.5   # 2.5
"1" + 1      # TypeError. Python never guesses between str and number
```

## Mutability, Identity, and Copying

### Immutable vs Mutable behavior
```python
a = "hi"
b = a
a += "!"          # creates a NEW string; b is unaffected
print(b)          # 'hi'

x = [1, 2]
y = x
x.append(3)       # mutates the SHARED object
print(y)          # [1, 2, 3]
```

### Copying

```python
import copy
orig = [[1, 2], [3, 4]]

alias = orig                  # same object
shallow = orig.copy()         # also: list(orig), orig[:]   (new outer list, SAME inner lists)
deep = copy.deepcopy(orig)    # fully independent

orig[0].append(99)
shallow[0]   # [1, 2, 99]  <- inner list is shared!
deep[0]      # [1, 2]
```

### Mutable default argument trap

```python
def add(item, bucket=[]):      # the default list is created ONCE
    bucket.append(item)
    return bucket

add(1); add(2)    # [1, 2] on the second call, not [2]

# Fix
def add(item, bucket=None):
    if bucket is None:
        bucket = []
    bucket.append(item)
    return bucket
```

### Hashability

Only immutable (hashable) objects can be dict keys or set members:

```python
{(1, 2): "ok"}      # tuple works
{[1, 2]: "no"}      # TypeError: unhashable type: 'list'
```

### Small-int caching
```python
a = 256; b = 256
a is b       # True (cached; CPython detail)
a = 1000; b = 1000
a is b       # may be False. NEVER use `is` to compare values
```

## Common pitfalls checklist

| Mistake                                       | Fix                                         |
|-----------------------------------------------|---------------------------------------------|
| Mixing tabs and spaces                        | Use spaces only, 4 per indentation level    |
| `input()` result used as a number             | `int(input())`                              |
| `"5" + 5`                                     | `int("5") + 5` or an f-string               |
| `int("3.7")` crashes                          | `int(float("3.7"))`                         |
| Expecting `int(-3.9)` to be -4                | It truncates to -3. Use `math.floor` for -4 |
| `/` expected to give an int                   | Use `//`                                    |
| `0.1 + 0.2 == 0.3`                            | Use `math.isclose` or `Decimal`             |
| `round(2.5)` expected 3                       | Banker's rounding gives 2                   |
| `a = b = []` shared                           | Create separate lists                       |
| `{}` expected to be an empty set              | Use `set()`                                 |
| `(5)` expected to be a tuple                  | Use `(5,)`                                  |
| Naming a variable `list`, `str`, `sum`, `max` | Use a different name                        |
| `x == None`                                   | Use `x is None`                             |
| `is` for comparing numbers/strings            | Use `==`                                    |
| Mutable default argument                      | Use `None` as the default                   |
| Assigning to a global inside a function       | Use `global x`, or better, return values    |
| `copy` expected from `b = a`                  | Use `.copy()` or `deepcopy`                 |
| `-2 ** 2` expected to be 4                    | It evaluates to -4. Use `(-2) ** 2`         |



---

[(Prev) Table of Contents](README.md) - [(Next) Conditionals](Conditionals.md)

[Table of Contents](README.md)
