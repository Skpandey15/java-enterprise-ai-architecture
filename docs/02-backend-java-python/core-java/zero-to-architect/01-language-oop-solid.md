# 1. Language, Object Model, OOP and SOLID

## Language contracts that senior engineers must know

Java is statically typed, nominally typed, garbage-collected, and pass-by-value. For object arguments, the copied value is the reference—not the object. Reassigning a parameter never changes the caller's variable; mutating the referenced object can change shared state.

Understand primitives versus wrappers, numeric promotion, overflow, autoboxing, caching of small wrapper values, string pooling, `final`, initialization order, nested classes, enums, records, sealed types, and accessibility. Prefer `.equals()` for value comparison and reserve `==` for primitive values or identity.

## Object modelling

Use encapsulation to protect invariants, not merely to hide fields. A domain object should make invalid state difficult to represent.

```java
public record Money(BigDecimal amount, Currency currency) {
    public Money {
        Objects.requireNonNull(amount);
        Objects.requireNonNull(currency);
        if (amount.signum() < 0) throw new IllegalArgumentException("negative amount");
    }
}
```

Choose composition over inheritance when behavior changes independently. Inheritance is appropriate only when the subtype fully honors the supertype contract (Liskov substitution). Avoid deep hierarchies and “god” base classes.

## SOLID as design feedback

| Principle | Senior interpretation | Typical failure |
|---|---|---|
| SRP | One reason to change, aligned to a business capability | Controller performs validation, SQL and messaging |
| OCP | Add behavior through stable extension points | Repeated switch statements across services |
| LSP | Subtypes preserve observable contracts | Subtype weakens validation or throws surprises |
| ISP | Consumers depend on small role interfaces | One interface forces irrelevant methods |
| DIP | Policy depends on abstractions; details plug in | Domain imports vendor clients |

SOLID is not a mandate to create an interface for every class. Abstraction has a cost. Introduce it where volatility, substitution, testing, or a boundary justifies it.

## Value objects, entities and records

- Entity: identity persists while attributes change.
- Value object: equality comes from immutable values.
- Record: concise data carrier; it is shallowly immutable, so mutable components still require defensive copying.
- Sealed hierarchy: models a closed set of variants and enables exhaustive pattern matching.

## API design rules

Define nullability, mutability, threading, error, performance, and ownership contracts. Prefer small cohesive APIs, immutable request/response objects, defensive copies at trust boundaries, and explicit domain types over strings.

## Interview depth

A strong answer connects OOP to invariants, SOLID to change patterns, and abstractions to operational cost. Discuss why excessive interfaces, inheritance and generic “common” modules increase cognitive coupling.