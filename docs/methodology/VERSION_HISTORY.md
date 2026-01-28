# Methodology Version History

**Version:** 1.0
**Date:** 2026-01-21
**Status:** ACTIVE

---

## Executive Summary

This document records the evolution of the AMPAY methodology from v1 through v5, including the problems identified in each version and the solutions implemented.

---

## 1. Version Overview

```
v1 ──────► v2 ──────► v3 ──────► v4 ──────► v5
(Failed)   (Failed)   (Improved) (Improved) (Current)

Jan 2026   Jan 2026   Jan 2026   Jan 2026   Jan 2026
```

| Version | Status | Main Problem | Solution |
|---------|--------|-------------|----------|
| v1 | DISCARDED | Vote-by-vote comparison with no pattern | Category aggregation |
| v2 | DISCARDED | Aggregation loses specificity | Keyword-based search |
| v3 | SUPERSEDED | Direct search only | Add inverse search |
| v4 | SUPERSEDED | No confidence system | Add confidence levels |
| v5 | **ACTIVE** | - | - |

---

## 2. Version 1 (Discarded)

### 2.1 Description

First approach: compare each promise with each vote individually.

### 2.2 Methodology

```
For each promise:
  For each vote:
    If vote.asunto contains promise.keywords:
      If party voted NO:
        Flag as contradiction
```

### 2.3 Identified Problems

| Problem | Example | Impact |
|---------|---------|--------|
| **No pattern** | 1 NO vote out of 50 related = AMPAY? | Massive false positives |
| **No threshold** | Any NO counted | Excessive sensitivity |
| **Literal matching** | "salud" (health) matched everything | Noise |

### 2.4 Results

```
AMPAYs detected: 200+
Valid AMPAYs: ~5
Precision: ~2.5%
```

### 2.5 Decision

**DISCARDED** -- Too many false positives, unusable.

---

## 3. Version 2 (Discarded)

### 3.1 Description

Aggregate all votes by category and calculate the percentage of NO votes.

### 3.2 Methodology

```
For each promise:
  category = promise.category
  category_votes = filter(votes, category == promise.category)

  total_yes = sum(category_votes.yes)
  total_no = sum(category_votes.no)

  pct_no = total_no / (total_yes + total_no)

  If pct_no >= 60%:
    AMPAY
```

### 3.3 Identified Problems

| Problem | Example | Impact |
|---------|---------|--------|
| **Excessive aggregation** | Promise "hospital X" vs. all health laws | Loses specificity |
| **Irrelevant votes** | Specific fiscal promise diluted across 100 fiscal votes | False negatives |
| **General pattern != specific promise** | 90% YES on health but NO on the specific law | Missed AMPAYs |

### 3.4 Example of Failure

```
Promise: "Implement tax reform based on universality"
Category: fiscal

v2 Result:
- Fiscal votes: 143
- FP: 93.7% YES
- Verdict: NO AMPAY

Reality:
- FP voted YES on 6/6 laws that CONTRADICT universality
- Should be AMPAY
```

### 3.5 Decision

**DISCARDED** -- Category aggregation loses critical information.

---

## 4. Version 3 (Superseded)

### 4.1 Description

Search for SPECIFIC laws related to each promise using extracted keywords.

### 4.2 Methodology

```
For each promise:
  keywords = extract_keywords(promise.text)

  specific_laws = search(votes, keywords)

  For each law in specific_laws:
    Verify party position

  If >= 60% NO:
    AMPAY
```

### 4.3 Improvements over v2

| Aspect | v2 | v3 |
|--------|----|----|
| Granularity | By category | By specific law |
| Matching | Single category | Multiple keywords |
| Precision | Low | Medium |

### 4.4 Identified Problem

**Direct search only:** Only searched for laws that IMPLEMENT the promise.

**Example of failure:**
```
Promise: "Eliminate exemptions"

v3 Search (direct):
- Keywords: "eliminar exoneracion"
- Laws found: 0
- Result: INSUFFICIENT DATA

Reality:
- 6 laws EXTENDING exemptions exist
- Party voted YES on all of them
- Should be AMPAY (inverse contradiction)
```

### 4.5 Decision

**SUPERSEDED** -- Functional but incomplete. Needs inverse search.

---

## 5. Version 4 (Superseded)

### 5.1 Description

Add INVERSE search: not only search for supporting laws, but also contradictory laws.

### 5.2 Methodology

