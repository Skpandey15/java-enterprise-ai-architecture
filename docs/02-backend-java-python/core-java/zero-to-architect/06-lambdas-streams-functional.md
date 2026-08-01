# 6. Lambdas, Streams and Functional Design

Lambdas implement functional interfaces and capture effectively-final variables. Method references improve clarity when the referenced operation is obvious. Use standard interfaces (`Function`, `Predicate`, `Consumer`, `Supplier`) before inventing new ones.

## Streams

A stream pipeline has a source, lazy intermediate operations and a terminal operation. Streams are single-use and do not store data.

```java
Map<Tier, Long> counts = customers.stream()
    .filter(Customer::active)
    .collect(Collectors.groupingBy(Customer::tier, Collectors.counting()));
```

Prefer readable pipelines. Avoid hidden I/O, mutation, exceptions-as-control-flow and deeply nested collectors. A loop is often clearer for stateful, short-circuit-heavy or diagnostic logic.

## Parallel streams

Parallelism uses the common ForkJoinPool by default. It is not automatically faster. It needs sufficiently large data, splittable sources, associative/stateless operations, low coordination cost, and spare CPU. Blocking I/O or shared mutable accumulation is a poor fit. Benchmark realistic inputs and consider interference with other common-pool users.

## Optional

Use `Optional` mainly as a return type for absence. Avoid it in fields, DTOs and method parameters unless a framework/domain convention justifies it. Never call `get()` without proving presence; prefer `map`, `flatMap`, `orElseThrow`. Remember `orElse` eagerly evaluates while `orElseGet` is lazy.

## Functional architecture

Functional style helps isolate pure decision logic from side effects. A useful service shape is:

1. validate and normalize input;
2. compute decision using pure functions;
3. perform side effects at explicit ports;
4. emit observable outcome.

Immutability and referential transparency simplify tests and concurrency, but Java remains multi-paradigm. Do not force domain logic into obscure combinators.

## Review questions

- Is encounter order required?
- Is the operation associative?
- Does the pipeline allocate excessively?
- Are nulls possible?
- Are side effects explicit?
- Would a loop communicate intent better?
- Is parallel execution isolated and measured?

Senior judgment is knowing when streams improve declarative clarity and when they conceal operational behavior.