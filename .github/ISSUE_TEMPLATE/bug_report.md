---
name: Bug report
about: Something is broken or doesn't behave as documented.
title: '[bug] '
labels: bug
---

## Describe the bug

A clear and concise description of what the bug is.

## To reproduce

Steps to reproduce the behavior. Ideally a minimal failing example, e.g.:

```
# Create a fixture
mkdir /tmp/fixture
echo 'import openai' > /tmp/fixture/app.py

# Run vouchsafe
vouchsafe scan /tmp/fixture --format json --output /tmp/vs-out
```

## Expected behavior

What you expected to happen.

## Actual behavior

What actually happened. Paste the relevant output:

```
(paste output here, redacting anything sensitive)
```

## Environment

- Vouchsafe version: (run `vouchsafe --version`)
- OS: (Windows / macOS / Linux + version)
- Python version: (run `python --version`)
- Installation method: (PyPI / standalone binary / from source / Codespaces)

## Additional context

Anything else that helps diagnose — does it happen on every run? Only with certain repo layouts? Specific file types? Etc.
