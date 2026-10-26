---
name: mini-code-nexus
version: 2.0.0
description: Reduce token usage in AI coding-agent sessions by 20-60% through foveated file retrieval, tool-output compression, response concision, and implementation minimalism. Primary lever replaces lexical minification with layered retrieval. Use when token budget is tight, context window is near limit, or a long session is burning through subscription. NOT for debugging, code review, or line-sensitive edits without disabling compression.
when_to_use: Long sessions. Large repos. Token budget tight. Context window near limit. Multi-agent workflows. NOT during: compile error debugging, stack trace analysis, git diff/code review, config file editing.
argument-hint: "[audit|full|foveated|tool-output|narration|implementation|off] [path]"
allowed-tools: Bash(tree-sitter *) Bash(rg *) Bash(sqlite3 *) Bash(python3 *) Bash(git *)
compatibility: opencode, claude-code, codex, cursor, gemini-cli, copilot-cli
license: MIT
metadata:
  version: 2.0.0
  token_reduction: 20-60%
  levers: 4
  primary_lever: foveated-read
  replaces: lexical-minification
  dependencies: tree-sitter (required for L0), ripgrep + sqlite3 (for dedup), optional RTK/Caveman/Ponytail
---

# Mini-Code-Nexus

A composable token-optimization skill for AI coding agents. Targets four independent waste sources: **file reads, tool output, agent narration, over-engineered implementation**. Ordered by leverage. Measured per session. Safe by default.

## Why v2

v1 compressed the wrong layer. Lexical minification (strip comments, collapse whitespace, drop blank lines) attacks the cheap 10-20% of tokens and removes the highest-density comprehension signal in the file (docstrings, intent, TODOs). It pays the savings back in wrong patches, re-reads, and debugging — which is why v1's own README warns "don't minify then edit" in four separate anti-patterns.

v2 targets the expensive layer: **boilerplate** (imports, license headers, getters/setters, generated code), **repetition** (identical blocks across files), and **re-reads** (the agent re-reading the same unchanged file because it lost track). These are 30-60% of tokens in large repos and they compound with session length. Per-file minification is flat. v2 compounds.

The unit of minimization is not the file. It is the **task's working set** — the minimal set of lines the agent actually needs. Everything else is context that should be summarized, referenced, or fetched on demand.

## The foveated-read model

Four layers, read top-down, descend only when needed.

| Layer | What the agent reads | Token share | When |
|---|---|---|---|
| **L0 — Manifest** | File tree, sizes, content hashes | ~1% of repo | Always, once per session |
| **L1 — Outline** | Per file: exports, signatures, type decls, line ranges. Bodies collapsed to `{...}`. Repeated blocks replaced with references. | ~10-15% of file | Default first read |
| **L2 — Working set** | Raw source of named line ranges | Task-dependent | When the agent names the range |
| **L3 — Full file** | Raw source | 100% | Only when L2 is insufficient |

Three additions on top:

1. **Dedup cache (repo-level).** Hash every contiguous block > 8 lines. If the same import block, license header, or helper appears in 40 files, the outline replaces it with `see: common/imports.py:1-12`. This is where the real savings live.
2. **Session memo.** Track `(path, content_hash)` for every file read. If the agent re-reads an unchanged file, return `already read at turn N, unchanged` instead of re-injecting. Most waste in long sessions is re-reads, not first reads.
3. **Task-conditioned compression.** When the agent names a task ("fix bug in `parse_config`"), keep `parse_config` and its callers raw (L2), summarize everything else (L1). Compression applies to the irrelevant 90%, not the relevant 10%.

## Commands

Install the CLI as a skill directory, then use these commands.

~~~bash
mcn outline <path>              # L1 outline: exports, signatures, line ranges, refs
mcn read <path>:<start>-<end>   # L2 raw range, full fidelity
mcn read <path>                 # L3 raw file, warns that L1 exists
mcn resolve <ref>               # expand a dedup reference to its source
mcn read-status <path>          # "already read, unchanged" or "changed since turn N"
mcn manifest [dir]              # L0 manifest: tree, sizes, hashes
mcn index [dir]                 # build/refresh dedup index (FTS5)
mcn stats                       # session token accounting: reads, bytes, re-reads, dedup hits
~~~

