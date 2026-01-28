# Categorization of Parliamentary Votes

**Version:** 1.0
**Date:** 2026-01-21
**Status:** ACTIVE

---

## Executive Summary

Each congressional vote is classified into one of 15 thematic categories (14 general categories + justice). The classification combines keyword analysis with AI verification (Gemini/Claude).

---

## 1. 15-Category System

### 1.1 Category List

| # | Category | Display Name | Scope |
|---|----------|-------------|-------|
| 1 | `seguridad` (security) | Security | Crime, police, prisons, drug trafficking, terrorism |
| 2 | `economia` (economy) | Economy | Taxes, trade, investment, MSMEs, FTAs |
| 3 | `fiscal` | Fiscal | Deficit, budget, public debt, current spending |
| 4 | `social` | Social | Poverty, vouchers, social programs, pensions |
| 5 | `empleo` (employment) | Employment | Labor, informality, minimum wage |
| 6 | `educacion` (education) | Education | Schools, universities, scholarships, teachers |
| 7 | `salud` (health) | Health | Hospitals, medicines, anemia, SIS, ESSALUD |
| 8 | `agricultura` (agriculture) | Agriculture | Agribusiness, irrigation, farmers, agricultural prices |
| 9 | `agua` (water/sanitation) | Water/Sanitation | Drinking water, sewage, sewerage |
| 10 | `vivienda` (housing) | Housing | Housing deficit, urban planning, construction |
| 11 | `transporte` (transport) | Transport | Ports, airports, highways, railways |
| 12 | `energia` (energy) | Energy | Electricity, gas, petroleum, renewables |
| 13 | `mineria` (mining) | Mining | Formal/informal mining, canon, royalties |
| 14 | `ambiente` (environment) | Environment | Deforestation, waste, pollution, protected areas |

**Note:** Category 15 is "justicia/anticorrupcion" (justice/anti-corruption), used for AMPAYs and classification of votes related to the judicial system and anti-corruption. It is not included in voting pattern analysis (sparklines).

### 1.2 Excluded Categories

| Category | Reason for Exclusion |
|----------|---------------------|
| `digitalizacion` (digitalization) | It is a HOW, not a WHAT. Crosses all categories |
| `otros` (other) | Not permitted. Force classification into an existing category |

---

## 2. Categorization Process

### 2.1 Flow Diagram

```
┌─────────────────────────────────────────┐
│         VOTE SUBJECT                    │
│  "PL 3456 que modifica Ley del SIS"    │
└────────────────────┬────────────────────┘
                     │
                     ▼
         ┌───────────────────────┐
         │  KEYWORD MATCHING     │
         │  (First pass)         │
         └───────────┬───────────┘
                     │
        ┌────────────┼────────────┐
        │            │            │
   Single match  Multiple match  No match
        │            │            │
        ▼            ▼            ▼
   ┌─────────┐  ┌─────────┐  ┌─────────┐
   │ ASSIGN  │  │ AI to   │  │ AI to   │
   │ directly│  │ decide  │  │ classify│
   └─────────┘  └─────────┘  └─────────┘
                     │            │
                     └────┬───────┘
                          │
                          ▼
              ┌───────────────────────┐
              │  HUMAN VERIFICATION   │
              │  (random sample)      │
              └───────────────────────┘
```

### 2.2 Keyword Rules

**Matching priority:**

1. **Specific technical terms** (high precision)
2. **Institution names** (medium precision)
3. **General terms** (low precision, requires context)

### 2.3 Keyword Dictionary by Category

```
SEGURIDAD (security):
├── High confidence: PNP, policia, narcotrafico, terrorismo, penal, crimen
├── Medium confidence: detencion, seguridad ciudadana, requisitoria
└── Requires context: ley, delito, sancion

ECONOMIA (economy):
├── High confidence: MYPE, SUNAT, TLC, exportacion, importacion, aranceles
├── Medium confidence: competitividad, inversion, mercado
└── Requires context: empresa, comercio, fiscal

FISCAL:
├── High confidence: presupuesto, MEF, deficit, credito suplementario
├── Medium confidence: endeudamiento, gasto publico, tesoro
└── Requires context: recursos, financiamiento

SOCIAL:
├── High confidence: pension, ONP, AFP, bono, Qali Warma, Cuna Mas
├── Medium confidence: programa social, subsidio, pobreza
└── Requires context: beneficio, apoyo

EMPLEO (employment):
├── High confidence: laboral, SUNAFIL, salario minimo, CTS, gratificacion
├── Medium confidence: trabajador, contrato, despido, sindicato
└── Requires context: empleo, trabajo

EDUCACION (education):
├── High confidence: MINEDU, universidad, docente, beca, SUNEDU
├── Medium confidence: educacion, escuela, colegio, estudiante
└── Requires context: formacion, capacitacion

SALUD (health):
├── High confidence: MINSA, ESSALUD, SIS, hospital, farmacia, vacuna
├── Medium confidence: salud, medico, enfermedad, tratamiento
└── Requires context: atencion, servicio

AGUA (water/sanitation):
├── High confidence: SEDAPAL, saneamiento, alcantarillado, desague
├── Medium confidence: agua potable, acueducto, tratamiento aguas
└── Requires context: agua, hidrico

VIVIENDA (housing):
├── High confidence: vivienda, urbanismo, COFOPRI, Techo Propio
├── Medium confidence: construccion, habilitacion urbana
└── Requires context: inmobiliario

TRANSPORTE (transport):
├── High confidence: MTC, carretera, aeropuerto, puerto, ferrocarril
├── Medium confidence: transporte, infraestructura vial, peaje
└── Requires context: movilidad

ENERGIA (energy):
├── High confidence: OSINERGMIN, electricidad, gas, petroleo, hidrocarburo
├── Medium confidence: energia renovable, solar, eolica
└── Requires context: energia

MINERIA (mining):
├── High confidence: MINEM, mineria, canon minero, regalias mineras
├── Medium confidence: concesion minera, exploracion, explotacion
└── Requires context: extraccion, recursos

AMBIENTE (environment):
├── High confidence: MINAM, SERNANP, area protegida, deforestacion
├── Medium confidence: ambiental, contaminacion, residuos, reciclaje
└── Requires context: conservacion, ecosistema
```

