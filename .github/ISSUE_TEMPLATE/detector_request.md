---
name: Detector request
about: Ask for Vouchsafe to detect a specific AI SDK, framework, or pattern.
title: '[detector] '
labels: detector-request
---

## What should Vouchsafe detect?

Describe the AI library, framework, MCP server, vector DB, cloud service, or pattern.

## Why it matters

How widely is it used? Is it part of a regulated compliance workflow? Is it a competitor we miss?

## Detection signal

What exactly should we look for? (manifest entry, import statement, file extension, magic bytes, URL pattern, config filename, etc.)

Examples:

```python
# Python import that should trigger
from newco_ai import NewCoClient
```

```json
// package.json entry that should trigger
"@newco/sdk": "^1.0.0"
```

## Suggested mapping (optional)

Should this map to any specific framework reference? E.g.:

- ISO 42001 A.10.3 (third-party supplier)
- OWASP LLM03 (Supply Chain)
- MITRE ATLAS AML.T0010

## Sources / docs

Links to the vendor's documentation, public API reference, or any third-party reference confirming the signal.