```
For each promise:
  # Search A: Direct
  support_keywords = keywords that IMPLEMENT the promise
  support_laws = search(votes, support_keywords)
  no_support_ratio = count_NO(support_laws) / total(support_laws)

  # Search B: Inverse
  contradiction_keywords = keywords that CONTRADICT the promise
  contradiction_laws = search(votes, contradiction_keywords)
  yes_contradiction_ratio = count_YES(contradiction_laws) / total(contradiction_laws)

  # Decision matrix
  If no_support_ratio >= 60% OR yes_contradiction_ratio >= 60%:
    AMPAY
```

### 5.3 Improvements over v3

| Aspect | v3 | v4 |
|--------|----|----|
| Search directions | Direct only | Direct + Inverse |
| AMPAY types detected | Type A only | Type A + Type B |
| Coverage | Partial | Complete |

### 5.4 Identified Problem

**No confidence system:** All AMPAYs treated equally, without distinguishing the strength of evidence.

### 5.5 Decision

**SUPERSEDED** -- Complete detection but lacking confidence calibration.

---

## 6. Version 5 (Current)

### 6.1 Description

Add a confidence system and mandatory manual review.

### 6.2 Methodology

```
For each promise:
  # Detection (same as v4)
  execute_dual_search()

  # Calculate confidence
  confidence = calculate_confidence(
    num_laws,
    ratio,
    semantic_connection_strength
  )

  # Classify
  If confidence >= HIGH:
    AMPAY_candidate (automatic review)
  If confidence == MEDIUM:
    AMPAY_candidate (mandatory manual review)
  If confidence == LOW:
    Do not publish
```

### 6.3 Improvements over v4

| Aspect | v4 | v5 |
|--------|----|----|
| Confidence | Binary (AMPAY/NO) | HIGH/MEDIUM/LOW |
| Review | Optional | Mandatory for MEDIUM |
| Transparency | Basic | Complete documentation |

### 6.4 New Components

1. **Confidence system:** HIGH, MEDIUM, LOW
2. **Manual review process:** Standardized checklist
3. **Rejection documentation:** False positive log
4. **Appeals process:** Channel to dispute AMPAYs

### 6.5 Results

```
AMPAYs detected: 23
AMPAYs approved (HIGH): 4
AMPAYs approved (MEDIUM): 4
AMPAYs rejected: 17
AMPAYs final (after audit): 6
Estimated precision: 90%+
```

---

## 7. Detailed Timeline

| Date | Version | Event |
|------|---------|-------|
| 2026-01-18 | v1 | First implementation |
| 2026-01-18 | v1 | 200+ AMPAYs detected (suspicious) |
| 2026-01-18 | v1 | Manual review: >95% false positives |
| 2026-01-19 | v2 | Pivot to category aggregation |
| 2026-01-19 | v2 | 0 AMPAYs detected (suspicious) |
| 2026-01-19 | v2 | Identified: specific promises lost |
| 2026-01-20 | v3 | Pivot to keyword-based search |
| 2026-01-20 | v3 | AMPAYs detected but incomplete |
| 2026-01-20 | v4 | Add inverse search |
| 2026-01-20 | v4 | First valid AMPAY: FP-universality |
| 2026-01-21 | v5 | Add confidence system |
| 2026-01-21 | v5 | 6 AMPAYs validated for publication |

---

## 8. Lessons Learned

### 8.1 Mistakes Not to Repeat

| Mistake | Lesson |
|---------|--------|
| Trusting automated detection without review | Always validate with a human |
| Aggregating without measuring precision | Measure before publishing |
| Assuming more detections = better | Quality > quantity |
| Unidirectional search | Always search in both directions |

### 8.2 Established Principles

1. **Pattern > isolated event:** Minimum 3 laws
2. **Specificity > category:** Search for exact laws
3. **Bidirectional > unidirectional:** Direct + inverse
4. **Calibrated confidence:** Not all AMPAYs are equal
5. **Mandatory review:** Human in the loop

---

## 9. Future Improvements (Roadmap)

| Improvement | Priority | Status |
|-------------|----------|--------|
| Incorporate 2024-2026 data | High | Pending |
| Improve keyword extraction with NLP | Medium | Under research |
| Precision monitoring dashboard | Medium | Pending |
| API for journalists | Low | Future |

---

## 10. Files for Each Version

| Version | File | Status |
|---------|------|--------|
| v2 | `docs/methodology/archive/METHODOLOGY_V2_DEPRECATED.md` | Archived |
| v3 | `docs/methodology/archive/METHODOLOGY_V3_DEPRECATED.md` | Archived |
| v4 | `docs/methodology/METHODOLOGY_V4_DUAL_SEARCH.md` | Superseded |
| v5 | Documentation distributed across `/docs/methodology/` | **ACTIVE** |

---

*Last updated: 2026-01-21*
