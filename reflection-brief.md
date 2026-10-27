# Capstone Reflection Brief

## 1. Insurance Policy Extraction Pipeline

**Evidence:** `insurance/01-policy-pipeline-tests.txt`, `insurance/05-offline-routing-tests.txt`, `insurance/06-retry-perturbation.txt`, and `insurance/04-calibration-report.txt`.

The main test suite recorded 45 passing tests and 3 skipped tests. The offline routing suite recorded 9 passing tests. The retry suite recorded 13 passing tests and 1 skipped live test. Mypy and Ruff both passed.

The routing tests demonstrate that low confidence, integration failures, and reviewer disagreement can lead to human review. The calibration experiment shows why subgroup metrics matter: the umbrella/exclusions slice had confidence 0.93, accuracy 0.00, and Brier score 0.865, while the overall Brier score was 0.291. The aggregate score alone would not clearly communicate this subgroup's weakness.

**Limitation:** A full pipeline run and `routing_decisions.json` have not been captured. Therefore, a specific policy ID and independent signal from the actual routing output cannot yet be cited.

## 2. Mortgage Document Extraction System

**Evidence:** `mortgage/04-missing-bonus-run.txt`, `mortgage/05-appraisal-normalization-run.txt`, and `mortgage/06-income-mismatch-run.txt`.

The missing-bonus run returned `bonus_monthly: null` rather than inventing a value. The appraisal run normalized the informal square-footage value to integer 2400 and returned `consistent: true`.

The income mismatch run calculated 9642.17 while the stated total was 10892.17. It reported a delta of -1250.00 and marked the extraction inconsistent. This demonstrates the value of validating arithmetic rather than trusting a structured extraction without checking it.

The comparison uses supplied fixtures rather than a deliberately edited input. A separate input-perturbation experiment would strengthen this section.

## 3. Supply Chain Risk Investigation

**Evidence:** `supply-chain/report-normal.md`, `supply-chain/report-timeout.md`, `supply-chain/report-normal.log`, and `supply-chain/report-timeout.log`.

The normal investigation generated a briefing. The simulated-timeout run completed despite a logistics-source failure and retained partial results, marking unavailable information in the Incomplete section.

The normal and timeout reports demonstrate that source failure should reduce coverage transparently rather than silently fabricate information or abort the entire investigation. The experiment changes the execution mode rather than modifying source data.

The briefing also preserves conflicting on-time-delivery claims, approximately 95% and 78%, instead of collapsing them into a single unsupported value. Keeping both values with provenance allows a reviewer to investigate the discrepancy.

## 4. Reliability Principles Across the Three Systems

All three systems demonstrate the importance of making uncertainty and failure visible.

- The Insurance pipeline uses validation, retry rules, routing, and calibration slices to identify outputs needing additional scrutiny.
- The Mortgage system checks structured extraction against arithmetic constraints and preserves missing values as null.
- The Supply Chain system preserves source attribution and partial results when sources conflict or become unavailable.

The reliability principle I would apply in my own work is: **validate outputs, preserve provenance, and escalate uncertainty instead of hiding it behind a confident-looking result.** Automated systems should make it easy for a reviewer to understand what was observed, what failed, and what still needs investigation.

## 5. Verification Summary

- Insurance: 45 passed, 3 skipped in the full suite; 9 routing tests passed; 13 retry tests passed and 1 skipped.
- Mortgage: 25 tests passed.
- Supply Chain: 34 tests passed.
- Mypy and Ruff passed for all three systems.
- Environment: Python 3.13.0 on Linux, recorded in `environment.txt`.

## 6. Remaining Submission Checks

1. Capture the full Insurance pipeline and `routing_decisions.json` if API access is available; otherwise retain the documented offline fallback and its limitations.
2. Verify the Mortgage README's reference to 28 acceptance tests against the 25 tests discovered.
3. Capture at least one screenshot per system if required by the submission instructions.
4. Confirm the final submission folder matches the starter README layout.
