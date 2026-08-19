<!-- markdownlint-disable -->

# Hardening Report: yegor256--latexmk-action/0.19.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **yegor256--latexmk-action/0.19.0** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All workflow files use mutable action refs (tags/branches) instead of pinned 40-character SHA commits, and action.yml references a Docker image by mutable tag instead of SHA digest. Mutable refs are vulnerable to supply-chain attacks if the upstream tag or branch is moved. Failing references include: actions/checkout@v6, actions/setup-python@v6, Uno-Takashi/checkmake-action@v2, yegor256/copyrights-action@0.0.12, hadolint/hadolint-action@v3.3.0, DavidAnson/markdownlint-cli2-action@v21.0.0, volodya-lombrozo/pdd-action@master, fsfe/reuse-action@v6, ludeeus/action-shellcheck@master, crate-ci/typos@v1.40.0, peter-evans/create-pull-request@v7, g4s8/xcop-action@master, ibiqlik/action-yamllint@v3, and action.yml image: 'docker://yegor256/latexmk-action:0.19.0' (tag, not SHA digest).

Locations:

- `.github/workflows/actionlint.yml:17`
- `.github/workflows/bashate.yml:17`
- `.github/workflows/bashate.yml:19`
- `.github/workflows/checkmake.yml:17`
- `.github/workflows/checkmake.yml:18`
- `.github/workflows/copyrights.yml:17`
- `.github/workflows/copyrights.yml:18`
- `.github/workflows/hadolint.yml:17`
- `.github/workflows/hadolint.yml:18`
- `.github/workflows/markdown-lint.yml:17`
- `.github/workflows/markdown-lint.yml:18`
- `.github/workflows/pdd.yml:17`
- `.github/workflows/pdd.yml:18`
- `.github/workflows/reuse.yml:17`
- `.github/workflows/reuse.yml:18`
- `.github/workflows/shellcheck.yml:17`
- `.github/workflows/shellcheck.yml:18`
- `.github/workflows/test.yml:17`
- `.github/workflows/test.yml:18`
- `.github/workflows/typos.yml:17`
- `.github/workflows/typos.yml:18`
- `.github/workflows/up.yml:17`
- `.github/workflows/up.yml:18`
- `.github/workflows/xcop.yml:17`
- `.github/workflows/xcop.yml:18`
- `.github/workflows/yamllint.yml:17`
- `.github/workflows/yamllint.yml:18`
- `action.yml:10`

### missing-permissions (severity: medium)

None of the workflow files define a top-level `permissions:` block, and no job within any workflow defines its own `permissions:` block. Without explicit permissions, workflows run with the default (potentially broad) token permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/actionlint.yml:1`
- `.github/workflows/bashate.yml:1`
- `.github/workflows/checkmake.yml:1`
- `.github/workflows/copyrights.yml:1`
- `.github/workflows/hadolint.yml:1`
- `.github/workflows/markdown-lint.yml:1`
- `.github/workflows/pdd.yml:1`
- `.github/workflows/reuse.yml:1`
- `.github/workflows/shellcheck.yml:1`
- `.github/workflows/test.yml:1`
- `.github/workflows/typos.yml:1`
- `.github/workflows/up.yml:1`
- `.github/workflows/xcop.yml:1`
- `.github/workflows/yamllint.yml:1`

### unsafe-shell (severity: high)

actionlint.yml downloads and executes a remote script using bash process substitution: `bash <(curl https://raw.githubusercontent.com/rhysd/actionlint/main/scripts/download-actionlint.bash)`. This is functionally equivalent to `curl ... | bash` — remote content is piped directly into a shell interpreter without first being saved to disk and inspected, enabling supply-chain attacks if the remote URL is compromised.

Locations:

- `.github/workflows/actionlint.yml:19`

### script-injection (severity: high)

Sub-rule (a): actionlint.yml directly interpolates a `${{ ... }}` expression inside a `run:` shell command: `run: ${{ steps.get_actionlint.outputs.executable }} -color`. The expression `${{ steps.get_actionlint.outputs.executable }}` is substituted into the shell command string before the shell parses it. If the step output contains shell metacharacters, this results in command injection.

Locations:

- `.github/workflows/actionlint.yml:22`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions, unsafe-shell, script-injection

**Notes:**

Fixed all 4 findings across 15 files:

1. **unpinned-uses**: Pinned all action refs to full SHA commits in all 14 workflow files. Also pinned the Docker image in action.yml to its SHA digest (docker://yegor256/latexmk-action:0.19.0@sha256:644c63a8253f73a52d157b85d5edfd6443c36c789423531c9f9228d56e084f12) while preserving the docker:// scheme and tag.

2. **missing-permissions**: Added top-level `permissions: contents: read` to all 14 workflow files. The up.yml workflow gets `contents: write` and `pull-requests: write` since it creates pull requests.

3. **unsafe-shell** (actionlint.yml): Replaced `bash <(curl ...)` with a two-step approach: download the script to `/tmp/download-actionlint.bash` first, then execute it with `bash /tmp/download-actionlint.bash`.

4. **script-injection** (actionlint.yml): Moved `${{ steps.get_actionlint.outputs.executable }}` out of the `run:` shell string into an `env:` block as `ACTIONLINT_BIN`, then referenced it as `$ACTIONLINT_BIN` in the shell script.

