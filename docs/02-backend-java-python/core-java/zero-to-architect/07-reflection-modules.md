# 7. Reflection, Annotations, Modules and Class Loading

## Reflection

Reflection inspects and invokes types at runtime. Frameworks use it for dependency injection, persistence, serialization and testing. Costs include weaker compile-time guarantees, access complications, startup work, native-image configuration and harder refactoring.

Cache validated metadata where appropriate; never accept arbitrary class or method names from untrusted input. Prefer `MethodHandle` or generated code when performance and type adaptation matter.

## Annotations

Annotations are metadata, not behavior by themselves. Retention determines availability (SOURCE, CLASS, RUNTIME); target constrains locations. Annotation processors generate or validate code at compile time and can be safer than runtime reflection.

A custom annotation must have a clear processor/interceptor, documented ordering, failure behavior and test strategy. Beware self-invocation and proxy boundaries in Spring annotations such as transactional or async behavior.

## Class loading

Class loaders follow delegation and define type identity: the same binary name loaded by two class loaders represents different types. Loading, linking and initialization are separate. Static initialization is thread-safe but can cause deadlocks, slow startup or irreversible failure.

Class-loader leaks commonly arise from application-server redeployments, static registries, threads, ThreadLocals, drivers and caches retaining classes from an obsolete loader.

## JPMS

The Java Platform Module System provides explicit dependencies and strong encapsulation using `module-info.java`.

- `requires`: dependency.
- `exports`: public API package.
- `opens`: reflective access.
- `uses/provides`: service loading.

JPMS can improve boundaries but adds migration cost for reflective frameworks and split packages. Do not confuse JPMS modules with business/domain modules; they can reinforce each other but solve different problems.

## ServiceLoader and plugins

Use a stable service interface plus `ServiceLoader` for controlled plugin discovery. Version the SPI, isolate failures, validate implementations and consider class-loader/security boundaries.

## Architect questions

- Can compile-time generation replace reflection?
- Which packages must truly be opened?
- Is plugin code trusted?
- How are startup and native-image impacts measured?
- Can class unloading occur?
- Are framework proxies compatible with final methods/classes and invocation paths?

Senior engineers treat “magic annotations” as executable architecture with boundaries and failure modes.