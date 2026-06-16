<!-- markdownlint-disable -->

# Hardening Report: yegor256--latexmk-action/0.19.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **yegor256--latexmk-action/0.19.0** was hardened automatically. 1 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action uses a Docker image referenced by a mutable version tag rather than an immutable SHA digest. `image: 'docker://yegor256/latexmk-action:0.19.0'` uses the tag `0.19.0`, which can be overwritten at any time, enabling supply-chain attacks. It should be pinned to a full SHA256 digest, e.g. `image: 'docker://yegor256/latexmk-action@sha256:<64-hex-char-digest>'`.

Locations:

- `action.yml:10`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Replaced the mutable Docker image tag `yegor256/latexmk-action:0.19.0` with the immutable SHA256 digest `yegor256/latexmk-action@sha256:644c63a8253f73a52d157b85d5edfd6443c36c789423531c9f9228d56e084f12` in action.yml line 10. The original tag is preserved as a comment for readability.

### Iteration 2

**Fixes applied:** suspicious-run-content

**Notes:**

Replaced `eval "${INPUT_CMD} ${opts[*]}"` at entry.sh line 50 with a safe array-based execution. The fix reads INPUT_CMD into a bash array using `read -r -a cmd <<< "${INPUT_CMD}"` (consistent with the pattern already used for INPUT_DEBS, INPUT_PACKAGES, and INPUT_OPTS in the same script), then executes `"${cmd[@]}" "${opts[@]}"`. This eliminates the eval-dynamic vulnerability by preventing shell interpretation of user-controlled input — each word is passed as a discrete argument rather than being evaluated as shell code.