Provider-backed fallbacks (optional, if the CLI is unavailable):

~~~bash
# Context Mode (MCP server) — handles large non-file tool output
/plugin marketplace add mksglu/context-mode
/plugin install context-mode@context-mode
/context-mode:ctx-doctor
/context-mode:ctx-stats
~~~

## Levers

Four levers, ordered by leverage. Each is self-contained. Compose in order: L0 → L1 → L2 → L3.

### L0 — Foveated read (primary, replaces lexical minification)

**Problem.** The agent reads whole files when it needs a few lines. Boilerplate and repetition are read repeatedly. Re-reads of unchanged files burn tokens for zero information gain.

**Strategy.** Layered retrieval. Outline first, raw ranges second, full file only when required. Deduplicate repeated blocks at repo level. Memoize reads by content hash.

**Protocol.**

~~~text
L0 Manifest  ->  file tree, sizes, hashes       (~1% of repo)
L1 Outline   ->  exports, signatures, ranges    (~10-15% of file)
L2 Working   ->  raw line ranges named by task
L3 Full      ->  raw file, only if L2 insufficient
~~~

**Baseline command.**

~~~bash
# Measure current read volume for the session
mcn stats
# or manually:
# total files read, total bytes, re-read count, dedup hit rate
# token estimate = bytes / 4
~~~

**Verification command.**

~~~bash
# After enabling foveated-read, run the same task.
# Success criteria:
#   >= 50% reduction in bytes read
#   zero re-reads of unchanged files
#   the agent edited raw L2 source, not an outline
mcn stats
~~~

**Safety gates.**

- Edits always operate on L2 or L3 raw source. Never on an outline.
- Dedup references must be resolvable. If `resolve` fails, fall back to raw L3.
- Hash check on every read. Stale memo is a correctness bug, not a nuisance.
- If the repo has no AST parser for the language, fall back to L3 raw. Do not fake an outline with regex.
- Never dedup across trust boundaries (secrets, keys, credentials). These remain inline or redacted, never referenced.

**Tools.**

| Tool | Role |
|---|---|
| Tree-sitter + grammars | Required. Produces L1 outlines. |
| ripgrep + sqlite3 FTS5 | Required for dedup index and `resolve`. |
| Context Mode (MCP) | Optional. Handles large non-file tool output (browser snapshots, MCP responses) that L0 does not cover. |
| vix | Reference implementation of the Tree-sitter virtual filesystem idea. |

**Trade-offs.**

- Adds a parser dependency. Breaks the "zero-dep" claim from v1. This is the cost of attacking the expensive layer.
- Requires agent cooperation to request ranges. That is a skill-instruction change, not a tool change.
- Dedup breaks naive `grep`. The `resolve` command is required. Document this for users.
- Savings compound with repo size and session length — the opposite of per-file minification, which is flat.

### L1 — Tool output

**Problem.** Shell commands (`git status`, `pytest`, `npm install`, log tail, search) return hundreds or thousands of lines. Most is useful to a human, but an agent only needs errors, failures, changed files, and summaries.

**Strategy.** Intercept output before the agent sees it. Filter, group, truncate, deduplicate. Preserve errors, failures, and summaries.

**Mechanism.** Shell hook or wrapper. Command-specific filters: git status → changed files grouped by status; pytest → failures + summary; npm/yarn/pip → errors + summary; grep/rg → deduplicated matches with context; docker logs → error/warn + last N lines.

**Baseline command.**

~~~bash
git status | wc -c
pytest --tb=short 2>&1 | wc -c
# token estimate = bytes / 4
~~~

**Verification command.**

~~~bash
# Same commands after enabling filter.
# Success: >= 50% byte reduction on supported commands,
# with zero loss of errors, failures, or changed-file names.
# Diff raw vs filtered error/failure lines. Must be identical.
~~~

**Safety gates.**

