# SWE-bench Verified: 410 of 500 resolved (82.0%)

**OxCode, closed book, one attempt per instance, graded by the official SWE-bench harness. Run on 25 September 2026.**

| | |
|---|---|
| Benchmark | [SWE-bench Verified](https://www.swebench.com/), all 500 instances |
| Resolved | **410 of 500 (82.0%)** |
| Attempts | one per instance, no retries, no instance re-run |
| Excluded | none. Every one of the 500 is in the denominator |
| Network | **closed book.** No internet access during the agent phase ([proof](#closed-book)) |
| Graded by | the official SWE-bench evaluation harness, not by us |

---

## Check it yourself

Everything needed to re-grade this result is in this folder.

| File | What it is |
|---|---|
| [`predictions.jsonl`](predictions.jsonl) | the patch OxCode produced for each of the 500 instances, in the harness's own input format |
| [`report.json`](report.json) | the official harness's report for those patches, unedited |
| [`closed-book-probe.txt`](closed-book-probe.txt) | the network check run inside the benchmark image before the run |

To grade the patches yourself with the [official harness](https://github.com/SWE-bench/SWE-bench):

```bash
pip install swebench
python -m swebench.harness.run_evaluation \
  --dataset_name SWE-bench/SWE-bench_Verified \
  --predictions_path predictions.jsonl \
  --max_workers 8 \
  --run_id oxcode-2026-09-25
```

Your report should list the same 410 resolved instances as ours.

**One field in `predictions.jsonl` was changed for publication.** `model_name_or_path` reads `oxcode` instead of an internal configuration label. Every `model_patch` is byte-identical to what was graded. The SHA-256 of all 500 patches concatenated in file order is:

```
38c63d993cdd1d92b24a77f1a433ebeec32c4846117632da670bf9819e018bce
```

---

## How it was run

- **The agent:** the OxCode CLI, a development build from late September 2026, with OxCode's own model routing. The steps that write and run code used one model at high reasoning effort; planning and checking used the router.
- **Where it ran:** inside each instance's own evaluation image, on the repository at the commit before the fix. Each instance had up to 40 minutes, and 12 ran at a time.
- **What it saw:** a fixed preamble and the issue text, and nothing else. The gold patch, the test patch and the list of tests that decide the score never reach the agent.
- **How it was scored:** once the run had finished, its patches went to the official harness. That harness applies each patch in a fresh container and runs the project's tests. A patch counts only if the harness says the issue is resolved. We do not score anything ourselves.

### Closed book

Before the run, a probe ran inside the same image the agent used. It confirmed, by name and by bare IP, that the agent had no route to the internet, to package indexes or to git remotes, and that the image held no commits from after the fix:

```
--- MUST ALL FAIL: the open internet, by name
    github.com                     blocked ok
    pypi.org                       blocked ok
--- MUST FAIL: a BARE IP, so this is not merely a DNS result
    1.1.1.1:443                    blocked ok
--- MUST FAIL: a git fetch, which is the actual leak route
    git ls-remote blocked ok
    commits reachable that are not ancestors of HEAD: 0

PROBE PASSED: closed book holds in the image arm B will use
```

The full output is in [`closed-book-probe.txt`](closed-book-probe.txt). The only open route was to OxCode's own model gateway.

---

## The result in detail

| | Instances |
|---|:--:|
| Resolved | **410** |
| Not resolved | 87 |
| Graded, but the harness could not run the project's tests | 5 |
| Empty patch (counted as not resolved) | 3 |
| **Total** | **500** |

For the 5 instances whose tests could not run, `report.json` says why: `no_tests_collected` or `missing_module`. They count as not resolved.

By repository:

| Repository | Instances | Resolved | Share |
|---|:--:|:--:|:--:|
| django | 231 | 195 | 84% |
| sympy | 75 | 58 | 77% |
| sphinx-doc | 44 | 32 | 73% |
| matplotlib | 34 | 28 | 82% |
| scikit-learn | 32 | 30 | 94% |
| astropy | 22 | 18 | 82% |
| pydata (xarray) | 22 | 18 | 82% |
| pytest-dev | 19 | 16 | 84% |
| pylint-dev | 10 | 6 | 60% |
| psf (requests) | 8 | 7 | 88% |
| mwaskom (seaborn) | 2 | 1 | 50% |
| pallets (flask) | 1 | 1 | 100% |

Every number in this table can be recomputed from `predictions.jsonl` and `report.json`.

### Cost, per instance (averages)

| Model calls | Prompt tokens | Cached share of prompt | Output tokens |
|:--:|:--:|:--:|:--:|
| 55 | 1.9 million | 58% | 36 thousand |

The run took 5 hours 50 minutes, 12 instances at a time.

---

## What this result does not include

- **The benchmark's answers.** The gold patches and test patches stay where SWE-bench publishes them, and are not copied here.
- **Agent transcripts.** They contain the full problem statements and files from the instance repositories.
- **A model name.** The agent is OxCode as a whole. We do not publish which provider serves which step.

If something here does not add up, open an issue. The files above are what we would point to.
