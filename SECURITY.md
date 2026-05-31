# Security Policy

## Reporting a vulnerability

If you discover a security vulnerability in Vouchsafe — for example a path
traversal, an injection sink, a way to make the scanner crash on a malicious
repository, or anything that could let a scanned repo affect the scanner host —
**please do not open a public issue**.

Instead, email **kurri.govindareddy@gmail.com** with:

- A clear description of the issue
- Steps to reproduce (a minimal failing repo or input file is ideal)
- The version of Vouchsafe you tested (output of `vouchsafe --version`)
- Your assessment of impact

I aim to acknowledge reports within **72 hours** and ship a fix within
**30 days** for high-severity issues.

You will be credited in the release notes of the fix unless you ask to remain
anonymous.

## Supported versions

| Version | Status |
|---|---|
| 0.1.x | Active (pre-alpha; expect rapid iteration) |
| < 0.1 | Unsupported |

After v1.0, the most recent two minor versions will receive security fixes.

## Threat model

Vouchsafe is a **read-only static scanner** that runs as a CLI with the user's
filesystem permissions. It must:

1. Never execute, `import`, `pickle.load`, `eval`, or `subprocess` any content from the scanned filesystem.
2. Never make network calls during a scan. Network is reserved for an explicit `--update-corpus` action (not yet implemented).
3. Treat all file content as untrusted text. Use safe parsers (`json`, `tomllib`, `yaml.safe_load`, regex with explicit bounds) only.
4. Never read or transmit secret values — env-var detector records names only.
5. Produce identical output for identical input, signed (Ed25519: planned) so any post-hoc tampering is detectable.

A malicious repository should not be able to:

- Cause Vouchsafe to crash with anything other than a graceful error
- Cause Vouchsafe to exfiltrate data
- Cause Vouchsafe to execute code from the scanned tree
- Bypass file size / time limits to DoS the scan host

If you find a way to do any of these, that's a security vulnerability — report it as above.

## Out of scope

The following are intentionally **not** treated as security issues:

- False positives or false negatives in detectors (open a regular issue)
- Detector coverage gaps (open a feature request)
- Cosmetic issues in output formatting
- Issues that require local code execution on the scanner host before Vouchsafe runs

## Signing keys (future)

When Ed25519 signing is implemented (v0.6.0), the public key for verifying
release binaries and report signatures will be published here. Until then,
report integrity is not cryptographically attested.
