# Lab 5: Static Code Analysis

**Name:** Neha
**SRN:** PES1UG24CS581
**Program analysed:** Personal Expense Tracker (`expense_tracker.py`)
**Forked repo:** https://github.com/nehaks28/L5_32_expense_tracker

## Tools and Environment

| Tool | Version | Purpose |
|------|---------|---------|
| Pylint | 4.1.3 | Code quality and logic issues |
| Bandit | 1.9.4 | Security vulnerabilities |
| Flake8 | 7.4.1 | PEP 8 style and formatting |

Python 3.14.6, run inside a virtual environment (`venv`) on macOS.

## Results

| Tool | Before | After |
|------|--------|-------|
| Pylint | 5.27/10 | 10.00/10 |
| Bandit | 1 High, 2 Medium, 1 Low | 0 issues |
| Flake8 | 4 issues | 0 issues |

The program output is identical before and after the fixes, so behaviour was preserved.

## Issues Fixed

### Bandit (security)
- **B605 (High):** `os.system("xdg-open " + path)` allowed command injection. Replaced with `subprocess.run(["xdg-open", path], check=True)`.
- **B306 (Medium):** `tempfile.mktemp` has a race condition. Replaced with `tempfile.mkstemp`.
- **B108 (Medium):** hardcoded `/tmp` path. Replaced with `tempfile.gettempdir()`.
- **B105 (Low):** hardcoded password in source. Now read from the `BANK_SYNC_PASSWORD` environment variable and compared with `hmac.compare_digest`.

### Pylint (quality)
- Added the module docstring and function docstrings.
- Renamed camelCase functions, arguments and variables to snake_case.
- Removed the `global` statement by keeping the budget in a `settings` dict.
- Replaced the broad `except Exception` with `max(..., default=None)`.
- Replaced `range(len(...))` with `enumerate`.
- Added an `if __name__ == "__main__":` guard so importing the file doesn't run the demo.

### Flake8 (formatting)
- E203 whitespace before `:`
- E302 expected 2 blank lines
- E261 two spaces before inline comment
- E501 line too long (wrapped the Summary print)

## Note on `# nosec`

After switching to `subprocess.run` with a fixed argument list and no shell, Bandit still raises informational Low notes (B404, B603, B607). The injection risk is gone, so these were suppressed with specific `# nosec` codes instead of removing the feature.

## Commit History

| Commit | Message |
|--------|---------|
| 3641564 | Add baseline static analysis reports |
| 2e3ff53 | Fix Bandit issues (B105, B108, B306, B605) |
| 4d623aa | Fix Pylint issues (docstrings, naming, global, broad except, enumerate, main guard) |
| 18af269 | Fix Flake8 formatting issues (E203, E302, E261, E501) |
| 45aa5ef | Add after reports, issues table and reflection |

## Files

- `expense_tracker.py`: cleaned version
- `issues.md`: issue table (issue, type, lines, description, fix approach)
- `reflection.md`: reflection answers
- `pylint_before.txt`, `bandit_before.txt`, `flake8_before.txt`: reports before fixes
- `pylint_report.txt`, `bandit_report.txt`, `flake8_report.txt`: reports after fixes

## How to Reproduce

Create and activate a venv, run `pip install pylint bandit flake8`, then run `python expense_tracker.py`, `pylint expense_tracker.py`, `bandit -r expense_tracker.py` and `flake8 expense_tracker.py`.
