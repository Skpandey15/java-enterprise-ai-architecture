# 2. Collections and Pythonic Coding

## Choose the right structure

| Type | Best use | Typical lookup |
|---|---|---|
| `list` | ordered, mutable sequence | index O(1), membership O(n) |
| `tuple` | fixed record or immutable sequence | index O(1) |
| `dict` | key/value lookup | average O(1) |
| `set` | uniqueness and membership | average O(1) |
| `deque` | queue operations at both ends | O(1) ends |

Dictionary and set keys must be hashable. Avoid relying on incidental ordering when the business contract requires explicit sorting.

```python
scores = {"java": 82, "python": 91, "sql": 76}
passed = {topic: score for topic, score in scores.items() if score >= 80}
top = sorted(scores.items(), key=lambda item: item[1], reverse=True)
```

## Iteration tools

Know `enumerate`, `zip`, `range`, `sorted`, `any`, `all`, `min`, `max` and `sum`. Prefer a clear loop when a nested comprehension becomes difficult to read.

Generators yield values lazily and reduce peak memory:

```python
def normalized(lines):
    for line in lines:
        value = line.strip()
        if value:
            yield value.lower()
```

## Copying and immutability

Assignment does not copy. A shallow copy duplicates the outer container but shares nested objects. `copy.deepcopy` recursively copies, but can be expensive and sometimes indicates unclear ownership.

## Practical rules

- use sets for membership checks in large collections;
- use `collections.Counter` for frequencies and `defaultdict` for grouping;
- do not modify a collection while iterating over it;
- prefer iterator pipelines for large inputs, but measure before optimizing;
- express sorting and duplicate-handling requirements explicitly.

## Interview checks

Explain list versus tuple, shallow versus deep copy, dictionary key rules, generator benefits, and the time complexity of common operations.

