# Audit Trail

**Version:** 1.0
**Date:** 2026-01-21
**Status:** ACTIVE

---

## Executive Summary

This document records all methodological decisions, data changes, and validations performed during the development of AMPAY.

---

## 1. Methodological Decisions

### 1.1 Quiz Algorithm Selection

| Date | Decision | Alternatives Considered | Justification |
|------|----------|------------------------|---------------|
| 2026-01-19 | Manhattan distance | Euclidean, Cosine | Standard in VAAs, intuitive interpretation |
| 2026-01-19 | 3-point scale | 5-point Likert | Simplicity for a short quiz |
| 2026-01-19 | No weighting | Weighting by importance | Avoid bias in topic selection |

### 1.2 AMPAY Criteria

| Date | Decision | Value | Justification |
|------|----------|-------|---------------|
| 2026-01-20 | AMPAY threshold | >= 60% | Balance between precision and recall |
| 2026-01-20 | Potential threshold | 40-59% | Gray zone for review |
| 2026-01-20 | Minimum number of laws | >= 3 | Avoid conclusions based on a single data point |
| 2026-01-21 | Confidence system | HIGH/MEDIUM/LOW | Transparency in certainty |

### 1.3 Categorization

| Date | Decision | Value | Justification |
|------|----------|-------|---------------|
| 2026-01-18 | Number of categories | 13 | Balance between granularity and usability |
| 2026-01-18 | Exclude digitization | N/A | It is cross-cutting, not a category |
| 2026-01-18 | One category per vote | N/A | Avoid double counting |

### 1.4 Position Sources

| Date | Decision | Alternatives | Justification |
|------|----------|--------------|---------------|
| 2026-01-19 | 2026 campaign promises only | Promises + votes | Quiz is for 2026; AMPAY covers votes |
| 2026-01-19 | Silence = 0 | Infer position | Consistent with MARPOR |

---

## 2. Data Changes

### 2.1 Voting Dataset

| Date | Event | Details |
|------|-------|---------|
| 2026-01-15 | Initial download | openpolitica, 289K votes |
| 2026-01-16 | Declaratory votes filtered out | 497 votes excluded |
| 2026-01-16 | Procedural votes filtered out | 847 votes excluded |
| 2026-01-16 | Final dataset | 2,226 substantive votes |

### 2.2 Extracted Promises

| Date | Party | Event | Details |
|------|-------|-------|---------|
| 2026-01-15 | All | Initial extraction | 372 raw promises |
| 2026-01-16 | All | Validation | 345 valid promises |
| 2026-01-17 | Fuerza Popular | Correction | Promise FP-2021-023 reclassified |

### 2.3 AMPAYs

| Date | AMPAY ID | Event | Details |
|------|----------|-------|---------|
| 2026-01-20 | AMPAY-001 | Detected | Fuerza Popular tax universality |
| 2026-01-20 | AMPAY-001 | Validated | 6 laws, 100% YES, HIGH |
| 2026-01-20 | AMPAY-002 | Detected | Renovacion Popular tax regimes |
| 2026-01-20 | AMPAY-002 | Validated | 5 laws, 100% YES, HIGH |
| 2026-01-21 | AMPAY-003 | Detected | Renovacion Popular exports |
| 2026-01-21 | AMPAY-003 | Validated | HIGH |
| 2026-01-21 | (various) | Rejected | 15 false positives discarded |

---

## 3. Validations Performed

### 3.1 Data Quality

| Date | Validation | Result | Action |
|------|------------|--------|--------|
| 2026-01-16 | Voting completeness | 99.7% | Accepted |
| 2026-01-16 | Categorization accuracy | 94.8% | Accepted |
| 2026-01-17 | Promise sources | 100% verifiable | Accepted |

### 3.2 AMPAYs

| Date | AMPAY | Reviewer | Result |
|------|-------|----------|--------|
| 2026-01-20 | AMPAY-001 | JD | Approved (HIGH) |
| 2026-01-20 | AMPAY-002 | JD | Approved (HIGH) |
| 2026-01-21 | AMPAY-003 | JD | Approved (HIGH) |
| 2026-01-21 | AMPAY-004 | JD | Approved (HIGH) |
| 2026-01-21 | AMPAY-005 | JD | Approved (MEDIUM) |
| 2026-01-21 | AMPAY-006 | JD | Approved (MEDIUM) |
| 2026-01-21 | AMPAY-007 | JD | Approved (MEDIUM) |
| 2026-01-21 | AMPAY-008 | JD | Approved (MEDIUM) |

