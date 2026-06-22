# Challenge 10 — Fraud Detection Scoring Engine

This project implements an advanced, rule-based fraud detection engine that evaluates medical claims and assigns a composite risk score (0–100) with detailed itemized evidence. It features simulated dataset generation containing exactly 210 embedded fraud cases representing 8 distinct fraud patterns and reaches a **100% recall** with a **4.58% False Positive Rate (FPR)**.

## Detection Rules Implemented

1. **Duplicate Claim** (Weight 5): Same member + provider + date + diagnosis + amount.
2. **Rapid Re-submission** (Weight 4): Same member + same diagnosis within 7 days.
3. **Upcoding** (Weight 3): Claim amount > 2 standard deviations above the statistical mean for that procedure code across the dataset.
4. **Unbundling** (Weight 3): Individual billing of components that should be bundled (e.g. billing Scaling, Polishing, and Flouride separately instead of the Complete Dental Clean Bundle). Supports 5 specific bundle definitions.
5. **Phantom Billing** (Weight 5): Provider submitting > 30 claims in a single day.
6. **Weekend Anomaly** (Weight 4): Surgical procedures (Appendix/Cataract) on weekends by a provider whose historical weekend volume (excluding surgical claims) is < 5%.
7. **Diagnosis-Procedure Mismatch** (Weight 3): Procedure performed that has no clinical association with the diagnosis (e.g. cataract surgery for a common cold). Supports 10 clinical mappings.
8. **Amount Clustering** (Weight 2): Claims sitting in the 5% buffer directly below the 50,000 auto-approval threshold (47,500 – 49,999.99).

## Risk Scoring

Composite risk score is computed as:
$$\text{Composite Score} = \min\left(100, \frac{\sum \text{Severity of Triggered Rules}}{10.0} \times 100\right)$$
Claims with a risk score of 20+ (at least one rule triggered) are flagged as suspicious.

## How to Run

Runs using Python 3 standard library with zero external dependencies.

### 1. Run Data Generation and Detection Pipeline
Execute the full process:
```bash
python3 run.py
```
This script will:
- Generate `claims.csv` containing 2,000 claims (approx. 10% fraud).
- Analyze the claims using `detector.py`.
- Save results to `scored_claims.json`.
- Output a comprehensive metrics report (Precision, Recall, FPR, Recall by pattern, and Top 10 High Risk claims).

### 2. Run the Unit Tests
Verify all detection rules and scoring edge cases:
```bash
python3 -m unittest test_detector.py
```
All unit tests should pass.
