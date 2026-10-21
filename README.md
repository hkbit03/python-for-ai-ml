# Python

# Strings in Python

- A Python `str` is an immutable sequence of **Unicode code points**. Each word matters:
  - **Sequence**: ordered, so you can index, slice, and iterate
  - **Immutable**: once created, it can never be changed. Every "modification" creates a new string.
  - **Unicode code points**: Python3 strings hold characters from any language, not bytes. `"é"`,`"日本"`, and `"😀"` are all valid.
```python
s = "hello"
print(type(s)) # <class 'str'>
print(len(s)) # 5
```

## **Creating Strings**
```python
a = 'single quotes'
b = "double quotes"
c = "It's easy" # double quotes let you include '
d = 'He said "hi"' # single quotes let you include "
e = """triple quotes
can span multiple lines""" # also '''...'''
f = "adjacent " "literals " "join automatically"
empty = ""
```
Single and double quotes are identical in behavior. Pick one style and stick with it.

## **Escape Sequences**
| Escape       | Meaning                                 |
| ------------ | --------------------------------------- |
| `\n`         | newline                                 |
| `\t`         | tab                                     |
| `\\`         | a literal backslash                     |
| `\'` `\"`   | quote characters                        |
| `\xhh`       | character with hex value `"\x41"` → `A` |
| `\uXXXX`     | Unicode, 16-bit `"\u00e9"` → `é`        |
| `\UXXXXXXXX` | Unicode, 32-bit                         |
| `\N{name}`   | by Unicode name `"\N{SNOWMAN}"` → ☃     |

## **Raw Strings**

A raw string (`r"...."`) treats backslashes literally. It's essential for regex and Windows File Paths.
```python
print(r"C:\Users\Hari Krishnan\Desktop\Python")
# Output: C:\Users\Hari Krishnan\Desktop\Python
```
One catch: *a raw string can't end in an odd number of backslashes (`r"abc\` is a syntax error)* 

## **Converting to a string**

```python
str(42) # '42'
str(3.14) # '3.14'
str([1,2]) # '[1,2]'
str(None) # 'None'
```

`str()` vs `repr()`
- `str(x)` is for humans (readable).
- `repr(x)` is for developers (unambiguous, ideally valid Python).

```python
s = "hi\n"
print(str(s)) # hi  (followed by an actual newline)
print(repr(s)) # 'hi\n'
```

## **Indexing**

Each character has a position starting from 0. negative indices count from the end.
```text
 H  e  l  l  o
 0  1  2  3  4
-5 -4 -3 -2 -1
```
```python
s = "Hello"
print(s[0]) # 'H'
print(s[4]) # 'o'
print(s[-1]) # 'o'
print(s[-5]) # 'H'
print(s[5]) # IndexError: string index out of range
```
Indexing returns a **string of length 1**. Python has no separate "char" type.

## **Slicing**

Syntax: `s[start:stop:step]`
- `start` is inclusive, `stop` is exclusive.
- Any part can be omitted.
- Default `step` is 1

```python
s = "Python"
print(s[0:3]) # 'Pyt'
print(s[:3]) # 'Pyt'
print(s[3:]) # 'hon'
print(s[:]) # 'Python'
print(s[-3:]) # 'hon'
print(s[:-1]) # 'Pytho'
print(s[::2]) # 'Pto'
print(s[::-1]) # 'nohtyP'
print(s[4:1,-1]) # 'oht'
```
**Key Insight**: slicing never raises an error for out-of-range values. It clamps them
```python
"abc"[1:100]   # 'bc'
"abc"[10:20]   # ''
```
**Mental model**: think of indices as sitting *between characters**. Then `s[a:b]` is everything between marker `a` and marker `b`, and `s[:n] + s[n:] == s` is always true.

## **Immutability**

```python
s = "hello"
s[0] = "J" # TypeError: 'str' object does not support item assignment

s = "J" + s[1:] # creates a NEW string 'Jello' and rebinds the name s
```

## **Consequences**:
- Methods like `.upper()` return a new string. They dont modify the original
- A common beginner bug is calling `s.upper()` and ignoring the result.

