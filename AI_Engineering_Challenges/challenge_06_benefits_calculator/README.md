# Challenge 06 — Policy Benefits Calculator

This project implements a reusable business logic calculator module that evaluates medical claims chronologically against a customer's specific insurance policy details, including co-pays, annual limits, sub-limits, exclusions, global deductibles, and waiting periods.

## Components

- `calculator.py`: The core reusable `PolicyBenefitsCalculator` class containing all evaluation and limit-deduction algorithms.
- `policy.json`: A complete policy configuration mapping annual limits, sub-limits, copays, exclusions, and a global deductible.
- `expenses.json`: 20 chronologically ordered medical expenses exercising all business rules.
- `run.py`: Script that parses `policy.json` and `expenses.json`, executes calculations chronologically, outputs results to `output.json`, and outputs a comprehensive balance summary in the terminal.
- `test_calculator.py`: Unit tests covering all calculation paths using Python's standard `unittest` library (no external test harness installation required).
- `output.json`: The processed calculations output.

## Covered Rules

1. **Policy Duration Validation**: Denies claims dated outside policy start/end dates.
2. **Exclusion Check**: Rejects claims with diagnoses or benefits matching exclusions (e.g. self-inflicted injuries, cosmetic surgery).
3. **Waiting Periods**: Checks if the claim date occurs before the benefit-specific waiting period (e.g. 90 days for Dental, 270 days for Maternity) has elapsed.
4. **Global Deductibles**: Subtracts expenses from a global deductible first. Only after the deductible is completely met do insurance payouts begin.
5. **Sub-limits**: Applies caps per visit, per day, per event, or per pregnancy (e.g. max Room & Board daily limits or outpatient visit caps).
6. **Co-pays**: Calculates percentage co-pays (e.g. 20% outpatient copay) and applies optional caps per visit (e.g. max 500 THB copay per visit).
7. **Benefit Limits**: Enforces and decrements both overall benefit annual limits (e.g. 100,000 THB Outpatient limit) and sub-benefit annual limits.
8. **Visits/Days Limits**: Enforces and decrements counts of allowed visits or days per year.

## How to Run

This project runs using Python 3 standard library with zero external dependencies.

### 1. Run the Claims Simulation
Execute the main simulator to process the 20 test expenses and print the breakdown:
```bash
python3 run.py
```
This prints a clean table to the terminal and outputs the results to `output.json`.

### 2. Run the Unit Test Suite
Execute the unit tests to verify the core calculator logic:
```bash
python3 -m unittest test_calculator.py
```
All tests should pass.
