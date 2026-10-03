# Lab 1 - GitHub Actions (Testing with Pytest & Unittest)

Based on [Github_Labs/Lab1](https://github.com/raminmohammadi/MLOps/tree/main/Github_Labs/Lab1) from the course repo. The original lab instructions are kept in [instructions.md](instructions.md).

## Overview
<!-- 1-2 lines: what this lab demonstrates -->

## Changes from the reference
<!-- What you added or modified beyond the reference solution -->
- 

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
Workflows live at the repo root in `.github/workflows/` (GitHub only runs workflows from there).

## Running Locally
```
python -m venv lab_01
lab_01\Scripts\activate
pip install -r requirements.txt
pytest
python -m unittest test.test_unittest
```

## CI Results
<!-- Screenshot or status badges of passing GitHub Actions runs -->