```python
name = "ana"
name.upper() # result thrown away
print(name) # 'ana'
name = name.upper() # correct
```
Immutability also makes strings **hashable**, so they can be dictionary keys or set members.

## **Operators**

```python
"ab" + "cd"       # 'abcd'  (concatenation)
"ab" * 3          # 'ababab' (repetition)
"ell" in "hello"  # True    (substring test)
"x" not in "abc"  # True
"abc" + 5         # TypeError. You must convert: "abc" + str(5)
```

## **Comparison**

Strings compare lexicographically by code point, character by character
```python
"apple" < "banana"   # True
"a" < "B"            # False! 'a' is 97 and 'B' is 66
"Z" < "a"            # True  (uppercase sorts before lowercase)
ord("a"), chr(97)    # (97, 'a')
```
For case-insensitive comparison, use `.casefold()`:
```python
"Straße".casefold() == "STRASSE".casefold()  # True
```

`==` vs `is`
`==` compares values. `is` compares identity (same object in memory). Use `==` for strings.
`is` may appear to work due to interning, but that's an implementation detail you must never rely on.

## **Iterating**

```python
for ch in "abc":
    print(ch)

for i, ch in enumerate("abc"):
    print(i,ch)
# 0 a
# 1 b
# 2 c

list("abc") # ['a','b','c']
```

## **String Methods**
All of these **return new values** and leave the original unchanged.

### Case
```python
"hello world".upper() # 'HELLO WORLD'
"HELLO".lower() # 'hello'
"hello world".title() # 'Hello World'
"hello world".capitalize()   # 'Hello world'  (first char only)
"Hello".swapcase()           # 'hELLO'
"ß".casefold()               # 'ss'  (aggressive lower, for comparisons)
```
### Searching

```python
s = "banana"
s.find("an")        # 1   (first index, or -1 if not found)
s.rfind("an")       # 3   (search from the right)
s.index("an")       # 1   (like find, but raises ValueError if missing)
s.count("an")       # 2   (non-overlapping occurrences)
s.startswith("ban") # True
s.endswith(("a", "z"))  # True (accepts a tuple of options)
```
Use `in` when you only need a yes/no answer. Use `find` when you need the position

### Testing content
```python
"abc".isalpha()      # True   (letters only)
"123".isdigit()      # True
"123".isdecimal()    # True   (strictest: decimal digits 0-9 and equivalents)
"½".isnumeric()      # True   (broadest: includes fractions, etc.)
"abc123".isalnum()   # True
"   ".isspace()      # True
"ABC".isupper()      # True
"abc".islower()      # True
"my_var".isidentifier()  # True
```
All of them return `False` on an empty string. also note that `"-5".isdigit()` is `False` and so is `"3.14".isdigit()`. For numbers, use `try: int(s)`

### Cleaning

```python
"  hi  ".strip()       # 'hi'
"  hi  ".lstrip()      # 'hi  '
"  hi  ".rstrip()      # '  hi'
"xxhixx".strip("x")    # 'hi'
```
**Gotcha**: `strip("abc")` removes any of those characters from the ends. It does not remove the substring `"abc"`
```python
"cabbage".strip("abc")         # 'ge'
"test.py".removesuffix(".py")  # 'test'  (3.9+, removes an exact substring)
"v1.2".removeprefix("v")       # '1.2'
```

### Replacing and translating
```python
"a-b-c".replace("-", "+")       # 'a+b+c'
"a-b-c".replace("-", "+", 1)    # 'a+b-c' (limit the count)


table = str.maketrans("abc", "xyz")
print(table) # {97: 120, 98: 121, 99: 122} ascii value table
print("aabbcc".translate(table))       # 'xxyyzz'
print(str.maketrans("", "", "aeiou"))  
# builds a table that deletes vowels 
# {97: None, 101: None, 105: None, 111: None, 117: None}
```

### Splitting and joining

```python
"a,b,c".split(",")         # ['a', 'b', 'c']
"a,b,c".split(",", 1)      # ['a', 'b,c']  (maxsplit)
"a  b   c".split()         # ['a', 'b', 'c'] (splits on any whitespace, drops empties)
"a  b".split(" ")          # ['a', '', 'b']  (different behavior!)
"l1\nl2\r\nl3".splitlines()  # ['l1', 'l2', 'l3']
"k=v=w".partition("=")     # ('k', '=', 'v=w')  (always a 3-tuple)
"k=v=w".rpartition("=")    # ('k=v', '=', 'w')

"-".join(["a", "b", "c"])  # 'a-b-c'
"".join(["a", "b"])        # 'ab'
```

