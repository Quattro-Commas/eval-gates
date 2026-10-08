# eval-gates

Gate patterns for agent systems, collected in one place.

## The thesis

> A guard that does not hold must fail the run.

Most agent tooling treats a check as something that should pass. These three projects take the
opposite position: a gate that cannot be shown to catch a real failure is not a working gate, and
the only honest way to prove one works is to break it on purpose and watch it go red.

This repo is an **index**, not code. The implementations live in their own repositories and are
maintained there.

## The gates

| Project | What it gates | Language | License |
|---|---|---|---|
| [elohim](https://github.com/BoozeLee/elohim) | Numerical claims — publishes a number only when it can re-derive it | Python | MIT |
| [harness](https://github.com/BoozeLee/harness) | Agent edits — blocks protected-path writes and secret reads before an edit lands | Python | AGPL-3.0 |
| [mcp-regression-lab](https://github.com/BoozeLee/mcp-regression-lab) | MCP tool contracts — a renamed or narrowed tool fails CI instead of production | TypeScript | ISC |

### elohim

Eight instruments measure hard mathematics and what an agent claimed about it, pin every result, and
refuse to pass if anything moved — **including the instrument itself**. One of them,
`reproducibility`, holds the other seven to that: it runs each of them, reads back the seal each one
recorded, and fails if the seal does not match.

One instrument ships **tampered copies of the others on purpose**, so that the gate has to be caught
failing. A guard that has never been observed failing has not been shown to work.

### harness

A local-first control plane that turns a normal Git repo into a governed, verifiable agentic
development environment. Stdlib-only Python core.

```
harness scan                       # agent-readiness score, detected gates, protected paths
harness scan --fail-under 70       # same score as a CI floor: exits 1 when readiness is below N
```

### mcp-regression-lab

Snapshots an MCP server's tool contract, diffs it against the last release, lints it for things that
hurt tool selection, and re-runs golden `prompt → expected tool` tests on a local model.
`diff` exits 1 on breaking changes, so it can gate CI.

## Current status

Stated plainly rather than hidden:

- All three projects currently sit at **0 stars and 0 forks**.
- Re-verified **2026-10-08**: **no project in the wider
  [studio portfolio](https://github.com/Quattro-Commas) is failing a test.** `elohim` is green on its
  latest run, five consecutive successes.

  That is **not** the same as all green, and the difference is the point of this repo. One real defect
  exists: `harness` last *executed* CI at `d11de009` on 2026-10-02 and failed on `uv sync --frozen`.
  Its probable fix, six commits later, has never been validated, so `harness` is *probably green and
  unverified*. `terminal221b` and `repotruth` have no executed run on `main` since GitHub began refusing
  to start their jobs over billing, so they are `UNKNOWN`.

  An earlier version of this file published a count of red checks across the portfolio. That was false:
  those runs executed **zero steps** and were never assigned a runner. A job that ran no steps never ran
  a line of the code, so its conclusion is not evidence about the code. The old number described a
  GitHub invoice, not five codebases.

  Re-derive it with `qc-work/check-state.sh`, which prints `steps=` and `runner=` per failing job so the
  refusals are visible rather than silently counted. Note `repotruth` lives at
  `Bakery-street-project/galacticfederation`; `BoozeLee/repotruth` does not resolve.
- Nothing here is in production. There are no customers and no deployments.

## Studio

[Quattro Commas](https://quattro-commas.github.io) — an independent AI engineering studio.
`,` Ideas · `,` Code · `,` Culture · `,` Freedom
