# Project agent memory

## Workspace context

SWE-bench Verified benchmark workspace for Django group `g01` (Jira AI-5326): 11 problems
against Django 3.0. This is the **Superset.sh control arm**, a deliberate mirror of the
Conductor arm's workspace so the three arms' scores are comparable. `django/` is vendored
Django source at the anchor commit, with no git history of its own.

- [`GROUP.json`](GROUP.json) is the authoritative group definition — anchor, the 11
  instance ids and their base commits, and the verification record.
- [`README.md`](README.md) explains why the anchor is the group's *oldest* base commit and
  why any newer commit contaminates the set.
- [`problems/`](problems) holds the 11 verbatim SWE-bench statements, stripped of hidden
  test names, F2P/P2P lists, grading modules and gold-patch file lists.

**The anchor is immutable.** Never update `django/`, fetch a later ref into it, or
re-vendor it at a different commit — later Django commits contain the fixes to these 11
problems.

**No per-ticket branch convention.** This arm is driven as one prompt for all 11 problems,
unattended; branch and commit shape are the agent's own choice and part of what is
measured. Do not add a one-branch-per-ticket rule here — it would not match how this arm
is run.

## Running tests

All test commands run from `django/tests/`, SQLite by default, no extra setup:

```bash
cd django/tests

# First-time setup
python -m pip install -e ..
python -m pip install -r requirements/py3.txt

# Run a full test module, then a single class or method
python runtests.py template_tests.test_engine
python runtests.py template_tests.test_engine.RenderToStringTest.test_autoescape_off
```

## Linting

From `django/`: `flake8 .` and `isort --check-only --diff django tests scripts`.
Style rules (line length, indent, isort profile) live in `django/setup.cfg` and
`django/tox.ini` — read those rather than assuming.

## Architecture

`django/django/` is the framework source and `django/tests/` mirrors its layout — a test
module `tests/<name>/` exercises the correspondingly named package. `tests/runtests.py`
builds a minimal Django environment and discovers modules by scanning `tests/` for
directories with an `__init__.py`.

## Out of scope

The runner, grading harness and annotated problem metadata live in the separate
`fc-django-eval` repo; they carry the hidden tests and must not be readable from here.
This repo has no CI workflow by design, so pull requests show no checks.

## Maintaining this file

Keep this file for knowledge useful to almost every future agent session in this project.
Do not repeat what the codebase already shows; point to the authoritative file or command instead.
Prefer rewriting or pruning existing entries over appending new ones.
When updating this file, preserve this bar for all agents and keep entries concise.
