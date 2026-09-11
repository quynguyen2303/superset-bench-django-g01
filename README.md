# superset-bench-django-g01

Benchmark workspace for **SWE-bench Verified Django group `g01`** — Jira
[AI-5326](https://qode.atlassian.net/browse/AI-5326).

11 problems, Django 3.0, anchored at `d4df5e1b0b1c643fe0fc521add0236764ec8e92a`.

This is the **Superset.sh control arm**. Two arms have already been graded on this exact
problem set — FleetControl (10/11 resolved, 516/516 PASS_TO_PASS) and Conductor (9/11
resolved, 4 PASS_TO_PASS regressions). A third number is only comparable if its inputs are
provably identical to theirs, so this workspace is a deliberate mirror of the Conductor
arm's workspace: same anchor, same manifest, same 11 problem statements byte-for-byte.
Nothing here should be "improved".

The machine-readable group definition lives in [`GROUP.json`](GROUP.json); it is the
authoritative description of this workspace, not the repository name.

## What `django/` is

`django/` is vendored third-party source: the Django working tree exactly as it stood at
the anchor commit, copied in as plain files. It carries Django's own `LICENSE`,
`LICENSE.python` and `AUTHORS` unchanged, and remains under the Django Software
Foundation's BSD-3-Clause licence. It is not a submodule and carries no git history of
its own — history is deliberately truncated to a single commit's worth of files, because
the real upstream fixes to these problems live in later Django commits and must not be
readable from this workspace.

## The anchor

The anchor is **the commit before any of the 11 fixes landed**. It is the *oldest* base
commit in the group — **not** a base that the problems share. All 11 problems have
distinct base commits, spanning 84 commits of Django history.

This matters, and it is the most expensive lesson this group has already paid for. An
upstream fix always lands *after* its own problem's base commit, so only the oldest base
commit leaves every member of the group genuinely unsolved. The first attempt at this
group was anchored at `8180ffba21bf`, the group's *newest* base commit, and 9 of these 11
problems already contained their own real upstream fix in that tree. That run scored 1/11
and measured almost nothing. The anchor is the one thing that must not be got wrong again.

Do not substitute a different commit, "update to something more recent", or fetch any
later ref into `django/`.

All 11 problems were verified unsolved at this anchor, with every official patch applying
exactly: zero leaked, zero drifted. Because all 11 share one `environment_setup_commit`
(`419a78300f7cd27611196e1e464d50fd0385ff27`), a single environment build serves the whole
group.

## The problem statements

[`problems/`](problems) holds the 11 statements, one file per instance id. Each is the
verbatim SWE-bench Verified `problem_statement` for that instance and nothing else — the
hidden test names, the FAIL_TO_PASS / PASS_TO_PASS lists, the grading modules and the
gold-patch file lists are stripped out. They are byte-identical to the set the Conductor
arm was handed. Do not reformat, re-wrap or regenerate them.

## How this arm is driven

Superset.sh is given **one prompt covering all 11 problems, unattended**, mirroring how
the Conductor arm was run. There is deliberately no per-ticket branch convention here: an
instruction demanding one branch per ticket would be a false instruction for this arm, and
the Conductor run ignored exactly that instruction with nothing forcing it to.

Whatever branch and commit shape the agent produces is part of what is being measured.
Per-problem diffs are reconstructed at grading time from the resulting tree.

## Out of scope for this repo

The benchmark runner, grading harness, CI configuration and the annotated problem metadata
are deliberately **not** here — they live in the separate `fc-django-eval` repo, because
they carry the hidden tests and must never be readable from a workspace an agent under
test can see.

This repo also has no CI workflow, by design; pull requests against it will show no checks.
