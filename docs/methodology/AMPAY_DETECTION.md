# AMPAY Detection: Methodology v5

**Version:** 5.0
**Date:** 2026-01-21
**Status:** ACTIVE
**Supersedes:** v4 (dual search), v3 (direct search), v2 (category aggregation)

---

## Executive Summary

An **AMPAY** is a verifiable contradiction between a campaign promise and the party's voting behavior in Congress. Detection employs dual search (direct + inverse) over specific legislation.

---

## 1. Definition of AMPAY

### 1.1 What Is an AMPAY

```
AMPAY = A party voted contrary to its campaign promise
```

**Types of AMPAY:**

| Type | Description | Example |
|------|-------------|---------|
| **Type A (Direct)** | Party voted NO on legislation that would implement its promise | Promised "increase education budget," voted NO on increase bill |
| **Type B (Inverse)** | Party voted YES on legislation that contradicts its promise | Promised "eliminate exemptions," voted YES on bill extending them |

### 1.2 What Is NOT an AMPAY

- A single isolated vote (a pattern is required; minimum 3 laws)
- A vote cast for documented procedural reasons
- A vague promise with no identifiable related legislation
- Insufficient data (fewer than 3 laws found)

---

## 2. Detection Process

### 2.1 Flow Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                       CAMPAIGN PROMISE                           │
│  "Implement tax reform based on universality"                    │
└─────────────────────────────┬───────────────────────────────────┘
                              │
                              ▼
         ┌────────────────────┴────────────────────┐
         │                                         │
         ▼                                         ▼
┌─────────────────────┐               ┌─────────────────────┐
│   SEARCH A          │               │   SEARCH B          │
│   (Direct)          │               │   (Inverse)         │
│                     │               │                     │
│ Laws that           │               │ Laws that           │
│ IMPLEMENT           │               │ CONTRADICT          │
│ the promise         │               │ the promise         │
└──────────┬──────────┘               └──────────┬──────────┘
           │                                     │
           ▼                                     ▼
┌─────────────────────┐               ┌─────────────────────┐
│ Did the party vote  │               │ Did the party vote  │
│ NO on these laws?   │               │ YES on these laws?  │
└──────────┬──────────┘               └──────────┬──────────┘
           │                                     │
           ▼                                     ▼
    ┌──────┴──────┐                       ┌──────┴──────┐
    │ >= 60% NO   │                       │ >= 60% YES  │
    │ = AMPAY (A) │                       │ = AMPAY (B) │
    └─────────────┘                       └─────────────┘
```

### 2.2 Step 1: Keyword Extraction

From each promise, extract searchable terms:

```
Promise: "Formalize one million new MSMEs"
Party: Fuerza Popular
ID: FP-2021-002

Extracted keywords:
├── Nouns: mype, mypes, micro empresa, pequena empresa
├── Verbs: formalizar, formalizacion
├── Synonyms: microempresa, emprendimiento
└── Technical terms: RUC, RUS, regimen especial
```

### 2.3 Step 2: Dual Search

**Search A (Direct):**
- Find laws that WOULD IMPLEMENT the promise
- Check whether the party voted NO

**Search B (Inverse):**
- Find laws that CONTRADICT the promise
- Check whether the party voted YES

```bash
# Example search for "tax universality"

# Search A: Laws that eliminate exemptions (implement the promise)
jq '.votes[] | select(.asunto | contains("eliminar exoneracion"))'

