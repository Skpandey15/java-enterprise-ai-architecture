# 5. Exceptions, I/O, NIO and Serialization

## Exception design

Use exceptions for exceptional paths, not normal branching. Checked exceptions can express recoverable obligations at stable APIs; unchecked exceptions suit programming errors and domain/runtime failures. Preserve the cause, add safe context, and translate exceptions only at architectural boundaries.

Never catch `Throwable` broadly, swallow interrupts, log-and-rethrow at every layer, or expose stack traces/secrets to clients.

```java
try {
    return repository.load(id);
} catch (SQLException ex) {
    throw new PersistenceFailure("Could not load policy " + id, ex);
}
```

For `InterruptedException`, either propagate it or restore the interrupt flag. Try-with-resources closes in reverse order and preserves close failures as suppressed exceptions.

## I/O choices

- Classic streams/readers: simple blocking byte/character processing.
- Buffered I/O: reduces system calls.
- NIO channels/buffers: explicit buffer state and scalable file/network building blocks.
- NIO.2: `Path`, filesystem operations, async channels and watch service.
- Memory mapping: useful for some large-file/random-access workloads, with lifecycle caveats.

Always define charset explicitly, stream large payloads rather than loading everything, bound sizes, validate paths, and close resources.

## Serialization

Native Java serialization is dangerous for untrusted data, brittle across versions and often inappropriate for service contracts. Prefer explicit formats such as JSON, Avro or Protobuf with schemas, validation, allowlists and versioning.

A production serialization contract covers:

- compatibility direction;
- required/default fields;
- numeric precision and time zones;
- polymorphism controls;
- payload size limits;
- unknown-field handling;
- sensitive-field redaction.

## Filesystem security

Normalize and validate paths against an allowed root to prevent traversal. Avoid TOCTOU assumptions, unsafe temporary-file names and broad permissions. Treat uploaded content as untrusted; validate type, size and malware policy outside filename extensions.

## Reliability

Timeouts, retry and idempotency apply to I/O. A retry after an ambiguous timeout may duplicate an external side effect. Use request IDs/idempotency keys and distinguish connection, read, total-operation and queue timeouts.

Senior interviews expect you to connect resource management with backpressure, security and failure semantics.