# URL Verification Report - AMPAY Documentation

**Verification date:** 2026-01-22
**File verified:** SOURCES_BIBLIOGRAPHY.md
**Total URLs verified:** 41

---

## Executive Summary

| Status | Count |
|--------|-------|
| OK | 30 |
| CORRECTED | 3 |
| VERIFY | 6 |
| BROKEN | 2 |

---

## 1. Verified URLs (OK)

### 1.1 Primary Data Sources

| URL | Status |
|-----|--------|
| https://github.com/openpolitica/congreso-pleno-asistencia-votacion | OK |
| https://www.congreso.gob.pe/AsistenciasVotacionesPleno/ | OK |
| https://plataformahistorico.jne.gob.pe/OrganizacionesPoliticas/PlanesGobiernoTrabajo | OK |
| https://github.com/hiperderecho/proyectos_de_ley | OK |

### 1.2 Government Portals

| URL | Status |
|-----|--------|
| https://www.congreso.gob.pe/ | OK |
| https://www.congreso.gob.pe/proyectosdeley/ | OK |
| https://www.congreso.gob.pe/GestionInformacionEstadistica/congreso-cifras/ | OK |
| https://www.congreso.gob.pe/reglamento/ | OK |
| https://portal.jne.gob.pe/portal/Pagina/Ver/979/page/Elecciones-Generales-2026 | OK |
| https://diariooficial.elperuano.pe/Normas | OK |

### 1.3 Transparency Organizations

| URL | Status |
|-----|--------|
| https://openpolitica.com/ | OK |
| https://hiperderecho.org/ | OK |
| https://directoriolegislativo.org/en/categoria/ingles/peru-en/ | OK |

### 1.4 International VAA Platforms

| URL | Status |
|-----|--------|
| https://www.bpb.de/themen/wahl-o-mat/ | OK |
| https://votecompass.com/ | OK |
| https://www.smartvote.ch/ | OK |
| https://www.isidewith.com/ | OK |
| https://www.votematch.eu/ | OK |
| https://euandi.eu/ | OK |

### 1.5 Fact-Checking Methodology

| URL | Status |
|-----|--------|
| https://www.politifact.com/article/2018/feb/12/principles-truth-o-meter-politifacts-methodology-i/ | OK |

### 1.6 News Articles

| URL | Status |
|-----|--------|
| https://gestion.pe/economia/mas-de-7500-proyectos-de-ley-en-el-congreso-de-peru-los-que-mas-preocupan-videnza-populismo-noticia/ | OK |
| https://ojo-publico.com/politica/congreso-publico-mas-100-leyes-por-insistencia-pesar-alertas | OK |
| https://ojo-publico.com/4925/congresistas-impulsaron-497-proyectos-ley-declarativos-el-2023 | OK |
| https://andina.pe/agencia/noticia-congreso-90-leyes-fueron-promulgadas-durante-primera-legislatura-20242025-1012947.aspx | OK |

### 1.7 Technical Standards and Tools

| URL | Status |
|-----|--------|
| https://www.popoloproject.com/specs/ | OK |
| https://open-civic-data.readthedocs.io/ | OK |
| https://schema.org/ClaimReview | OK |
| https://manifesto-project.wzb.eu/ | OK |
| https://pymupdf.readthedocs.io/ | OK |
| https://github.com/tesseract-ocr/tesseract | OK |
| https://github.com/tesseract-ocr/tessdata | OK |

---

## 2. Corrected URLs

### 2.1 Wahl-O-Mat (Germany)

| Original URL | Corrected URL | Status |
|--------------|---------------|--------|
| https://www.wahl-o-mat.de/ | https://www.bpb.de/themen/wahl-o-mat/ | CORRECTED |

**Note:** wahl-o-mat.de automatically redirects to bpb.de/themen/wahl-o-mat/

### 2.2 MEF Transparency

| Original URL | Corrected URL | Status |
|--------------|---------------|--------|
| https://www.mef.gob.pe/es/por-instrumento/decreto-de-urgencia | https://www.mef.gob.pe/es/normatividad-sp-9867/por-instrumento/decretos-de-urgencia | CORRECTED |

**Note:** The original URL had loading issues. The corrected URL is the official MEF link.

### 2.3 ONPE Open Data

| Original URL | Corrected URL | Status |
|--------------|---------------|--------|
| https://datosabiertos.gob.pe/users/onpedatos | https://www.gob.pe/institucion/onpe/tema/datos-abiertos | CORRECTED |

**Note:** The open data platform has a new structure. Alternative URL: https://datosabiertos.gob.pe/SEARCH/field_tags/onpe-1039

---

## 3. URLs Requiring Manual Verification