- Never filter output when debugging a specific command. The root cause may be in a dropped line.
- Never filter when the agent explicitly requests full output.
- Always preserve exit codes. Filtered command returns the same exit status as raw.
- Errors and failures pass through unfiltered.

**Tools.**

~~~bash
# RTK
brew install rtk-ai/tap/rtk
# or: curl -fsSL https://raw.githubusercontent.com/rtk-ai/rtk/master/install.sh | sh
rtk --version && rtk gain      # verify (another unrelated project uses the name "rtk")
rtk init -g                    # Claude Code
rtk init -g --codex            # Codex
rtk init -g --gemini           # Gemini CLI
~~~

Reported result: 60-90% fewer command-output tokens on supported commands (RTK's benchmark, not yours).

### L2 — Narration

**Problem.** Agents narrate every step, repeat obvious details, pad with pleasantries, hedge, restate the code in prose. Output tokens cost 5x input tokens per model. In long sessions this narration is carried forward in history, compounding the cost.

**Strategy.** Strip filler, pleasantries, hedging, and unnecessary narration. Preserve code blocks, commands, API names, exact error messages, security warnings. Relax terseness when clarity matters (security warnings, irreversible actions).

**Baseline command.**

~~~bash
# Run a representative task. Record completion tokens.
# In Claude Code: /cost or the run summary.
# Record: input tokens, output tokens, output-to-input ratio.
~~~

**Verification command.**

~~~bash
# Same task after enabling. Success: output tokens drop >= 40%
# with no loss of code, commands, error text, or correctness.
# Manual check: every code block and command from baseline is still present.
~~~

**Safety gates.**

- Never compress security warnings or irreversible-action confirmations.
- Never compress error messages. Exact text preserved.
- Never compress code. Code blocks pass through verbatim.
- If the user asks for explanation, give it in full. Rule is against unrequested narration, not requested explanation.

**Tools.**

~~~bash
npx skills add JuliusBrussee/caveman
# activate: /caveman
# deactivate: /caveman off
# optional proxy: npm install -g @caveman-ai/cli && caveman setup --install
~~~

Reported result: up to 65% fewer output tokens (Caveman's benchmark, not yours).

### L3 — Implementation

**Problem.** Agents over-engineer. A simple request becomes new dependencies, helper classes, wrapper components, extra configuration. More code now, larger diffs, more code for the agent to read again later.

**Strategy.** Force a decision ladder before writing code: does this need to exist; does stdlib do it; does a native platform feature cover it; does an existing dependency solve it; can it be one line; only then write the minimum code that works. Never simplify away validation, security, accessibility, or error handling.

**Baseline command.**

~~~bash
git diff --stat
git diff --shortstat
# record: lines added, files created, dependencies added
~~~

**Verification command.**

~~~bash
# Same task after enabling. Success: >= 30% reduction in lines added
# and zero new dependencies for tasks that did not require them.
# Correctness: all tests pass; behavior unchanged.
~~~

**Safety gates.**

- Never simplify away input validation at trust boundaries.
- Never simplify away error handling that prevents data loss.
- Never simplify away security measures or accessibility basics.
- Never simplify away anything explicitly requested by the user.
- If the user insists on the full version, build it. No re-arguing.

**Tools.**

~~~bash
/plugin marketplace add DietrichGebert/ponytail
/plugin install ponytail@ponytail
# levels: /ponytail lite | /ponytail full | /ponytail ultra | /ponytail off
# review: /ponytail-review
# audit:  /ponytail-audit
~~~

Reported result: 54% less code and 22% fewer tokens across 12 tasks (Ponytail's benchmark, not yours).

## Global safety gates

These override any lever rule. If a gate fires, skip the lever for that turn and note why.

| Gate | Rule |
|---|---|
| **G1 edit-mode** | If the agent is about to edit, refactor, or patch code, disable all compression and filtering for files being edited. Line numbers must match raw source. Use raw L2/L3 before edits. |
| **G2 debug-mode** | If debugging a compile error, stack trace, test failure, or runtime error, disable all filtering and compression on error output. Root cause may be in a dropped line. |
| **G3 review-mode** | If doing code review or generating a git diff, use raw source. Minified views misalign diffs and line references. |
| **G4 config-files** | Never minify JSON, YAML, TOML, or DSL files. Exact formatting or line references may be required. |
| **G5 secrets** | Never store secrets, credentials, API keys, or PII in any local index, cache, or compressed store. Filter from logs before indexing. |
| **G6 user-override** | If the user asks for full output, full explanation, or full implementation, honor it. Skill is default-on, not mandatory. |

## Execution protocol (mode: full)

Ordered by leverage. Run in this sequence.

**Step 0 — Baseline.** Before any lever: `mcn stats`, `/context`, `git diff --stat` on the last task. Record input tokens, output tokens, output-to-input ratio, bytes read, lines added, dependencies added. If measurement is unavailable, write "baseline: Not run" and proceed qualitatively. Do not fabricate numbers.

**Step 1 — Enable L0.** Install Tree-sitter grammars. Run `mcn index .` then `mcn manifest`. Run a representative task using `mcn outline` → `mcn read path:start-end` → `mcn read path` only if needed. Run `mcn stats`. Gate: if the current task is an edit or debug task, defer L0 for that task.

**Step 2 — Enable L1.** Install and verify RTK. Run a representative command twice (raw vs filtered) and diff the error/failure lines. Must be identical. Gate: if debugging, disable for that command.

**Step 3 — Enable L2.** Activate Caveman. Run a representative task. Verify every code block and command from baseline is still present. Gate: security warnings and irreversible-action confirmations keep full prose.

**Step 4 — Enable L3.** Activate Ponytail at `full`. Run a representative task. Record diff size. Verify: all tests pass, behavior unchanged. Gate: never simplify away validation, security, accessibility, or error handling.

**Step 5 — Measure.** Compare against baseline per lever, not just combined. The levers do not add linearly. Report context tokens, tool-output bytes, output tokens, lines added, dependencies added — each with before → after → delta → percent.

**Step 6 — Verify correctness.** Run full test suite. Run lint/type check. Run one end-to-end task. If any correctness check fails, disable the lever most likely responsible and re-test. Report which lever was disabled and why.

**Step 7 — Report.** Output the measurement table, correctness verdict, active levers, disabled levers and why, and the next optimization to consider (provider-side caching, model routing, batch processing — hand off to `/Research`).

## Composability

**Safe combinations.**

- `L0 + L1` — both reduce input-side tokens. No conflict.
- `L2 + L3` — both reduce output-side tokens. No conflict.
- `L0 + L1 + L2 + L3` — full stack. Safe if safety gates are respected.

**Overlaps.**

- `L0` and `L1` both handle large shell output. Run the tool-output filter first (L1), then the context indexer stores the filtered text (L0). No double-compression.
- `L2` and `L3` both reduce output tokens but at different layers. Narration affects what the agent says; implementation affects what the agent builds. No conflict, but measure separately.

**Recommended start order.**

1. Ponytail alone (simplest, immediate diff-size win, no parser dependency).
2. Ponytail + Caveman (output + implementation).
3. Add RTK if CLI-heavy.
4. Add foveated-read (L0) last — it is the highest leverage but also the highest setup cost (Tree-sitter grammar per language).
5. Full stack only after each lever is individually verified.

## Anti-patterns

| ID | Rule |
|---|---|
| **A1** | Never edit minified or outlined output. Line offsets are wrong. Use raw L2/L3 before edits. |
| **A2** | Never mix outline and raw views of the same file in one session. Inconsistent mental model leads to wrong patches. |
| **A3** | Never apply compression to config files (JSON, YAML, TOML). No benefit, may break. |
| **A4** | Never use compression for debug, review, or stack-trace analysis. Line numbers shift; grep results mismatch. |
| **A5** | Never stack all four levers from the start. They do not add linearly. Add one at a time. |
| **A6** | Never report a savings number not measured in this session. Tool benchmarks are not your numbers. |
| **A7** | Never compress security warnings, irreversible-action confirmations, or error messages. |
| **A8** | Never store secrets or PII in a local index, cache, or compressed store. |
| **A9** | Never disable a safety gate to hit a token target. Safety beats savings. |
| **A10** | Never present a tool benchmark as the user's expected result. |
| **A11** | Never fake an outline with regex when no AST parser is available. Fall back to L3 raw. |
| **A12** | Never dedup across trust boundaries. Secrets stay inline or redacted. |

## Self-test

Run before reporting success.

1. Did I measure a baseline before enabling any lever? If no, write "baseline: Not run" and do not claim a delta.
2. Did I verify that errors, failures, and changed-file names pass through every filter identically to raw? If not run, say so.
3. Did I confirm all code blocks, commands, and exact error messages survived narration compression? If not checked, say so.
4. Did I run the test suite and confirm behavior unchanged after implementation compression? If not run, write "tests: Not run."
5. Did I report per-lever deltas, not just a combined total?
6. Did I name every lever that was disabled and why?
7. Did I avoid presenting a tool benchmark as the user's result?
8. Did I confirm the agent edited raw L2/L3 source, not an outline?

## Failure behavior

| Case | Behavior |
|---|---|
| **measurement-unavailable** | Write "baseline: Not run." Proceed qualitatively. Do not fabricate token numbers. |
| **tool-not-installable** | Name the blocker and skip that lever. Do not substitute without stating the substitution. |
| **correctness-regression** | Disable that lever, re-run tests, report which lever caused it. Do not ship a passing token number with failing tests. |
| **safety-gate-conflict** | Gate wins. Report the skipped lever and the gate that fired. |
| **user-override** | "normal mode" or "off" disables all levers immediately. Do not argue. |
| **no-ast-parser** | Fall back to L3 raw for that language. Note the fallback. |

## Invocation

~~~text
/mini-code-nexus [mode] [path]

Modes:
  audit           measure current usage, no levers enabled.
                  output: baseline table + top 3 waste sources + recommended order.
  full            run execution protocol steps 0-7.
  foveated        enable only L0. measure bytes-read delta.
  tool-output     enable only L1. measure command-output delta.
  narration       enable only L2. measure output-token delta.
  implementation  enable only L3. measure diff-size delta.
  off             disable all levers. release locks.
~~~

**Aliases.** `/mcn`, `/tokens-optimize`.

**Persistence.** Default single-turn. `/mini-code-nexus lock` keeps the last mode active across turns. Release with "normal mode" or `/mini-code-nexus off`.

## Handoffs

| To | When |
|---|---|
| `/Research` | Provider-side optimization (caching, routing, batch) or tool selection. |
| `/Escanor` | Code audit that may reveal token waste in the codebase itself (dead code, duplicate logic, over-abstraction). |
| `/OTO` | Optimizing a specific pipeline or dataset, not an agent session. |
| `/Pony` | Minimal implementation of a new feature. |
| `/Code` | Correct full implementation when minimalism is not appropriate. |

## Execution notes

1. **Start with audit.** Do not skip to `full`. The audit tells you which lever pays off most for your workflow. CLI-heavy → L1. Chatty → L2. Tool-heavy with many MCP calls → L0. Over-engineered code → L3.
2. **One lever at a time.** Enable, measure, verify correctness, then add the next. Only way to isolate a regression.
3. **Ponytail first if new.** No parser dependency, immediate win, and reducing unnecessary code has a knock-on effect: fewer tokens now, smaller diffs, less code to read later.
4. **Never optimize during edits.** The moment the agent edits, refactors, debugs, or reviews, compression is off for the files involved.
5. **Measure your own delta.** Caveman's 65%, Ponytail's 54%, RTK's 60-90% are their workloads. Your delta comes from your baseline and verification.
6. **Safety beats savings.** Every gate overrides every lever. A token target is never worth a wrong patch, a lost error, or an inaccessible UI.
7. **Hand off when out of scope.** Provider-side caching, routing, batching are outside this skill. Hand off to `/Research`.
8. **Report honestly.** If a lever was skipped, say so. If a measurement was not run, write "Not run." If tests were not run, write "tests: Not run."
