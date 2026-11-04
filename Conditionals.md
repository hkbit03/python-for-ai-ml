# Conditionals

A conditional lets your program **choose** which code to run based on whether something is true or false. Without them, code runs top to bottom with no decisions.

```python
age = 20
if age >= 18:
    print("Adult")
```

## Syntax and Indentation

```python
if condition:
    # runs if condition is truthy
    statement
elif other_condition:
    # runs if the first was falsy and this is truthy
    statement
else:
    # runs if nothing above matched
    statement
```

Rules:
- The colon `:` after the condition is required.
- **Indentation defines the block**: use 4 spaces consistently. Mixing tabs and spaces raises `TabError`/`IndentationError`.
- `elif` and `else` are optional. You can have many `elif`s but only one `else`, and it must be last.
- Only the first matching branch runs. After one matches, Python skips the rest.

```python
score = 85
if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"      # this runs; the rest are skipped
elif score >= 70:
    grade = "C"
else:
    grade = "F"
```

**Order matters**: if you wrote `score >= 70` first, 85 would get "C".

**Empty blocks**

A block cannot be empty. Use `pass` as a placeholder:

```python
if x > 0:
    pass   # TODO
```

## Truthiness: What Counts as True or False

Python doesn't need a literal `True`/`False` in a condition. Every object has a truth value.

Falsy values (everything else is truthy):

| Falsy                       | Example                                     |
| --------------------------- | ------------------------------------------- |
| `False`                     |                                             |
| `None`                      |                                             |
| Zero of any numeric type    | `0`, `0.0`, `0j`                            |
| Empty sequences/collections | `""`, `[]`, `()`, `{}`, `set()`, `range(0)` |

```python
name = ""
if name:
    print("has a name")
else:
    print("empty")      # this runs, since it's an empty string

items = [0]
if items:               # True! The list is non-empty, even though it contains 0
    print("non-empty")
```

Idiomatic Python uses truthiness directly:

```python
# Avoid
if len(items) > 0:
if flag == True:

# Prefer
if items:
if flag:
```

**Watch Out**
- `bool("False")` is `True` (non-empty string).
- `bool("0")` is `True`.
- `0` is falsy, so `if count:` fails when 0 is a valid value. If you mean "was a value provided?", use `if count is not None:`.
- Your own classes can define truthiness via `__bool__` or `__len__`.

## Comparison Operators

| Operator             | Meaning                |
| -------------------- | ---------------------- |
| `==`                 | equal value            |
| `!=`                 | not equal              |
| `<`, `>`, `<=`, `>=` | ordering               |
| `is` / `is not`      | identity (same object) |
| `in` / `not in`      | membership             |

```python
5 == 5.0          # True
"a" != "b"        # True
3 in [1, 2, 3]    # True
"x" in "xyz"      # True
"k" in {"k": 1}   # True (checks keys)
```

### Chained Comparisons

Python lets you chain, which reads like math:

```python
if 0 <= x < 10:       # same as: 0 <= x and x < 10
    ...

1 < 2 < 3             # True
1 < 3 < 2             # False
a == b == c           # all three equal
```

`x` is evaluated only once, which matters if it's a function call.

### `==` vs `is`

- `==` asks *"do these have equal values?"*
- `is` asks *"are these the same object?"*

```python
a = [1, 2]
b = [1, 2]
a == b      # True   (equal values)
a is b      # False  (two separate objects)

c = a
c is a      # True   (same object, another name)
```

Use `is` only for singletons: `None`, `True`, `False`.

```python
if x is None:       # correct
if x == None:       # works, but not idiomatic
```

Never rely on `is` for numbers or strings. Small ints and some strings are cached, so it appears to work and then fails unpredictably.

### Floating-Point Trap

```python
0.1 + 0.2 == 0.3                # False!
import math
math.isclose(0.1 + 0.2, 0.3)    # True
```

## Logical Operators: `and`, `or`, `not`

```python
age >= 18 and has_id        # both must be truthy
is_admin or is_owner        # at least one
not is_banned               # inverts
```

**Precedence** (high to low): comparisons, then `not`, then `and`, then `or`.

```python
True or False and False     # True  (and binds tighter: True or (False and False))
(True or False) and False   # False
```

