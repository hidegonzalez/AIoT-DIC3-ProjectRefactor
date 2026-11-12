# Proposal: NutriAgent 8-Stage Grounded Conversational Dietary Workflow

## Why

Dining out accounts for nearly 70% of daily meals in Taiwan, while over 63% of adults exceed recommended daily sodium limits and 11.9% suffer from chronic kidney disease (CKD) with only a 3.5% awareness rate. Although over 40 million people globally now query Large Language Models (LLMs) daily for health and dietary guidance, empirical studies show that general-purpose LLMs systematically underestimate meal-box sodium content (with coefficients of variation reaching 20%–70%), generate renal diets that exceed safe potassium limits, and fail to improve lay clinical decision-making in randomized controlled trials. We need a spec-driven, safety-by-design conversational agent workflow where deterministic clinical and allergen rules govern safety and numerical accuracy, while the LLM handles intent understanding, semantic ranking within a pre-verified safe pool, and grounded natural language generation.

## What Changes

- Convert `DESIGN.md` (v2.0.0) into a formal OpenSpec workflow specification for the 8-stage **NutriAgent** pipeline.
- Define strict architectural decoupling between deterministic clinical/allergen filtering (`src/clinical_rules.py`), error-aware wearable IoT calorie budgeting (`src/iot_pipeline.py`), Haversine spatial retrieval (`src/spatial_lbs.py`), and hybrid LLM ranking/dialogue (`src/recommender.py`, `src/agent.py`).
- Enforce verbatim numerical verification (`verify_explanation_consistency`) on all Gemini-generated responses and automatic offline fallback (`LOW_CONF` / deterministic template) when wearable sensors or LLM APIs fail.
- Formalize the 5 core innovations, 3 architectural trade-offs, and 10 implementation/evaluation details (Algorithms 1–4).

## Capabilities

### New Capabilities
- `nutriagent-pipeline`: End-to-end 8-stage conversational dietary decision workflow integrating wearable IoT telemetry, Haversine LBS retrieval, deterministic clinical & TFDA-11 allergen hard-constraint gating, de-biased hybrid ranking, verbatim numerical verification, and Human-in-the-Loop online preference updates.

### Modified Capabilities
- None (initial OpenSpec workflow conversion from `DESIGN.md` v2.0.0).

## Impact

- **Modules**: `src/iot_pipeline.py`, `src/clinical_rules.py`, `src/spatial_lbs.py`, `src/recommender.py`, `src/agent.py`, `app.py`, `tests/test_agent.py`, `benchmark.py`.
- **Safety Guarantees**: Clinical Violation Rate ($\text{CVR} = 0.0\%$) and Allergen Violation Rate ($\text{AVR} = 0.0\%$) regardless of LLM availability or prompt injection attempts.
- **Performance Targets**: Calorie Mean Absolute Error ($\text{MAE} \le 180\text{ kcal}$, prototype verified at $114.48\text{ kcal}$), offline latency $\le 50\text{ ms}$ (prototype verified at $0.08\text{ ms}$), Gemini P95 latency $\le 5\text{ s}$.
