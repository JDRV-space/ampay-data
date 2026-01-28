# Feature Spec: By Topic

*Created: 2026-01-21*

## Overview

Browse congressional votes (2021-2024) filtered by topic. User selects a category, sees all relevant votes and how each party voted.

## User Value

**Question answered:** "How did parties vote on [health/economy/security] issues?"

**Use case:** User wants to see actual voting behavior, not just promises. Complements the quiz (promises) with real data (votes).

---

## Data Source

```
/data/01_input/votes/votes_categorized.json
├── 2226 votes total
├── 15 categories
└── Links to voting CSVs per vote
```

---

## UI Flow

### Screen 1: Category Selection

```
┌─────────────────────────────────────┐
│  VOTES BY TOPIC                     │
│                                     │
│  Select a topic:                    │
│                                     │
│  [Salud (Health)]       164 votes   │
│  [Economia (Economy)]   226 votes   │
│  [Seguridad (Security)] 161 votes   │
│  [Educacion (Education)]180 votes   │
│  [Empleo (Employment)]  116 votes   │
│  [Agricultura (Agriculture)] 106    │
│  [Transporte (Transport)]    97     │
│  [Ambiente (Environment)]    77     │
│  [Fiscal (Fiscal Policy)]    78     │
│  [Agua (Water)]              57     │
│  [Energia (Energy)]          48     │
│  [Vivienda (Housing)]        42     │
│  [Mineria (Mining)]          19     │
│                                     │
│  [Justicia (Justice) - 776 votes]   │
│  ⚠️ Includes internal procedures    │
└─────────────────────────────────────┘
```

### Screen 2: Vote List (filtered by category)

```
┌─────────────────────────────────────┐
│  ← SALUD (HEALTH) (164 votes)      │
│                                     │
│  Filter: [Substantive ▼] [2023 ▼]  │
│                                     │
│  ┌─────────────────────────────────┐│
│  │ SIS Strengthening Act          ││
│  │ 15 Mar 2023 · Approved          ││
│  │ Substantive vote                ││
│  │                        [View →] ││
│  └─────────────────────────────────┘│
│                                     │
│  ┌─────────────────────────────────┐│
│  │ Healthcare System Reform       ││
│  │ 08 Nov 2022 · Rejected          ││
│  │ Substantive vote                ││
│  │                        [View →] ││
│  └─────────────────────────────────┘│
│                                     │
│  [Load more...]                     │
└─────────────────────────────────────┘
```

### Screen 3: Vote Detail

```
┌─────────────────────────────────────┐
│  ← SALUD (HEALTH)                   │
│                                     │
│  SIS STRENGTHENING ACT             │
│  March 15, 2023                     │
│  Result: APPROVED (78-42-10)        │
│                                     │
│  ┌─────────────────────────────────┐│
│  │ PARTY              │ VOTE       ││
│  │────────────────────│────────────││
│  │ Fuerza Popular     │ ✗ Against  ││
│  │ Peru Libre         │ ✓ In favor ││
│  │ Renovacion Popular │ ✓ In favor ││
│  │ Avanza Pais        │ ✗ Against  ││
│  │ Alianza Progreso   │ ✓ In favor ││
│  │ Somos Peru         │ ○ Abstain  ││
│  │ Podemos Peru       │ — Absent   ││
│  │ Juntos por el Peru │ ✓ In favor ││
│  │ Partido Morado     │ ✓ In favor ││
│  └─────────────────────────────────┘│
│                                     │
│  [View full detail on Congress.gob] │
│                                     │
│  ───────────────────────────────────│
│  This is public information from    │
│  the Congress of the Republic.      │
└─────────────────────────────────────┘
```

---

## Filters

| Filter | Options |
|--------|---------|
| **vote_type** | Sustantivo (Substantive - real laws), Declarativo (Declaratory), Procedural, All |
| **year** | 2021, 2022, 2023, 2024, All |
| **result** | Aprobado (Approved), Rechazado (Rejected), All |

**Default:** Substantive only (hide procedural noise)

---

## Data Processing

### Step 1: Load votes by category
```javascript
const votesByCategory = votes.filter(v => v.category === selectedCategory);
```

### Step 2: Load party votes from CSV
```
Each vote has: votaciones_path → CSV with columns:
- congresista
- grupo_parlamentario
- voto (A FAVOR, EN CONTRA, ABSTENCIÓN, SIN RESPONDER)
```

### Step 3: Aggregate by party
```javascript
// Group votes by partido, count A FAVOR / EN CONTRA / etc.
const partyVotes = aggregateByParty(votacionesCSV);
```

---

## Party Mapping

Congressional groups → Our party slugs:

| Grupo Parlamentario | party_slug |
|---------------------|------------|
| FUERZA POPULAR | fuerza_popular |
| PERU LIBRE | peru_libre |
| RENOVACION POPULAR | renovacion_popular |
| AVANZA PAIS | avanza_pais |
| ALIANZA PARA EL PROGRESO | alianza_progreso |
| SOMOS PERU | somos_peru |
| PODEMOS PERU | podemos_peru |
| JUNTOS POR EL PERU | juntos_peru |
| PARTIDO MORADO | partido_morado |

**Note:** Some members of Congress changed parties mid-term. Use grupo_parlamentario at time of vote.

---

## Edge Cases

1. **Party did not exist yet** - Show "N/A" or hide row
2. **Party had no members present** - Show "— Absent"
3. **Mixed votes within party** - Show majority + "(divided)"
4. **Low confidence categorization** - Flag with "⚠️ Approximate categorization"

---

## Integration with Quiz

After quiz results, show:

```
┌─────────────────────────────────────┐
│  Your answers align with:           │
│  JUNTOS POR EL PERU (82%)          │
│                                     │
│  [See how they voted in Congress →] │
└─────────────────────────────────────┘
```

Links to "By Topic" filtered by quiz topics (health, economy, etc.)

---

## Technical Notes

- **Data size:** 2226 votes x ~130 members of Congress = ~290K vote records
- **Caching:** Pre-aggregate party votes per vote_id, store in JSON
- **Performance:** Lazy load vote details on click

---

## MVP Scope

**Include:**
- Category selection
- Vote list with filters
- Party vote breakdown per vote

**Exclude (v2):**
- Individual member of Congress lookup
- Vote comparison across time
- Export functionality

---

## Disclaimer (required)

```
Data sourced from the portal of the Congress of the Republic.
AMPAY presents this information without modification.
This is not a voting recommendation.
```
