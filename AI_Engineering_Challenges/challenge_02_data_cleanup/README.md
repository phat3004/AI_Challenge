# Challenge 02 — Claims Data Cleanup & Report

This project contains Python scripts to programmatically generate a messy insurance claims dataset and perform cleanup and analytics, outputting a cleaned dataset and a comprehensive data quality report.

## Files

- `generate_data.py`: Programmatically generates 500 records in `dirty_claims.csv` with simulated data quality issues.
- `clean_data.py`: Processes the dirty CSV file, executes data validation rules, normalizes fields, drops invalid entries, and saves the cleaned dataset as `clean_claims.csv` while printing and writing a report to `report.md`.
- `dirty_claims.csv`: The simulated raw claims dataset.
- `clean_claims.csv`: The normalized and validated claims dataset.
- `report.md`: The generated markdown data quality report containing cleaning details and statistics.

## Quality Issues Handled

1. **Exact Duplicate Rows**: Identifies and drops exact duplicate records.
2. **Duplicate `claim_id`**: Detects and tracks instances of identical claim IDs with differing claims data.
3. **Missing `claim_id`**: Removes claims that have no ID.
4. **Missing `policy_id`**: Identifies missing policy IDs and normalizes them to empty values.
5. **Inconsistent Casing**: Normalizes patient names to Title Case (e.g. `john doe` / `JOHN DOE` -> `John Doe`).
6. **Claim Type Typos**: Normalizes variant values like `Outpateint` or `OP` to `OUTPATIENT`, `IP` to `INPATIENT`, etc.
7. **Null/Empty Diagnosis**: Replaces strings like `"N/A"`, `"n/a"`, or `"none"` with consistent null values (empty cell).
8. **Invalid amounts**: Normalizes numbers represented as string with commas (e.g. `"15,000"` -> `15000.0`) and flags/removes invalid amounts (negative values, zero).
9. **Currency Standardization**: Normalizes variant currency strings like `Baht` or `thb` to `THB`, and `vnd` to `VND`.
10. **Date Normalization**: Parses differing date formats (e.g., `YYYY-MM-DD`, `DD/MM/YYYY`, and text formats like `March 15, 2024`) and normalizes all to ISO 8601 format (`YYYY-MM-DD`).

## How to Run

No third-party packages are required. The project runs purely on Python 3 standard libraries.

### 1. Generate the Messy Data
Run the generator to create `dirty_claims.csv`:
```bash
python3 generate_data.py
```

### 2. Clean the Data and Generate the Report
Run the cleaner script to process the data and output the clean CSV and report:
```bash
python3 clean_data.py
```

Check `report.md` for the summary report.
