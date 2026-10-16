# Lab 5: Static Code Analysis - Expense Tracker

Author: Neha (PES1UG24CS581)

Fixes for the Lab 5 static analysis findings on `expense_tracker.py`, one commit per tool:

1. **Bandit:** replaced `os.system` with `subprocess.run` and an argument list (command injection), replaced `tempfile.mktemp` with `mkstemp`, removed the hardcoded `/tmp` path, and moved the hardcoded password into the `BANK_SYNC_PASSWORD` environment variable.
2. **Pylint:** added docstrings, renamed camelCase names to snake_case, removed the `global` statement, replaced the broad `except Exception` with `max(..., default=None)`, used `enumerate`, and added an `if __name__ == "__main__":` guard.
3. **Flake8:** fixed E203, E302, E261 and E501 formatting issues.

Results: Pylint 5.27 to 10.00/10, Bandit 1 High / 2 Medium / 1 Low to 0 issues, Flake8 4 issues to 0. The program output is unchanged.

The before/after tool reports and the issue table are in the Lab 5 folder of the course repo.
