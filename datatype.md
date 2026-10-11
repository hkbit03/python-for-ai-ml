# Data Types
In Python, **data types** classigy values (objects) by what operations they support and how they behave.


**Numeric Types**
- `int`: integers with unlimited precision (e.g., 0,-5,10*100).
- `float`: floating point decimal based numbers (e.g., 3.14, -0.001, 2.0)
- `complex`: complex numbers (e.g., 1+2j)
- `bool`: boolean values `True` and `False` returning 1 and 0 respectively

**Text and "nothing"**
- `str`: text (Unicode strings), e.g., "hello", "data"
- `NoneType`: the type of `None`, representing "no value" (distinct from 0, False, `""`)

**Sequence Types (ordered collections)**
- `list`: mutable ordered collection e.g., `[1,2,3]`
- `tuple`: immutable ordered collection, e.g., `(1,"a",True)`
- `range`: immutable sequence of integers, used mostly in loops, e.g., `range(5)`

**Mapping Types (key value)**
- `dict`: mutable mapping from keys to values, e.g, `{"name":"alice", "age":30}`

**Set Types (unordered, no duplicates)**
- `set`: mutable unordered collection of unique items, e.g., `{1,2,3}`
- `frozenset`: immutable version of set

There are also types like `bytes` and `bytearray`, but the above cover everyday use.

[<prev](/variables.md) | [next>](/type_conv.md)