---

## 3. AI Classification

### 3.1 Classification Prompt

```
Classify the following Peruvian Congress vote subject into ONE of these categories:
seguridad, economia, fiscal, social, empleo, educacion, salud, agua, vivienda, transporte, energia, mineria, ambiente

SUBJECT: "[vote subject text]"

Rules:
1. Choose the PRIMARY category, not a secondary one
2. When in doubt, choose the most specific category
3. NEVER use "other" or invent categories
4. Respond ONLY with the lowercase category name

Category:
```

### 3.2 Model Used

| Model | Use | Cost |
|-------|-----|------|
| Claude Opus | Final classification, AMPAYs | High |
| Gemini Flash | Bulk pre-classification | Low |

### 3.3 Confidence Thresholds

```
Confidence >= 0.95  -> Accept directly
Confidence 0.80-0.94 -> Review if conflicting keyword exists
Confidence < 0.80   -> Mandatory human review
```

---

## 4. Special Cases

### 4.1 Multi-Category Votes

Some votes touch multiple topics. Rule: **choose the primary category**.

**Example:**
```
Subject: "Law promoting formal mining to reduce poverty"
Possible categories: mineria (mining), social
Classification: mineria (the object of the law; poverty is the objective)
```

### 4.2 Sectoral Budgets

Budget laws are classified by SECTOR, not as "fiscal."

**Example:**
```
Subject: "Health sector budget 2024"
Classification: salud (health) (not fiscal)

Subject: "Public sector financial balance act"
Classification: fiscal (cross-cutting)
```

### 4.3 Institutional Reforms

Reforms of public entities are classified by the entity's FUNCTION.

**Example:**
```
Subject: "Law restructuring SEDAPAL"
Classification: agua (water)

Subject: "Law reorganizing the PNP"
Classification: seguridad (security)
```

---

## 5. Categorization Statistics

### 5.1 Distribution by Category (2,226 votes)

| Category | Votes | % of Total |
|----------|-------|------------|
| economia (economy) | 412 | 18.5% |
| seguridad (security) | 298 | 13.4% |
| salud (health) | 258 | 11.6% |
| educacion (education) | 245 | 11.0% |
| social | 198 | 8.9% |
| empleo (employment) | 187 | 8.4% |
| agricultura (agriculture) | 156 | 7.0% |
| fiscal | 143 | 6.4% |
| transporte (transport) | 121 | 5.4% |
| ambiente (environment) | 89 | 4.0% |
| agua (water/sanitation) | 67 | 3.0% |
| vivienda (housing) | 31 | 1.4% |
| mineria (mining) | 21 | 0.9% |

### 5.2 Classification Accuracy

Based on manual review of a sample (n=200):

| Method | Accuracy |
|--------|----------|
| Unique keyword matches | 98.2% |
| Keywords + AI | 95.7% |
| AI only | 91.3% |
| Weighted average | **94.8%** |

---

## 6. Validation

### 6.1 QA Process

1. **Random sample:** 5% of votes manually reviewed
2. **Edge cases:** All cases with confidence < 0.85 reviewed
3. **Consistency:** Similar votes must share the same category

### 6.2 Quality Metrics

| Metric | Acceptable Threshold | Actual |
|--------|---------------------|--------|
| Accuracy | >= 90% | 94.8% |
| Unclassified cases | 0% | 0% |
| Average time per vote | < 2s | 0.8s |

---

## 7. Limitations

1. **Inherent ambiguity:** Some votes genuinely cross categories
2. **Language evolution:** New terms may not be in the dictionary
3. **Lost context:** Only the "subject" is analyzed, not the full text of the law
4. **Model biases:** AI may have classification biases

---

## 8. Related Files

| File | Content |
|------|---------|
| `data/02_output/votes_categorized.json` | Votes with assigned category |
| `docs/CATEGORIES.md` | Category definitions |

---

## References

For all academic references and sources used in AMPAY, see the centralized document:
[Bibliography and Sources](/referencia/fuentes)

---

*Last updated: 2026-01-21*