# Search B: Laws that extend exemptions (contradict the promise)
jq '.votes[] | select(.asunto | contains("prorrogar") or contains("extender exoneracion"))'
```

### 2.4 Step 3: Ratio Calculation

**Direct Ratio:**
```
NO_votes_on_implementing_laws / Total_implementing_laws
>= 60% = AMPAY (TYPE A)
```

**Inverse Ratio:**
```
YES_votes_on_contradictory_laws / Total_contradictory_laws
>= 60% = AMPAY (TYPE B)
```

### 2.5 Step 4: Decision Matrix

| Search A | Search B | Final Verdict |
|----------|----------|---------------|
| AMPAY | AMPAY | **AMPAY (strong)** |
| AMPAY | NO | **AMPAY (direct)** |
| NO | AMPAY | **AMPAY (inverse)** |
| NO | NO | NO AMPAY |
| INSUF | AMPAY | **AMPAY (inverse)** |
| AMPAY | INSUF | **AMPAY (direct)** |
| INSUF | INSUF | INSUFFICIENT DATA |

---

## 3. Thresholds and Filters

### 3.1 Classification Thresholds

| Percentage | Status | Action |
|------------|--------|--------|
| >= 60% | **AMPAY** | Publish with manual review |
| 40-59% | **POTENTIAL AMPAY** | Requires detailed review |
| < 40% | **NO AMPAY** | Do not publish |
| < 3 laws | **INSUFFICIENT DATA** | Do not evaluate |

### 3.2 Applied Filters

1. **Substantive votes only:** Exclude declarative and procedural votes
2. **Semantic relevance:** The law must directly relate to the promise
3. **Human verification:** Each AMPAY reviewed before publication

---

## 4. Confidence Levels

### 4.1 Confidence System

| Level | Criteria | Action |
|-------|----------|--------|
| **HIGH** | >= 60% ratio + >= 5 laws + clear semantic connection | Auto-publish with flag |
| **MEDIUM** | >= 60% ratio + 3-4 laws OR weak semantic connection | Mandatory review |
| **LOW** | 40-59% ratio OR limited data | Do not publish without further investigation |

### 4.2 Classification Examples

**AMPAY-001 (HIGH):**
```json
{
  "promise": "Universalidad tributaria",
  "inverse_search": {
    "laws_found": 6,
    "si_percentage": 100
  },
  "confidence": "HIGH",
  "reasoning": "6/6 votos SI en leyes que extienden regimenes especiales"
}
```

**AMPAY-006 (MEDIUM):**
```json
{
  "promise": "Fortalecer sistema electoral",
  "direct_search": {
    "laws_found": 2,
    "no_percentage": 100
  },
  "confidence": "MEDIUM",
  "reasoning": "Solo 2 mociones encontradas, pero patron claro"
}
```

---

## 5. Manual Review

### 5.1 Verification Checklist

Before publishing any AMPAY:

- [ ] Is the promise verifiable and specific?
- [ ] Are the identified laws directly related to the promise?
- [ ] Did the party have a genuine opportunity to vote (quorum)?
- [ ] Is there additional context that explains the vote?
- [ ] Does the ratio meet the threshold (>= 60%)?
- [ ] Were at least 3 laws analyzed?

### 5.2 Review Process

```
1. Analyst reviews automated AMPAY
2. Verifies sources (promise PDF, voting record)
3. Confirms semantic connection between promise and law
4. Documents reasoning
5. Approves or rejects
6. If approved, enters publication queue
```

### 5.3 Rejection Documentation

If a potential AMPAY is rejected, document:
- Reason for rejection
- Evidence disqualifying it
- Date of review
- Responsible reviewer

---

## 6. Methodology Evolution

### 6.1 Version History

| Version | Problem Identified | Solution Implemented |
|---------|-------------------|---------------------|
| v1 | Vote-by-vote comparison with no pattern | Category aggregation |
| v2 | Aggregation lost specificity | Search by specific keywords |
| v3 | Only searched for supporting laws | Added inverse search |
| v4 | Incomplete dual search | Complete decision matrix |
| v5 | Arbitrary thresholds | Calibrated confidence system |

### 6.2 v3 vs v4 vs v5

| Aspect | v3 | v4 | v5 |
|---------|----|----|----|-
| Search direction | Direct only | Direct + inverse | Direct + inverse |
| AMPAY threshold | 60% NO | 60% either direction | 60% + confidence |
| Minimum laws | 3 | 3 | 3 with type adjustment |
| Human review | No | Partial | Mandatory |
| Confidence level | No | No | Yes |

---

## 7. Documented Examples

### 7.1 Confirmed AMPAY (Type B - Inverse)

```
ID: AMPAY-001
Party: Fuerza Popular
Promise: "Implement tax reform based on the principle of universality"

Search A (Direct):
- Laws found: 0
- Status: INSUFFICIENT DATA

Search B (Inverse):
- Keywords: "prorrogar exoneracion", "extender beneficio", "regimen especial"
- Laws found: 6
  1. PL 3740 - Extend IGV appendices -> FP voted YES
  2. PL 3195 - IGV refund for mining/hydrocarbons -> FP voted YES
  3. PL 3195 - Second vote -> FP voted YES
  4. PL 6473 - Extend securities exemption -> FP voted YES
  5. PL 3155 - Special depreciation regimes -> FP voted YES
  6. PL 3671 - Fund fiscal incentives -> FP voted YES
- YES votes: 6/6 = 100%
- Status: AMPAY

Verdict: AMPAY (TYPE B - INVERSE)
Confidence: HIGH
Reasoning: FP promised tax universality but voted YES on 6/6 laws extending special regimes.
```

### 7.2 NO AMPAY (Support Pattern)

```
ID: NO-AMPAY-FP-002
Party: Fuerza Popular
Promise: "Formalize one million new MSMEs"

Search A (Direct):
- Keywords: "mype", "micro empresa", "formalizacion"
- Laws found: 26
- FP voted YES: 26/26 = 100%
- Status: NO AMPAY

Verdict: NO AMPAY
Reasoning: FP consistently supported MSME legislation.
```

---

## 8. Limitations

1. **Data availability:** Analysis covers only the period 2021-07 to 2024-07
2. **Absences:** Does not distinguish between justified absences and strategic evasion
3. **Legislative context:** Does not capture prior negotiations or compromises
4. **Legal complexity:** Laws with multiple articles may contain both positive and negative provisions
5. **Party switching:** Legislators who switched parties affect per-party tallies

---

## 9. Related Files

| File | Content |
|------|---------|
| `data/02_output/ampays.json` | Confirmed AMPAYs |
| `data/02_output/AMPAY_CONFIRMED_2021.json` | Evidence detail |
| `docs/methodology/archive/METHODOLOGY_V*.md` | Previous versions |

---

## References

For all academic references and sources used in AMPAY, see the centralized document:
[Bibliography and Sources](/referencia/fuentes)

---

*Last updated: 2026-01-21*