When in doubt, add parentheses. They cost nothing and help readers.

### Short-Circuit Evaluation

Python stops as soon as the result is determined.

- `A and B`: if A is falsy, B is **never evaluated**.
- `A or B`: if A is truthy, B is **never evaluated**.

This is useful for guarding against errors:

```python
if user and user.age >= 18:              # safe: user.age only runs if user is truthy
    ...

if len(items) > 0 and items[0] == "x":   # no IndexError
    ...
```

### `and` / `or` Return Operands, Not Just Booleans

```python
"hi" and "there"    # 'there'  (first is truthy, so returns the second)
"" and "there"      # ''       (first is falsy, returns it)
"" or "default"     # 'default'
"val" or "default"  # 'val'
None or 0 or []     # []       (returns the last operand if all are falsy)
```

This enables the default-value idiom:

```python
name = user_input or "Guest"
```

Caveat: this replaces **any** falsy value, including `0` and `""`. If `0` is valid, use:

```python
count = 5 if count is None else count
```

`not` always returns a real bool:

```python
not "hi"    # False
not []      # True
```

## Nested Conditionals

```python
if user.is_active:
    if user.is_admin:
        print("Admin panel")
    else:
        print("Dashboard")
else:
    print("Account disabled")
```

Deep nesting gets hard to read. Two fixes:

**Combine with `and`:**

```python
if user.is_active and user.is_admin:
    ...
```

**Use guard clauses (early return):**

```python
# Nested
def process(order):
    if order:
        if order.is_paid:
            if order.items:
                ship(order)

# Flat and clearer
def process(order):
    if not order:
        return
    if not order.is_paid:
        return
    if not order.items:
        return
    ship(order)
```

## The Ternary (Conditional) Expression

A one-line `if/else` that **produces a value**:

```python
value_if_true if condition else value_if_false
```

```python
status = "adult" if age >= 18 else "minor"
print("even" if n % 2 == 0 else "odd")
```

Chaining is possible but gets ugly fast:

```python
label = "neg" if n < 0 else "zero" if n == 0 else "pos"   # OK once, then stop
```

Don't use it for side effects:

```python
print("a") if x else print("b")   # legal but poor style; use a normal if
```

## Common Idioms

### Membership Instead of Multiple `or`s

```python
# Verbose
if color == "red" or color == "green" or color == "blue":

# Better
if color in ("red", "green", "blue"):
```

**Classic bug:**

```python
if color == "red" or "green":    # ALWAYS True: "green" is a truthy string
```

### Checking Multiple Conditions

```python
all([a > 0, b > 0, c > 0])     # True if every item is truthy
any([a > 0, b > 0, c > 0])     # True if at least one is
all(x > 0 for x in nums)       # generator form
```

`all([])` is `True` and `any([])` is `False`.

### Returning a Boolean Directly

```python
# Redundant
if x > 5:
    return True
else:
    return False

# Direct
return x > 5
```

### Dictionary Dispatch (Replacing Long `elif` Chains)

```python
actions = {
    "start": start_game,
    "stop": stop_game,
    "pause": pause_game,
}
handler = actions.get(command)
if handler:
    handler()
else:
    print("Unknown command")
```

### Input Validation Pattern

```python
text = input("Age: ").strip()
if not text.isdigit():
    print("Enter a number")
elif int(text) < 0 or int(text) > 120:
    print("Out of range")
else:
    print("Valid")
```

## The Walrus Operator `:=` (3.8+)

Assigns and tests in one step:

```python
if (n := len(data)) > 10:
    print(f"Too long: {n} items")

if (match := re.search(r"\d+", text)):
    print(match.group())
```

Useful, but don't overuse it. Parentheses are usually required.

## `match` / `case` (Structural Pattern Matching, 3.10+)

Python's closest thing to a `switch`, but much more powerful.

### Basic Value Matching

```python
match command:
    case "start":
        print("Starting")
    case "stop" | "quit":        # OR pattern
        print("Stopping")
    case _:                      # wildcard (default)
        print("Unknown")
```

### Matching Structure and Capturing Values

```python
match point:
    case (0, 0):
        print("Origin")
    case (x, 0):
        print(f"On x-axis at {x}")
    case (0, y):
        print(f"On y-axis at {y}")
    case (x, y):
        print(f"Point at {x}, {y}")
```

