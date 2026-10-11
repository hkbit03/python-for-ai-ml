# Python Variables

-   **variable** - name that refers to an object in memory.
-   When a variable is assigned a value, python binds the name to the
    corresponding object rather than requiring the programmer to declare
    a fixed memory location or data type explicitly
- A **variable** is a symbolic name that references (points to) an object stored in memory. You create a variable by assigning a value to a name using = operator; no explicit type declaration is needed because Python is dynamically typed.
``` python
x = 10
name = "Python"
price = 99.5
```

-   `x` refers to the integer object `10`
-   `name` refers to the string object `"Python"`
-   `price` refers to the floating-pt object `99.5`

**Important Technical points** - **Dynamic Typing** - You do not need to
declare a variable's type explicitly. - **Name binding** - Assignment
associates a name with an object - **Reassignment** - A name can refer
to objects of different types at different times. - **Object
Identity** - Multiple variables can refer to the same object - **Type
determination** - Python determines an object's type at runtime.

``` python
x = 10
print(type(x)) # <class 'int'>

x = "Hello"
print(type(x)) # <class 'str'>
```

**Interview Ready Defintion**

*A variable in Python is an identifier that references an object. Python
uses dynamic typing, so the type belongs to the object rather than being
permanently declared for the variable name. Assignment creates or
updates the name-to-object binding*

``` python
# Integer
a = 2
print(a)
# Output: 2
```

``` python
# Integer
b = 9223372036854775807
print(b)
# Output: 9223372036854775807
```

``` python
# Floating point
pi = 3.14
print(pi)
# Output: 3.14
```

``` python
# String
c = 'A'
print(c)
# Output: A
```

``` python
# String    
name = 'John Doe'
print(name)
# Output: John Doe
```

``` python
# Boolean    
q = True
print(q)
# Output: True
```

``` python
# Empty value or null data type
x = None
print(x)
# Output: None
```

Variable assignment works - left to right - so the following will give
you a syntax error

``` python
0 = x
# => Output: SyntaxError: can't assign to literal
```

You cannot use python's keywords as a valid variable name. You can see
the list of keywords by:

``` python
import keyword
print(keyword.kwlist)
```

**Rules for variable naming:** 1. variables names must start with a
letter or an underscore`_`

``` python
 x  = True   # valid
 _y = True   # valid
 9x = False  # starts with numeral
#  => SyntaxError: invalid syntax  
 $y = False #  starts with symbol
#  => SyntaxError: invalid syntax
```

2.  The remainder of your variable name may consist of letters, numbers
    and underscores

``` python
has_0_in_it = "Still Valid"
```

3.  Names are case sensitive

``` python
x = 9
y = X*5
# NameError: name 'X' is not defined
```

When you use = to do an assignment operation, what's on the left of = is
a name for the object on the right. Finally, what = does is assign the
reference of the object on the right to the name on the left.

``` python
a_name = an_object  # "a_name" is now a name for the reference to the object "an_object"
```


[<prev](/type_system.md) | [next>](/datatype.md)