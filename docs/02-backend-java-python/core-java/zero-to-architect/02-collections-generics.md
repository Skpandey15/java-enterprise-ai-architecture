# 2. Collections, Generics, Equality and Immutability

## Select collections by workload

| Need | Typical choice | Important caveat |
|---|---|---|
| Ordered indexed access | `ArrayList` | Middle insertion is O(n) |
| Unique values | `HashSet` | Depends on stable equality/hash |
| Sorted keys | `TreeMap` | O(log n), comparator must be consistent |
| Insertion/access order | `LinkedHashMap` | Useful for bounded LRU policy |
| Concurrent key lookup | `ConcurrentHashMap` | Compound operations require atomic APIs |
| FIFO/deque | `ArrayDeque` | Prefer over `Stack` |
| Immutable snapshot | `List.copyOf` | Element objects may still be mutable |

A `HashMap` uses hash spreading, buckets, equality checks, resizing, and—under heavy collision conditions—tree bins. Average lookup is O(1), but correctness depends on the `equals/hashCode` contract. Never mutate fields used by a key while it is in a hash-based collection.

```java
cache.computeIfAbsent(key, this::load); // atomic per key in ConcurrentHashMap
```

Do not assume this makes `load` safe, fast, non-recursive, or suitable for unbounded cache growth.

## Equality contract

`equals` must be reflexive, symmetric, transitive, consistent, and false for null. Equal objects must have equal hash codes. Prefer immutable value fields. Entity equality needs an explicit lifecycle policy; generated database IDs can be absent before persistence.

## Generics

Generics provide compile-time type safety through erasure. Runtime usually sees raw erased types, which explains bridge methods, heap pollution, lack of `new T()`, and restrictions on generic arrays.

PECS:

- Producer extends: `List<? extends Number>` is read-oriented.
- Consumer super: `List<? super Integer>` accepts integers.

Use bounded type parameters when values have relationships; use wildcards for flexible API boundaries. Avoid raw types and unchecked casts. Generic invariance means `List<Integer>` is not a subtype of `List<Number>`.

## Immutability

Immutable objects simplify concurrency, caching and reasoning. Use final fields, validation at construction, no leaked mutable references, and defensive copies. Unmodifiable views are not immutable snapshots.

## Production questions

- Is the collection bounded?
- What is the contention and allocation profile?
- Is iteration weakly consistent, fail-fast, or snapshot-based?
- Are keys stable?
- Does the comparator agree with equality?
- Could a cache stampede or memory leak occur?

Senior interviews often test not the collection name, but the behavior during resizing, concurrency, skewed keys, or mutable state.