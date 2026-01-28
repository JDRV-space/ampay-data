# AMPAY Detection Methodology v4
## Dual Search: Direct + Inverse

**Version:** 4.0
**Date:** 2026-01-21
**Status:** ACTIVE
**Supersedes:** v3 (direct only)

---

## CRITICAL INFORMATION

**v3 Failure:** Only searched for laws that SUPPORTED the promise, then checked whether the party voted NO.
**Problem:** Sometimes no supporting laws exist, or the party voted YES on all of them.
**v4 Fix:** ALSO search for laws that CONTRADICT the promise, and check whether the party voted YES.

---

## THE TWO SEARCHES

### SEARCH A: Direct (v3)
```
Promise: "X"
Question: Did the party vote NO on laws that WOULD IMPLEMENT X?
AMPAY if: >= 60% NO votes on implementing laws
```

### SEARCH B: Inverse (NEW)
```
Promise: "X"
Question: Did the party vote YES on laws that WOULD CONTRADICT X?
AMPAY if: >= 60% YES votes on contradictory laws
```

**Both searches must be executed for each promise.**

---

## EXAMPLE: FP-2021-005

**Promise:** "Implement tax reform based on the principle of universality"

### Search A (Direct):
- Laws implementing "universality" (eliminating exemptions): **0 found**
- Result: INSUFFICIENT DATA

### Search B (Inverse):
- Laws that CONTRADICT "universality" (extending exemptions): **4 found**
  - PL 3740: Extend IGV appendices
  - PL 3195: Extend IGV refund for mining/hydrocarbons (x2 votes)
  - PL 6473: Extend securities market exemption
- FP voted YES on: **4/4 = 100%**
- Result: **AMPAY (INVERSE)**

### Final Verdict: AMPAY
- Direct search: Insufficient data
- Inverse search: 100% contradiction
- Combined: AMPAY confirmed via inverse search

---

## METHODOLOGY STEPS

### STEP 1: Extract Keywords from the Promise
```
Promise: "Formalize MSMEs"
Keywords: mype, micro empresa, formalizar
```

### STEP 2: Define Supporting Laws vs. Contradictory Laws

**Supporting laws (for Direct search):**
- Laws that WOULD HELP achieve the promise
- Example: MSME formalization programs, MSME tax benefits

**Contradictory laws (for Inverse search):**
- Laws that WOULD HINDER or PREVENT the promise
- Example: Additional regulation for small businesses, taxes on MSMEs

### STEP 3: Search for Specific Laws

```bash
# Direct: Laws that support the promise
jq '.votes[] | select(.asunto | contains("SUPPORT_KEYWORD"))'

# Inverse: Laws that contradict the promise
jq '.votes[] | select(.asunto | contains("CONTRADICTION_KEYWORD"))'
```

### STEP 4: Verify the Party's Position on EACH Law

For each specific law found:
- Direct: Check whether the party voted NO (blocking its own promise)
- Inverse: Check whether the party voted YES (supporting the contradiction)

### STEP 5: Calculate Ratios

**Direct ratio:**
```
NO votes on supporting laws / Total supporting laws
>= 60% = AMPAY (DIRECT)
```

**Inverse ratio:**
```
YES votes on contradictory laws / Total contradictory laws
>= 60% = AMPAY (INVERSE)
```

### STEP 6: Final Verdict

| Direct | Inverse | Verdict |
|--------|---------|---------|
| AMPAY | AMPAY | AMPAY (strong) |
| AMPAY | NO | AMPAY (direct) |
| NO | AMPAY | AMPAY (inverse) |
| NO | NO | NO AMPAY |
| INSUF | AMPAY | AMPAY (inverse) |
| AMPAY | INSUF | AMPAY (direct) |
| INSUF | INSUF | INSUFFICIENT DATA |

---

## KEYWORD PAIRS BY PROMISE TYPE

### Tax Promises
| Promise Type | Supporting Keywords | Contradictory Keywords |
|--------------|---------------------|------------------------|
| Universality | eliminar exoneracion, derogar beneficio | prorrogar, extender exoneracion, regimen especial |
| Simplification | simplificar, unificar | nuevo impuesto, nuevo regimen |
| Reduce taxes | reducir, eliminar impuesto | aumentar, crear impuesto |

