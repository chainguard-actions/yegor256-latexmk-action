<!-- markdownlint-disable -->

# Hardening Report: yegor256--latexmk-action/0.18.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **yegor256--latexmk-action/0.18.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image reference with a mutable tag (`docker://yegor256/latexmk-action:0.18.1`) instead of an immutable SHA digest. This means the action could silently pull a different (potentially malicious) image if the tag is overwritten on the registry. The image reference should be pinned to a full SHA256 digest, e.g. `docker://yegor256/latexmk-action@sha256:<64-hex-char-digest>`.

Locations:

- `action.yml:11`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned the Docker image reference in action.yml from 'docker://yegor256/latexmk-action:0.18.1' to 'docker://yegor256/latexmk-action@sha256:fc63dd1dcd94e0987e0b029d509e2813c47794bd85d7cb39e6c178f25c9f8250' with the original tag '0.18.1' preserved as a comment for readability.