### Guards, Sequences, Dicts, Classes

```python
match event:
    case {"type": "click", "pos": (x, y)}:
        print(f"Click at {x},{y}")
    case [first, *rest]:
        print(first, rest)
    case int(n) if n > 100:         # guard
        print("big int")
    case Point(x=0, y=0):           # class pattern
        print("origin")
```

### Critical Gotchas

A bare name in a pattern is a **capture**, not a comparison:

```python
RED = "red"
match color:
    case RED:       # WRONG: captures any value into RED, always matches
        ...
```

To match against a constant, use a **dotted name** (`Color.RED`) or a literal (`"red"`).

Also: `case _` is the only true wildcard, and there is **no fall-through** (no `break` needed).

Use `match` when matching on **shape/structure**. For simple comparisons, `if/elif` is fine.

## Conditionals in Other Places

### Comprehensions

```python
evens = [n for n in nums if n % 2 == 0]                    # filter
labels = ["even" if n % 2 == 0 else "odd" for n in nums]   # transform
```

A **filter** `if` goes at the end. A **ternary** goes at the front and needs `else`.

### Loops with `else`

```python
for n in nums:
    if n == target:
        print("found")
        break
else:
    print("not found")    # runs only if the loop did NOT break
```

### `assert`

```python
assert x > 0, "x must be positive"    # raises AssertionError if falsy
```

Use for internal sanity checks, not for validating user input (asserts can be disabled with `python -O`).

## Common Pitfalls Checklist

| Mistake                                   | Fix                                                                  |
| ----------------------------------------- | -------------------------------------------------------------------- |
| `if x = 5:`                               | `=` assigns, `==` compares (SyntaxError)                             |
| `if x == 1 or 2:`                         | `if x in (1, 2):` or `x == 1 or x == 2`                              |
| `if x == True:`                           | `if x:`                                                              |
| `if x == None:`                           | `if x is None:`                                                      |
| `if count:` when 0 is valid               | `if count is not None:`                                              |
| Comparing floats with `==`                | `math.isclose()`                                                     |
| Forgetting the colon                      | Every `if/elif/else` needs `:`                                       |
| Inconsistent indentation                  | 4 spaces everywhere                                                  |
| Wrong `elif` order (broad first)          | Order from most specific to most general                             |
| `else if`                                 | The keyword is `elif`                                                |
| `case NAME:` matching a constant          | Use dotted names or literals                                         |
| Comparing input strings to numbers        | `int(input())` first                                                 |
| Using `and`/`or` when you need `&`/`\|`   | `&` `\|` are bitwise; in pandas/NumPy, parenthesize each condition   |

## Cheat Sheet

```python
if a: ...                      # truthiness
if a is None: ...              # singleton check
if 0 <= x < 10: ...            # chained comparison
if x in (1, 2, 3): ...         # membership
if a and b or c: ...           # logic (and before or)
v = A if cond else B           # ternary
v = x or default               # default for falsy
if (n := f()) > 0: ...         # walrus
match v:                       # pattern matching
    case 1 | 2: ...
    case _: ...
```

Falsy: `False None 0 0.0 "" [] () {} set()`

## Practice Exercises

1. Read a number and print whether it's positive, negative, or zero.
2. Determine whether a year is a leap year (divisible by 4, except centuries unless divisible by 400).
3. Given three numbers, print the largest without using `max()`.
4. Classify a score into grades A/B/C/D/F using `if/elif/else`, then rewrite it as a dictionary lookup or `match`.
5. Check if a password is valid: at least 8 characters, contains a digit, and contains an uppercase letter.
6. FizzBuzz for 1 to 100.
7. Given a triangle's three side lengths, print whether it's equilateral, isosceles, scalene, or not a valid triangle.
8. Rewrite this using a chained comparison and a ternary:
   ```python
   if x >= 1 and x <= 10:
       result = "in range"
   else:
       result = "out of range"
   ```
9. Write a `match` statement that handles `("add", a, b)`, `("neg", a)`, and anything else.
10. Predict the output **before** running: `print("" or 0 or [] or "end")`, `print(1 and 2 and 3)`, `print(not [] and "yes")`.
