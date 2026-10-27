# Perturbation Log

## 1. Insurance Policy Extraction Pipeline

**Experiment:** Test validation and retry behavior, including a null required field.

**Command:**
`.venv/bin/pytest tests/test_us01_retry.py -v`

**Evidence:** `insurance/06-retry-perturbation.txt`

**Observed result:** 13 passed, 1 skipped. Tests cover format and consistency failures that trigger retries, missing-source behavior that halts immediately, and retry metadata. The live extraction test was skipped.

**Comparison:** A well-formed response is accepted; a format or consistency failure can trigger a retry, while missing required source information halts immediately. This experiment uses the provided tests rather than modifying a policy document.

## 2. Mortgage Document Extraction System

**Experiment:** Compare clean square-footage normalization with an income-total mismatch.

**Commands:**
- `.venv/bin/mortgage-extract fixtures/documents/appraisal_informal_sqft.txt --mode replay`
- `.venv/bin/mortgage-extract fixtures/documents/income_sum_mismatch.txt --mode replay`

**Evidence:**
- `mortgage/05-appraisal-normalization-run.txt`
- `mortgage/06-income-mismatch-run.txt`

**Observed result:** The appraisal's gross living area is normalized to integer 2400 and validation is consistent. For the income document, the calculated total is 9642.17 while the stated total is 10892.17; the validator reports a delta of -1250.00 and marks the result inconsistent.

**Comparison:** The appraisal is accepted without discrepancies, while the income total triggers a discrepancy. This is a comparison of two provided fixtures, not a modification of either input.

## 3. Supply Chain Risk Investigation

**Experiment:** Simulate a logistics-source timeout.

**Commands:**
- `.venv/bin/supply-chain-investigate meridian --offline`
- `.venv/bin/supply-chain-investigate meridian --offline --simulate-timeout`

**Evidence:**
- `supply-chain/report-normal.md`
- `supply-chain/report-timeout.md`
- `supply-chain/report-normal.log`
- `supply-chain/report-timeout.log`

**Observed result:** The normal run generates the briefing. The timeout run completes with the logistics source marked unavailable and missing information reflected in the Incomplete section.

**Comparison:** The normal run can use logistics data; the simulated-timeout run preserves partial results and reports the source failure rather than aborting. This changes the execution mode, not the source files.