### 3.3 Rejected False Positives

| Date | Candidate | Reason for Rejection |
|------|-----------|----------------------|
| 2026-01-20 | AC-FP-001 | Weak semantic connection |
| 2026-01-20 | AC-FP-002 | Only 1 vote (insufficient pattern) |
| 2026-01-20 | AC-PL-001 | Exculpatory context (Amazonia) |
| 2026-01-21 | (various) | See ampay_review_log.json |

---

## 4. Methodology Changes

### 4.1 Version Evolution

| Date | Version | Change | Reason |
|------|---------|--------|--------|
| 2026-01-18 | v1 → v2 | Aggregation by category | v1 had too many false positives |
| 2026-01-19 | v2 → v3 | Keyword-based search | v2 lost specificity |
| 2026-01-20 | v3 → v4 | Added inverse search | v3 only detected type A |
| 2026-01-21 | v4 → v5 | Confidence system | v4 did not distinguish certainty |

### 4.2 Files Affected by Changes

| Change | Regenerated Files |
|--------|-------------------|
| v1 → v2 | None (methodology rejected) |
| v2 → v3 | ampays.json |
| v3 → v4 | ampays.json |
| v4 → v5 | ampays.json, documentation |

---

## 5. Incidents and Corrections

### 5.1 Detected Errors

| Date | Error | Severity | Correction |
|------|-------|----------|------------|
| 2026-01-17 | Duplicate promise | Low | Removed FP-2021-023b |
| 2026-01-19 | Incorrect category | Medium | 12 votes reclassified |
| 2026-01-20 | False positive AMPAY published | High | Withdrawn before QA |

### 5.2 Known Data Gaps

| Gap | Identified | Status |
|-----|------------|--------|
| Votes from 2024-07 to 2026-01 | 2026-01-15 | Documented, no resolution |
| Secret votes | 2026-01-15 | Inherent limitation |
| Parties without a detailed 2026 plan | 2026-01-18 | Documented |

---

## 6. Quality Reviews

### 6.1 Voting Sample

| Date | Sample | Accuracy |
|------|--------|----------|
| 2026-01-16 | 200 votes | 94.8% |
| 2026-01-17 | 50 borderline votes | 91.0% |

### 6.2 Promise Sample

| Date | Sample | Accuracy |
|------|--------|----------|
| 2026-01-17 | 100 promises | 91.7% |

### 6.3 Source Verification

| Date | Verification | Result |
|------|--------------|--------|
| 2026-01-21 | Bibliography URLs | 100% functional |
| 2026-01-21 | Academic DOIs | 100% resolved |
| 2026-01-21 | JNE PDFs | 100% accessible |

---

## 7. Responsible Parties

| Role | Person | Tasks |
|------|--------|-------|
| Project lead | JD | Methodological decisions, validation |
| Developer | JD | Implementation, data |
| AMPAY reviewer | JD | Manual validation of AMPAYs |

---

## 8. Next Steps

| Target Date | Task | Status |
|-------------|------|--------|
| 2026-01-22 | Soft launch | Pending |
| 2026-01-25 | Initial feedback | Pending |
| 2026-02-01 | Post-launch bug fixes | Pending |
| 2026-Q2 | Data update 2024-2026 | Planned |

---

## 9. Version Control

### 9.1 Versioned Files

| File | Current Version | Last Modified |
|------|-----------------|---------------|
| quiz_statements.json | 2.1 | 2026-01-21 |
| ampays.json | 1.0 | 2026-01-21 |
| votes_categorized.json | 1.0 | 2026-01-21 |
| party_patterns.json | 1.0 | 2026-01-21 |

### 9.2 Git History

```
Relevant commits:
- 2026-01-15: Initial data import
- 2026-01-18: Methodology v1-v2
- 2026-01-20: Methodology v3-v4, first AMPAYs
- 2026-01-21: Methodology v5, documentation complete
```

---

*Last updated: 2026-01-21*
