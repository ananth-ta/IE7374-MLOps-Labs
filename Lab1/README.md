# Lab 1 - GitHub Actions (Testing with Pytest & Unittest)

Based on [Github_Labs/Lab1](https://github.com/raminmohammadi/MLOps/tree/main/Github_Labs/Lab1) from the course repo. The original lab instructions are kept in [instructions.md](instructions.md).

## Overview
A small calculator module tested with both **pytest** and **unittest**, with two GitHub Actions workflows that run each test suite automatically on every push or pull request to `main` that touches this lab.

## Changes from the reference

### Calculator (`src/calculator.py`)
- **Descriptive names:** `fun1`–`fun4` renamed to `add`, `subtract`, `multiply`, `add_three`.
- **New functions:** `divide`, `power`, `modulo`, `average`.
- **Shared validation:** the input check repeated in every function is replaced by two helpers: `is_num` (checks one value) and `val_num` (raises `ValueError` if any input is invalid).
- **Stricter input checks:**
  - Booleans are rejected. In the reference, `fun1(True, 2)` returned `3` because `bool` is a subclass of `int` in Python.
  - NaN and ±infinity are rejected.
  - `add_three` (the reference `fun4`) had no input validation at all; it now does.
- **Edge cases handled:**
  - `divide` / `modulo` by zero (including `0.0` and `-0.0`)
  - `power(0, -1)` (zero to a negative power)
  - `power(-8, 1/3)`, which in plain Python silently returns a **complex number**
  - `power(10.0, 400)` overflow
- **One exception type:** every invalid operation raises `ValueError` with a descriptive message, instead of a mix of `ValueError`, `ZeroDivisionError` and `OverflowError`.

### Tests
- **pytest (`test/test_pytest.py`):**
  - `@pytest.mark.parametrize` replaces the repeated `assert` lines.
  - Stacked `parametrize` decorators test every function against every invalid input, in every argument position.
  - `pytest.raises(..., match=...)` checks both the exception type and its message.
  - `pytest.approx` compares float results.
- **unittest (`test/test_unittest.py`):**
  - Covers the same cases, split into one `TestCase` class per area.
  - `subTest` plays the role of `parametrize`, and `setUp` shares the invalid-input data.
  - `assertRaisesRegex` checks exception messages; `assertAlmostEqual` compares floats.
- **Verified the tests catch bugs:** changing `add` to return `x - y` made both suites fail; reverting it made them pass again.

### CI (`.github/workflows/`)
The reference workflows would not run as-is, so they were fixed:
- **Location:** moved to the repo root, since GitHub ignores workflows in subfolders like `Lab1/workflows/`.
- **Triggers:** run only when `Lab1/` changes (`paths` filter) and from the `Lab1/` directory (`working-directory`), so other labs in this repo don't affect them.
- **Versions:** upgraded to `actions/checkout@v4`, `actions/setup-python@v5` and `actions/upload-artifact@v4` (v2 of upload-artifact has been retired and fails), and to Python 3.13 (3.8 is end-of-life).
- **Invalid YAML removed:** the `run-nam` typo, a conflicting `branches` / `branches-ignore` pair, and the `label` / `issues` triggers.
- **Manual runs:** added `workflow_dispatch` and `pull_request` triggers.

## Project Structure
```
Lab1/
├── data/
├── src/
│   └── calculator.py
├── test/
│   ├── test_pytest.py
│   └── test_unittest.py
├── instructions.md
├── README.md
└── requirements.txt
```
Workflows live at the repo root in `.github/workflows/` (`lab1_pytest.yml`, `lab1_unittest.yml`), since GitHub only runs workflows from there.

## Running Locally
From the `Lab1/` directory:
```
python -m venv lab_01
lab_01\Scripts\activate
pip install -r requirements.txt
pytest -v
python -m unittest -v test.test_unittest
```

## CI Results
<!-- TODO after first push: add status badges / screenshot of passing GitHub Actions runs -->

## Acknowledgements
Edge-case review, refactoring and test expansion assisted by Claude Code (Anthropic).
