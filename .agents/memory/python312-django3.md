---
name: Python 3.12 and Django 3.0 tests
description: Environment limitations encountered when running this vendored Django snapshot under the current Python runtime.
---

The vendored Django 3.0 test runner is not fully compatible with Python 3.12: it imports removed `distutils`, and parallel execution expects the newer `unittest` `addDuration()` API. Focused suites can still run serially with a temporary compatibility shim.

**Why:** The benchmark intentionally preserves an old Django anchor, so upgrading Django or changing the vendored source just to modernize the runner would contaminate the benchmark.

**How to apply:** Use the project’s intended older Python environment for full validation; under Python 3.12, run focused suites serially and treat the parallel-runner failure as environment compatibility rather than a benchmark regression.