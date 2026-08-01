# 1. Python Language Fundamentals

## Runtime model

Python is dynamically typed: names reference objects, and objects carry their types. CPython compiles source into bytecode and executes it in a virtual machine. Python passes object references by assignment; mutation is visible through shared references, while rebinding is local.

```python
def add_tag(tags: list[str]) -> None:
    tags.append("python")       # mutates the caller's list

def replace(tags: list[str]) -> None:
    tags = ["new"]              # only rebinds the local name
```

Use `is` for identity—especially `value is None`—and `==` for value equality.

## Essential subjects

- scalar types: `int`, `float`, `bool`, `str`, `bytes`, `None`
- truthiness and `and`, `or`, `not`
- `if`, `match`, `for`, `while`, `break`, `continue`
- functions, return values, positional/keyword arguments
- default, `*args`, keyword-only and `**kwargs` parameters
- scope: local, enclosing, global and built-in (LEGB)
- type hints, unions, aliases and `Optional`

```python
def calculate_score(correct: int, total: int, *, bonus: float = 0.0) -> float:
    if total <= 0:
        raise ValueError("total must be positive")
    return round((correct / total) * 100 + bonus, 2)
```

Never use a mutable default such as `items=[]`; it is created once at function definition. Use `None` and construct a new list inside, or use `dataclasses.field(default_factory=list)`.

## Interview checks

1. Why can two names observe the same list mutation?
2. What is the difference between `is` and `==`?
3. Why are type hints valuable if Python does not enforce them at runtime?
4. Explain mutable default arguments with an example.

## Practice

Implement validation and scoring functions with type hints. Test empty input, boundary values, incorrect types at the API boundary, and floating-point rounding.

