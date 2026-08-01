# 3. OOP, Dataclasses and Clean Code

Python supports encapsulation by convention, inheritance, polymorphism and composition. Prefer composition when behavior can be assembled without a rigid inheritance hierarchy.

```python
from dataclasses import dataclass
from typing import Protocol

class QuestionStore(Protocol):
    def save(self, question: "Question") -> None: ...

@dataclass(frozen=True, slots=True)
class Question:
    topic: str
    text: str

class QuestionService:
    def __init__(self, store: QuestionStore) -> None:
        self._store = store

    def create(self, topic: str, text: str) -> Question:
        question = Question(topic=topic.strip(), text=text.strip())
        self._store.save(question)
        return question
```

`Protocol` enables structural typing: an object is compatible when it provides the required behavior. Dataclasses reduce boilerplate; `frozen=True` communicates value semantics but does not recursively freeze nested objects.

## SOLID at this level

- keep request parsing separate from business rules and persistence;
- depend on small behavior-oriented interfaces;
- extend with strategies rather than large `if/elif` chains;
- inject clocks, repositories and external clients for testability;
- avoid “manager” classes that own unrelated responsibilities.

## Python data model

Understand `__init__`, `__repr__`, `__eq__`, `__hash__`, context managers and properties. Use dunder methods only when the object genuinely behaves like the corresponding Python concept.

## Code-review checklist

- names reveal intent;
- functions do one coherent job;
- invalid states are rejected early;
- domain rules do not depend on HTTP or database details;
- public functions have useful type hints;
- duplication is removed only when the shared abstraction is clear.

