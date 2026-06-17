<!-- markdownlint-disable -->

# Hardening Report: yegor256--latexmk-action/0.20.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **yegor256--latexmk-action/0.20.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml Docker action references a mutable image tag `docker://yegor256/latexmk-action:0.20.0` instead of an immutable SHA digest. This means the image could be replaced with a different (potentially malicious) version without changing the action reference. It should be pinned to a SHA digest, e.g. `docker://yegor256/latexmk-action@sha256:<64-hex-char-digest>`.

Locations:

- `action.yml:11`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Replaced the mutable Docker image tag `docker://yegor256/latexmk-action:0.20.0` with the immutable SHA digest `docker://yegor256/latexmk-action@sha256:8b82738f2e19f36bbbbaca919f1021426faa4527af7e4944c58826995523eec7` in action.yml line 11. The original tag `0.20.0` is preserved as a comment for readability.

