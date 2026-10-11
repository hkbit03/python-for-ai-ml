# Type System

- Python type system is **dynamic** and **strongly typed**
- **Dynamically Typed**:
    - Python does not require you to declare variable types
    - types are checked at **runtime**, not compile time.
    - variable is just a name bound to an object in the memory; the object has a type, not the variable itself.
- **Strongly Typed**:
    - Python is *strongly typed*: operations that don't make sense for a given type raise errors instead of silently coercing types.
    - Example:
    ```python
    "5"+3 # TypeError: can't concatenate str and int
    ```



[<prev](/README.md) | [next>](/variables.md)