`join` is called on the separator, and the items must all be strings: `", ".join(map(str, [1,2,3]))`

### Padding and alignment
```python
"42".zfill(5)        # '00042'
"hi".center(10, "*") # '****hi****'
"hi".ljust(6, ".")   # 'hi....'
"hi".rjust(6)        # '    hi'
```

## String Formatting

### f-strings (use these by default, Python 3.6+)
```python
name, age = "Ana", 30
print(f"{name} is {age}")            # 'Ana is 30'
print(f"{age + 1}")                  # expressions work: '31'
print(f"{name.upper()}")             # method calls work
print(f"{{literal braces}}")         # '{literal braces}'
print(f"{name!r}")                   # uses repr: "'Ana'"
print(f"{age=}")                     # debug form: 'age=30' (3.8+)
```

Format specific mini-language: `{value:spec}`
General form: `[[fill]align][sign][#][0][width][,][.precision][type]`

```python
pi = 3.14159265
print(f"{pi:.2f}")        # '3.14'      (2 decimals)
print(f"{pi:10.3f}")      # '     3.142' (width 10)
print(f"{'hi':>8}")       # '      hi'   (right align; < left; ^ center)
print(f"{'hi':*^8}")      # '***hi***'   (fill char + center)
print(f"{1234567:,}")     # '1,234,567'
print(f"{0.256:.1%}")     # '25.6%'
print(f"{255:b}")         # '11111111'   (binary; x=hex, o=octal)
print(f"{255:#x}")        # '0xff'
print(f"{5:03d}")         # '005'
print(f"{1e6:e}")         # '1.000000e+06'
w, p = 10, 2
print(f"{pi:{w}.{p}f}")   # nested fields: '      3.14'
```
Older styles (you'll see them in existing code)

```python
"{} is {}".format("Ana", 30)
"{0} {1} {0}".format("a", "b")        # 'a b a'
"{name}".format(name="Ana")
"%s is %d" % ("Ana", 30)              # printf-style
"%.2f" % 3.14159
```

## Unicode, encoding, and bytes

This is the topic that causes the most "why is my text broken" bugs.
- `str` = text (code points)
- `bytes` = raw binary data
- **Encoding** converts `str -> bytes`. **Decoding** converts `bytes -> str`

```python
s = "café"
b = s.encode("utf-8")      # b'caf\xc3\xa9'
len(s), len(b)             # (4, 5). 'é' takes 2 bytes in UTF-8
b.decode("utf-8")          # 'café'
b.decode("ascii")          # UnicodeDecodeError
s.encode("ascii", errors="ignore")   # b'caf'
s.encode("ascii", errors="replace")  # b'caf?'
```

**Rules of thumb**
1. Decode bytes to `str` as soon as data enters your program, work in `str`, and encode only when leaving (files, network)
2. Always specify the encoding: `open("f.txt", encoding="utf-8")`
3. Default to UTF-8

### Normalization

The same visible character can be built different ways

```python
import unicodedata
a = "é"            # one code point (U+00E9)
b = "e\u0301"      # 'e' + combining accent
a == b             # False!
len(a), len(b)     # (1, 2)
unicodedata.normalize("NFC", b) == a   # True
```
normalize before comparing user text. also remember that `len()` counts code points, not visible "characters" (e.g., some emoji are multiple code points)

## Performance and best practices

concatenating in a loop is slow in principle, since each `+=` may copy the whole string:
```python
# Avoid
result = ""
for w in words:
    result += w + " "

# Prefer
result = " ".join(words)
```

For building text piece by piece, collect parts in a list and `join` at the end (or use `io.StringIO`).
Other habits: 
- Use `in` for membership, not `find() != -1`
- Use `startswith`/`endswith` instead of slicing comparisons.
- Prefer f-strings for readability
- Use `str.split()` with no argument to split on whitespace.
- For complex pattern matching, use the `re` module rather that chaining string methods.

