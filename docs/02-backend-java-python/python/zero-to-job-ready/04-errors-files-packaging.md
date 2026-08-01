# 4. Errors, Files, Packages and Environments

## Exception discipline

Raise specific exceptions, catch only what you can handle, preserve the cause with `raise ... from ...`, and never use a silent broad `except Exception`.

```python
class QuestionNotFoundError(LookupError):
    pass

try:
    payload = external_client.fetch(question_id)
except TimeoutError as exc:
    raise QuestionNotFoundError(question_id) from exc
```

Translate domain exceptions to HTTP responses at the API boundary. Log an exception once at the boundary with useful context; repeated logging at every layer creates noise.

## Resource safety

Context managers close resources even on failure:

```python
from pathlib import Path

with Path("questions.json").open(encoding="utf-8") as source:
    content = source.read()
```

Prefer `pathlib`, explicit encodings, streaming for large files, and validated paths for user-controlled filenames.

## Modules and dependencies

- organize code into importable packages with clear public boundaries;
- use `pyproject.toml` as project metadata;
- isolate dependencies in a virtual environment;
- separate runtime and development dependencies;
- pin reproducible application deployments and scan dependencies;
- never commit `.env`, tokens, virtual environments or caches.

Know absolute versus relative imports, the purpose of `if __name__ == "__main__":`, and why circular imports usually indicate poor boundaries.

## Serialization

JSON is interoperable but supports fewer types. Do not deserialize untrusted `pickle` data because it can execute code. Validate external data with a schema library such as Pydantic.

