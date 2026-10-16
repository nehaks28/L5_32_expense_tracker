## Submission

**Forked repo:** https://github.com/nehaks28/L5_32_expense_tracker

- **Name:** Neha
- **SRN:** PES1UG24CS581
- **Lab:** Lab 5, Static Code Analysis

### Results

| Tool | Before | After |
|------|--------|-------|
| Pylint | 5.27/10 | 10.00/10 |
| Bandit | 1 High, 2 Medium, 1 Low | 0 issues |
| Flake8 | 4 issues | 0 issues |

### Files
- `expense_tracker.py`: cleaned version
- `issues.md`: issue table
- `reflection.md`: reflection answers
- `*_before.txt` / `*_report.txt`: static analysis reports before and after

# Lab 5: Static Code Analysis — Personal Expense Tracker

**Your file:** `expense_tracker.py`

Follow the lab handout for the full instructions.

## About the program

A monthly expense tracker. It records expenses by category, rejects invalid amounts, totals spending per category, tracks the remaining budget, finds the biggest expense and checks a bank-sync password.

The code works, but only just. It has quality, style and security problems of the kind Pylint, Flake8 and Bandit are built to find.

## What to do

1. **Run it first** and read the output carefully:

   ```
   python expense_tracker.py
   ```

   It runs a short demonstration and needs no input. Note what it prints, so you can check that it still behaves the same after your fixes.

2. **Install the tools:**

   ```
   python -m pip install pylint bandit flake8
   ```

3. **Run the analysis and save the reports:**

   ```
   python -m pylint expense_tracker.py > pylint_report.txt
   python -m bandit -r expense_tracker.py > bandit_report.txt
   python -m flake8 expense_tracker.py > flake8_report.txt
   ```

4. **Document the issues** in `issues.md` using this table (at least 4 rows, high and medium severity first):

   | Issue | Type | Line(s) | Description | Fix Approach |
   |-------|------|---------|-------------|--------------|
   | ...   | ...  | ...     | ...         | ...          |

5. **Fix the code.** Fix at least 4 of the issues you documented, then rerun all three tools to confirm they are gone. Repeat until you are satisfied. The program should still do its job when you are done.

6. **Reflect.** Answer the four reflection questions from the handout in `reflection.md`.

## Deliverables

- `expense_tracker.py`: your cleaned version, with at least 4 issues fixed.
- `issues.md`: the completed issue table.
- `reflection.md`: your answers to the reflection questions.

**Target:** aim for a Pylint score above 9.5 on your final file. A perfect 10.00/10 is very good.

Be ready to explain the bugs you found and how you fixed them. You may be called on to present.