### Social Promises
| Promise Type | Supporting Keywords | Contradictory Keywords |
|--------------|---------------------|------------------------|
| Reduce poverty | programa social, bono, subsidio | recortar, eliminar programa |
| Protect workers | derechos laborales, salario minimo | flexibilizar, tercerizar |
| Formalize MSMEs | formalizar, mype, micro empresa | fiscalizar mype, sancionar informal |

### Environmental Promises
| Promise Type | Supporting Keywords | Contradictory Keywords |
|--------------|---------------------|------------------------|
| Protect environment | proteccion ambiental, conservar | exonerar mineria, prorrogar formalizacion minera |
| Reduce conflicts | dialogo, consulta previa | acelerar concesion, simplificar EIA |

---

## AMPAY THRESHOLDS

| Ratio | Status |
|-------|--------|
| >= 60% | **AMPAY** |
| 40-59% | **POTENTIAL AMPAY** |
| < 40% | **NO AMPAY** |
| < 3 laws found | **INSUFFICIENT DATA** |

**Note:** The same thresholds apply to both Direct and Inverse searches.

---

## OUTPUT FORMAT

```json
{
  "promise_id": "FP-2021-005",
  "promise_text": "Implementar reforma tributaria con principio de universalidad",
  "party": "Fuerza Popular",

  "direct_search": {
    "keywords": ["eliminar exoneración", "universalidad tributaria"],
    "laws_found": 0,
    "si_votes": 0,
    "no_votes": 0,
    "no_percentage": null,
    "status": "DATOS_INSUFICIENTES"
  },

  "inverse_search": {
    "keywords": ["prorrogar exoneración", "extender beneficio", "régimen especial"],
    "laws_found": 4,
    "laws_detail": [
      {"asunto": "PL 3740 prorrogar IGV", "position": "SI"},
      {"asunto": "PL 3195 devolución IGV mineras", "position": "SI"},
      {"asunto": "PL 3195 segunda votación", "position": "SI"},
      {"asunto": "PL 6473 prorrogar exoneración valores", "position": "SI"}
    ],
    "si_votes": 4,
    "no_votes": 0,
    "si_percentage": 100,
    "status": "AMPAY"
  },

  "final_verdict": "AMPAY",
  "ampay_type": "INVERSO",
  "reasoning": "FP prometió 'universalidad tributaria' pero votó SÍ en el 100% de las leyes que extienden tratamiento tributario especial para corporaciones."
}
```

---

## VERIFICATION CHECKLIST

Before flagging an AMPAY:

- [ ] Did we search for SPECIFIC laws (not category aggregation)?
- [ ] Did we execute BOTH Direct and Inverse searches?
- [ ] Did we verify the party's position on EACH specific law?
- [ ] Did we verify that the law truly supports/contradicts the promise (not just a keyword match)?
- [ ] Is the ratio >= 60%?

---

## COMPARISON: v3 vs v4

| Aspect | v3 | v4 |
|--------|----|----|
| Search direction | Direct only | Direct + Inverse |
| Question asked | Did they vote NO on supporting laws? | ALSO: Did they vote YES on contradictory laws? |
| Detects | Active opposition | Active opposition + Active contradiction |
| FP-2021-005 result | INSUFFICIENT DATA | **AMPAY (inverse)** |

---

## FILES

- v2 (deprecated): `METHODOLOGY_V2_DEPRECATED.md`
- v3 (superseded): `METHODOLOGY_V3.md`
- v4 (current): `METHODOLOGY_V4.md`
- Category baseline: `CATEGORY_VOTING_PATTERNS.md`

---

## FIRST AMPAY FOUND

**FP-2021-005:** "Implement tax reform based on the principle of universality"
- **Type:** INVERSE AMPAY
- **Evidence:** Voted YES on 4/4 laws extending special tax treatment
- **Validation:** Peru Libre voted NO on the same laws (consistent with their anti-exemption promise)

---

**END OF METHODOLOGY v4**
