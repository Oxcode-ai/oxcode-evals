# Terminal-Bench 2.1: 73 of 89 solved (82.0%)

**OxCode, one attempt per task, network access on, graded by Terminal-Bench's own tests. Run on 28 September 2026.**

| | |
|---|---|
| Benchmark | [Terminal-Bench 2.1](https://www.tbench.ai/), all 89 tasks |
| Solved | **73 of 89 (82.0%)** |
| Attempts | **one per task** (k = 1). The public leaderboard averages several attempts per task, so this is not the same measurement as a leaderboard entry |
| Network | **on** during the agent's work. Task containers could reach package mirrors, which many tasks need for installing software |
| Graded by | each task's own hidden tests, run by the benchmark's verifier. A task counts as solved only on a reward of 1.0 |

---

## Check it yourself

[`results.tsv`](results.tsv) has one row per task:

| Column | Meaning |
|---|---|
| `task` | the Terminal-Bench 2.1 task name |
| `solved` | `yes` if the task's hidden tests passed |
| `outcome` | how the attempt ended (see below) |
| `model_calls` | how many model calls the agent made on that task |
| `agent_seconds` | how long the agent worked on it |

The tasks themselves, and their tests, are published by Terminal-Bench. Every task listed as solved here can be run again with your own agent, or checked against the benchmark's own tests.

---

## How it was run

- **The agent:** the OxCode CLI in its single-loop mode: one conversation, in which the model works until it decides the task is done and calls `finish`. That is the engine behind OxCode's Agent mode from version 0.8.0. It was a development build from 28 September 2026, using one model for every call, through OxCode's own model gateway.
- **Permissions:** full access inside each task's container, so the agent was not stopped for approvals. OxCode's safety checks stayed on.
- **The harness:** Harbor 0.22.0, the runner for [Terminal-Bench](https://github.com/laude-institute/terminal-bench) 2.1, with OxCode installed as the agent. 8 tasks ran at a time.
- **What it saw:** the task's own instructions, handed to the CLI in a file. Nothing from the task's tests.
- **Time:** Terminal-Bench sets a time limit for each task. OxCode gave itself a deadline just under it, to leave room to wrap up. The whole run took 2 hours 19 minutes and made 3,281 model calls.

---

## The result in detail

| Outcome | Tasks |
|---|:--:|
| `solved`: the hidden tests passed | **73** |
| `tests_failed_it`: the agent called `finish`, and the hidden tests failed | 9 |
| `out_of_time`: OxCode's deadline ended the attempt, and the work was not passing | 6 |
| `request_timeout`: see the note below | 1 |
| **Total** | **89** |

13 attempts reached the deadline. 7 of those had already got the work passing, so they count as solved.

### Notes on specific tasks

- **`torch-pipeline-parallelism` is labelled `request_timeout`, and the label is wrong.** The attempt was ended by OxCode's own halt after five consecutive failed edits, not by a timeout. It is a loss either way.
- **`qemu-alpine-ssh` and `qemu-startup` could not be graded.** On the day of the run, the tests' own setup step (`apt-get install -y curl`) failed: Debian no longer serves the package files its index listed. The tests never ran, for any agent we ran, so no agent could pass these two that day. They are counted as not solved and stay in the denominator.

---

## What this result does not include

- **Several attempts per task.** This is one attempt each. A multi-attempt run would measure something different.
- **A closed-book condition.** The network was on. Our [SWE-bench Verified result](../../swe-bench-verified/2026-09-25/) is the one run closed book.
- **A model name.** The agent is OxCode as a whole. We do not publish which provider serves it.
- **Agent transcripts.**

If something here does not add up, open an issue.
