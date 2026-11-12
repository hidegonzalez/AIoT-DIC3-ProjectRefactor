# NutriAgent Implementation & Refactor Tasks

## 1. OpenSpec & Architecture Specification (Steps 1–3)

- [x] 1.1 Initialize OpenSpec spec-driven development environment (`openspec init --tools antigravity`).
- [x] 1.2 Formulate Project Storyline A to F, 5 Core Innovations, 3 Architectural Trade-offs, and 10 Implementation Details (`NutriAgent_A_to_F_Core_Debates_Details.md`).
- [x] 1.3 Convert `DESIGN.md` (v2.0.0) and Proposal Algorithms 1–4 into OpenSpec `proposal.md`, `design.md`, and `specs/nutriagent-pipeline/spec.md`.

## 2. Core 8-Stage Agent Pipeline Verification

- [x] 2.1 **Wearable IoT & Dynamic Budgeting (`src/iot_pipeline.py`)**: Verify Mifflin-St Jeor BMR calculation, `0.65` active calorie damping factor, `900 kcal` single-meal cap, cross-sensor plausibility check (`steps > 10000` and `HR <= 70 bpm`), and `LOW_CONF` static fallback.
- [x] 2.2 **Spatial LBS Retriever (`src/spatial_lbs.py`)**: Verify Haversine distance calculation and automatic radius expansion (`1.2 km` $\rightarrow$ `2.5 km` when $|C| < 4$).
- [x] 2.3 **Deterministic Clinical & TFDA-11 Allergen Gate (`src/clinical_rules.py`)**: Verify hard-constraint filtering across Hypertension (`sodium <= 700 mg`, non-fried), Pre-diabetes (Low/Med GI, `added_sugar <= 5 g`), Gout (non-High purine), Dyslipidemia (`sat_fat <= 5 g`), CKD (`eGFR < 60` potassium/phosphorus gate), and TFDA 11 allergen alias expansion (`海鮮` $\rightarrow$ `魚類 + 甲殼類`), ensuring `CVR = 0.0%` and `AVR = 0.0%`.
- [x] 2.4 **Hybrid Ranking & Verbatim Grounded Dialogue (`src/recommender.py`, `src/agent.py`)**: Verify candidate shuffling (`shuffle(S)`), hybrid score $q = 0.5p + 0.5g$, numerical subset consistency check (`verify_explanation_consistency`), `12 s` timeout offline template fallback, and Human-in-the-Loop online preference weight updates (Algorithm 4).

## 3. Paper & Proposal Refactor Deliverables (Steps 4–5)

- [x] 3.1 Enrich A-to-F narrative with 2025–2026 external clinical and industry evidence (Perplexity 12 external sources, 300-word Chinese summary, and IEEE Systems Journal Abstract).
- [x] 3.2 Refactor **Chapter 1: Introduction** (`myplan/NutriAgent_Introduction_RelatedWork_Refactor.md`).
- [x] 3.3 Refactor **Chapter 2: Related Work** and **Article Summary Table + Research Gap** (`myplan/NutriAgent_Introduction_RelatedWork_Refactor.md`).
