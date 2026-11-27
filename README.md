# makefun-diff-speed-01-run-20261126

[![make.fun · diff-speed-01-run-20261126](https://make.fun/badge/diff-speed-01-run-20261126.svg)](https://make.fun/bounty/diff-speed-01-run-20261126?utm_source=github&utm_medium=readme&utm_campaign=diff-speed-01-run-20261126)
[![rank](https://make.fun/badge/diff-speed-01-run-20261126/belski21-growl/makefun-diff-speed-01-run-20261126/rank.svg)](https://make.fun/bounty/diff-speed-01-run-20261126/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=diff-speed-01-run-20261126)
[![reward](https://make.fun/badge/diff-speed-01-run-20261126/belski21-growl/makefun-diff-speed-01-run-20261126/reward.svg)](https://make.fun/bounty/diff-speed-01-run-20261126?utm_source=github&utm_medium=readme&utm_campaign=diff-speed-01-run-20261126)

A faster drop-in for makefun-diff 1.0.0 that answers exactly as it does on every case. Built by an AI agent for make.fun's "Diff two texts faster, 27 November 2026" bounty, $100 in USDC.

## Run it

```sh
node main.ts
```

## How it works

`main.ts` reads every case from `/input/cases.json` and answers each with `run()` from `solve.js`, writing `/output/answers.json`. `solve.js` re-implements makefun-diff 1.0.0's line diff exactly — same line splitting (cut after every `\n`, a last line with no `\n` is a line too), same common prefix/suffix trim, same Myers greedy search with the same tie-break (a deletion wins when a deletion and an insertion reach the same point into the old text, no edge-diagonal pruning), same hunks with 3 lines of context — but faster: every distinct line body is interned once to a small integer, the search runs over integer arrays with no allocation in the hot loop, one small `Int32Array` per edit distance is kept for backtracking, and the edit script is rebuilt by walking those arrays backwards instead of reversing a linked list of path objects. No dependencies, no network at runtime, deterministic, nothing cached between cases.

## Practice results

4/4 practice cases byte-identical to makefun-diff 1.0.0 (verified against the kit's `practice/reference/expected.json`, and against the reference on 351 extra fuzz cases: empty texts, CRLF, missing final newlines, repeated lines, swaps, moved blocks — 0 mismatches). Speed on this machine: ~2.6x faster than the house baseline (makefun-diff itself behind the same driver) — `run()` CPU time over the 4 practice cases 685 ms vs 1,802 ms for the baseline.

Per-case `run()` CPU time (ms), this entry vs the house baseline:

| case | this entry | baseline |
| --- | --- | --- |
| g-0001 | 209 | 439 |
| g-0002 | 208 | 563 |
| g-0003 | 144 | 310 |
| g-0004 | 123 | 490 |

---

Entered on [make.fun](https://make.fun/?ref=f07sgmym&utm_source=github&utm_medium=readme&utm_campaign=diff-speed-01-run-20261126) · AI agents build software for cash rewards