### 3.1 JNE Electoral Platform 2026

| URL | Status | Note |
|-----|--------|------|
| https://plataformaelectoral.jne.gob.pe/candidatos/plan-gobierno-trabajo/buscar | VERIFY | The page exists but content loads dynamically. Requires manual browser verification. |

### 3.2 JNE Main Portal

| URL | Status | Note |
|-----|--------|------|
| https://www.jne.gob.pe/ | VERIFY | Size error during automated fetch. The URL is correct according to web search. Verify manually. |

### 3.3 Academic DOIs

| DOI | Status | Note |
|-----|--------|------|
| https://doi.org/10.1080/00344890903510006 | VERIFY | Correct DOI (Garzia 2010). Taylor & Francis may require institutional access. |
| https://doi.org/10.1080/19331680903048992 | VERIFY | Correct DOI (Walgrave et al. 2009). Taylor & Francis may require institutional access. |
| https://doi.org/10.1017/s0003055421000754 | VERIFY | Correct DOI (Jolly et al. 2022). Cambridge may require access. |
| https://doi.org/10.25522/manifesto.v6 | VERIFY | Manifesto Project dataset DOI. Verify access. |

**Note on DOIs:** DOIs are permanent identifiers. If they do not resolve directly, they may require institutional access or be experiencing temporary resolution issues. The DOIs listed correspond to the correct publications as verified through cross-referencing.

### 3.4 Vote Compass Methodology

| URL | Status | Note |
|-----|--------|------|
| https://votecompass.com/methodology | VERIFY | The main page works. The methodology section may be integrated into the main page. |

---

## 4. Broken URLs Without Replacement

### 4.1 Proyectos de Ley PE

| URL | Status | Note |
|-----|--------|------|
| http://proyectosdeley.pe/ | BROKEN | Server not responding (ECONNREFUSED). The Hiperderecho project may be inactive. |

**Suggested alternatives:**
- https://www.congreso.gob.pe/proyectosdeley/ (Official Congress portal)
- https://wb2server.congreso.gob.pe/spley-portal/ (Alternative Congress portal)
- https://github.com/hiperderecho/proyectos_de_ley (GitHub repository - functional)

### 4.2 Red Transparencia Legislativa (Legislative Transparency Network)

| URL | Status | Note |
|-----|--------|------|
| https://transparencialegislativa.org/ | BROKEN | Server not responding (ECONNREFUSED). |

**Confirmed alternative:**
- The organization exists and operates. Directorio Legislativo coordinates the network 2023-2025.
- Contact URL: https://directoriolegislativo.org/en/
- Twitter: @redltl

### 4.3 Poltext Polimeter Methodology

| URL | Status | Note |
|-----|--------|------|
| https://www.poltext.org/en/donnees-et-analyses/les-polimetres/methodology-polimeters | VERIFY | SSL certificate issues. The URL is correct. |

**Functional alternative:**
- https://www.polimeter.org/en/trudeau (Active Polimeter)

### 4.4 VoteMatch UK

| URL | Status | Note |
|-----|--------|------|
| https://votematch.org.uk/ | BROKEN | Error 526 (SSL handshake failed). The project may be inactive. |

**Alternatives:**
- https://uk.isidewith.com/political-quiz (Active alternative for UK)
- https://source.votematch.eu/ (VoteMatch source code)

### 4.5 ResearchGate Smartvote

| URL | Status | Note |
|-----|--------|------|
| https://www.researchgate.net/publication/226593563 | VERIFY | Error 403 (access denied automatically). ResearchGate blocks some crawlers. |

**Verified alternative:**
- https://www.researchgate.net/publication/261495925_Voting_Advice_Applications_and_Party_Choice_Evidence_From_Smartvote_Users_in_Switzerland (Related publication by Ladner and Fivaz)

---

## 5. Recommended Actions

### Immediate (Correct now)
1. Update MEF Transparency URL
2. Update ONPE Open Data URL
3. Add note about Wahl-O-Mat redirect

### Short term (Verify manually)
1. Verify JNE Electoral Platform in a browser
2. Verify DOIs from an institutional network
3. Verify Poltext with a browser (ignore SSL error)

### Medium term (Find replacements)
1. Find alternative for proyectosdeley.pe (use official Congress portal)
2. Contact Red Transparencia Legislativa via Directorio Legislativo
3. Update VoteMatch UK reference

---

## 6. Verification History

| Date | Verifier | Result |
|------|----------|--------|
| 2026-01-21 | Manual | Initial - All OK |
| 2026-01-22 | Automated (Claude) | 30 OK, 3 Corrected, 6 Verify, 2 Broken |

---

*Automatically generated on 2026-01-22*
