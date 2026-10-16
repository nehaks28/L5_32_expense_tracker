# Issues found in expense_tracker.py (Lab 5)

Line numbers refer to the ORIGINAL file (see `*_before.txt`). Sorted by severity.

| Issue | Type | Line(s) | Description | Fix Approach |
|-------|------|---------|-------------|--------------|
| Shell command injection via `os.system` (Bandit B605, **High**) | Security | 61 | `os.system("xdg-open " + path)` builds a shell command by string concatenation, so a path like `x; rm -rf ~` would run arbitrary commands. | Replaced with `subprocess.run(["xdg-open", path], check=True)`. Argument list, no shell, and errors are raised. |
| Insecure `tempfile.mktemp` (Bandit B306, **Medium**) | Security | 55-56 | `mktemp` only returns a name and doesn't create the file, so there is a race condition: another process can create/symlink that name first. | Used `tempfile.mkstemp`, which atomically creates the file with safe permissions, and closed the handle. Also `os.makedirs(..., mode=0o700)`. |
| Hardcoded `/tmp` directory (Bandit B108, **Medium**) | Security | 8 | `REPORT_DIR = "/tmp/expense_reports"` hardcodes a shared, world-writable location (and breaks on Windows). | `os.path.join(tempfile.gettempdir(), "expense_reports")`. |
| Hardcoded password (Bandit B105, Low) | Security | 5, 50 | `BANK_SYNC_PASSWORD = "demo-sync-pass"` is in source (and in git history); `==` comparison is also not timing-safe. | Read from `os.environ["BANK_SYNC_PASSWORD"]` (empty means always reject), compare with `hmac.compare_digest`. |
| Broad exception `except Exception` (Pylint W0718) | Bug / Quality | 38-42 | Try/except around `max()` hides every possible error just to handle the empty-list case. | Removed the try/except, used `max(..., default=None)`. |
| `global` statement (Pylint W0603) | Quality | 28-30 | `setBudget` rebinds a module global, which makes state hard to track/test. | Kept budget in a `settings` dict and update `settings["monthly_budget"]` instead of rebinding. |
| `range(len(...))` loop (Pylint C0200) | Quality | 44-46 | Index-based loop is un-Pythonic and error-prone. | `for i, expense in enumerate(expenses, start=1)`. |
| Function / variable / argument names not snake_case (Pylint C0103) | Style | 7, 11, 20, 28, 33, 38, 44, 49, 53, 55, 59 | `addExpense`, `totalByCategory`, `setBudget`, `newBudget`, `remainingBudget`, `biggestExpense`, `printLedger`, `checkSyncPassword`, `exportReport`, `reportPath`, `openStatement` violate PEP 8 naming. | Renamed all to snake_case (`add_expense`, `total_by_category`, ...) and updated every call site. |
| Missing module and function docstrings (Pylint C0114 / C0116) | Documentation | 1, 11, 20, 28, 33, 38, 44, 49, 53, 59, 64 | No docstrings anywhere. | Added a module docstring and a one-line docstring per function. |
| `main()` runs on import (not a Pylint message, found in review) | Bug / Design | last line | `main()` is called at module level, so importing the file runs the demo. | Added `if __name__ == "__main__":` guard. |
| E203 whitespace before ':' (Flake8) | Formatting | 15 | `"amount" : amount` | Removed the space: `"amount": amount`. |
| E302 expected 2 blank lines (Flake8) | Formatting | 44 | Only 1 blank line before `printLedger`. | Added the second blank line. |
| E261 two spaces before inline comment (Flake8) | Formatting | 74 | `...)# after setBudget` with one space. | Two spaces before `#`. |
| E501 line too long, 93 > 79 (Flake8) | Formatting | 77 | The final "Summary" `print` is too long. | Wrapped the call over two lines. |

## Results

| Tool | Before | After |
|------|--------|-------|
| Pylint | 5.27/10 | 10.00/10 |
| Bandit | 1 High, 2 Medium, 1 Low | 0 issues |
| Flake8 | 4 issues | 0 issues |

Program output is identical before and after the changes.
