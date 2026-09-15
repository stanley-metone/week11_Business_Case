# KPC Secure AI Tooling Pipeline

**Team Apex Innovators**  
**Inuka Hackathon - Problem 10, Domain E: HSE, Compliance & Deployment Readiness**

A secure operational-data pipeline prototype for turning raw depot, pipeline and maintenance data into trusted, privacy-protected, decision-ready information.

> **Prototype status:** Phase 1 uses synthetic operational data. The project demonstrates the control flow, analytics logic, dashboard and CI/CD safeguards; it is **not** a production KPC deployment. Financial results below are planning estimates that must be validated with representative KPC data.

## Executive summary

Operational teams can lose time when records arrive incomplete, duplicated, out of range or with sensitive identifiers that must be protected before analytics use. This project creates one controlled path that:

1. checks incoming records for common quality failures;
2. masks defined sensitive fields;
3. flags abnormal pressure patterns and pipeline-health risks;
4. gives managers a clear executive dashboard; and
5. prevents unsafe releases through automated tests, security checks, health checks and rollback logic.

### Board recommendation

**GO - approve a stage-gated implementation, beginning with an 8-week pilot.**

The pilot should confirm the real data-quality baseline, actual analyst review time, privacy requirements and financial assumptions before any scale-up decision.

## Business value

### Planning assumptions

- Records processed: **25,000 per week**
- Records needing manual review: **5%** (derived from the 95% quality target)
- Review time: **4 minutes per record**
- Fully loaded analyst cost: **KES 1,500 per hour**
- One-time implementation estimate: **KES 1.80M**
- Annual support estimate: **KES 0.60M**

### Financial case

```text
Gross annual labour cost avoided
= 25,000 x 5% x 4/60 x KES 1,500 x 52
= KES 6.50M

Net recurring annual savings
= KES 6.50M - KES 0.60M
= KES 5.90M

Year-1 total cost
= KES 1.80M + KES 0.60M
= KES 2.40M

Year-1 ROI
= (KES 6.50M - KES 2.40M) / KES 2.40M
= 171%

Payback period
= KES 1.80M / (KES 5.90M / 12)
= 3.7 months
```

These figures are **not realized savings**. The repository dashboard explicitly treats its default values as placeholders pending stakeholder confirmation.

## What is implemented

| Component | Purpose | Business outcome |
|---|---|---|
| `ingestion_quality_gates.py` | Checks schema, ranges, duplicates and completeness | Reduces avoidable manual clean-up and prevents poor-quality records from flowing downstream |
| `anonymization_engine.py` | Masks sensitive identifiers and applies k-anonymity controls | Protects staff, vehicle and location information before analytics use |
| `predictive_model.py` | Detects unusual pressure behaviour and forecasts breach risk | Supports earlier operational attention instead of waiting for a hard threshold failure |
| `alerting_engine.py` | Converts quality/privacy/predictive conditions into actions | Makes exceptions visible and decision-ready |
| `app.py` | Streamlit executive dashboard | Shows business value, pipeline quality, privacy status and alerts in one view |
| `.github/workflows/deploy.yml` | Tests, security scan, image build, canary health check, promotion and rollback | Reduces release risk and preserves the last known-good version |
| `tests/` | Unit tests across the main modules | Blocks releases when core behaviour fails |

## Success metrics

The pilot should only proceed to scale if it demonstrates:

- **>=95%** accepted-record target;
- **100%** masking of defined sensitive fields;
- **<10%** noisy-alert target;
- **>=99.5%** service-uptime target for the deployed service;
- reliable release health checks and rollback; and
- a measured reduction in analyst review time that supports the ROI case.

## Architecture

```text
Operational data
      |
      v
Automated quality checks
      |
      v
Sensitive-field protection
      |
      +--------------------+
      |                    |
      v                    v
Predictive alerts     Clean protected data
      |                    |
      +---------+----------+
                v
        Executive dashboard
                |
                v
   Tested / health-checked release
                |
                v
        Safe promotion or rollback
```

## Quick start

### 1. Clone the repository

```bash
git clone https://github.com/LosBandidox/KPC-Secure-AI-Tooling-Pipeline.git
cd KPC-Secure-AI-Tooling-Pipeline
```

### 2. Create an environment and install dependencies

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
# macOS/Linux: source .venv/bin/activate

pip install -r requirements.txt
```

For tests and security tooling:

```bash
pip install -r requirements-dev.txt
```

### 3. Run the test gate

```bash
pytest tests/ -v
```

### 4. Run the modules directly

```bash
python ingestion_quality_gates.py
python anonymization_engine.py
python alerting_engine.py
python predictive_model.py
```

### 5. Launch the executive dashboard

```bash
streamlit run app.py
```

### 6. Optional Docker run

```bash
docker build -t kpc-pipeline .
docker run -p 8501:8501 kpc-pipeline
```

Then open `http://localhost:8501`.

## CI/CD release flow

Every push or pull request to `main` is designed to pass through controlled release stages:

1. **Test gate** - unit tests must pass.
2. **Security scan** - medium-or-higher code findings block the build.
3. **Build** - create the Docker image.
4. **Canary health check** - run the new image and verify the Streamlit health endpoint.
5. **Promote** - mark the tested image as stable.
6. **Rollback protection** - preserve the prior stable image and raise a tracking issue when promotion fails.

## 8-week pilot plan

| Period | Goal | Decision evidence |
|---|---|---|
| Weeks 1-2 | Baseline | Real data sources, current review process, sensitive fields, analyst effort |
| Weeks 3-5 | Integrate & test | Quality rules, masking, alerting and release controls validated on representative data |
| Weeks 6-7 | Operate | Repeat cycles; measure alert usefulness, analyst time saved and failure handling |
| Week 8 | Board gate | Recalculate ROI and choose **Scale / Adjust / Stop** |

## Main risks and mitigation

### 1. The savings model does not match real operations

**Mitigation:** use representative KPC data, measure actual volumes and review time, and recalculate ROI before scaling.

### 2. Automation misses a quality, privacy or alerting issue

**Mitigation:** retain human override, verify masking, monitor false alerts, restrict access, keep visible exceptions and preserve rollback.

## Team

- **Stanley Metone** - Project Lead / Data Scientist
- **David Kimathi** - Data Engineer (Pipeline Reliability & Ingestion)
- **Jackline Mboya** - ML / Data Privacy Engineer
- **Patrick K. Kariuki** - Business Analyst
- **Abdirahim Osman** - QA & Deployment Lead

## Week 11 submission portfolio

Add these files to the repository root so the submitted GitHub link contains the required evidence:

- `Week11_Business_Case_Stanley_Metone.pdf`
- `CV_Stanley_Metone_DataAnalyst.pdf`
- `Week11_Pitch_Deck.pdf`
- `Week11_Pitch_Deck.pptx` *(editable source; optional but recommended)*
- `Week11_Self_Reflection_Stanley_Metone.pdf`
- `Week11_LinkedIn_Optimization_Stanley_Metone.md`
- `Week11_Mock_Interview_Guide_Stanley_Metone.pdf` *(preparation support)*
- `Week11_Pitch_Speaker_Notes_Stanley_Metone.md` *(preparation support)*

### LMS-only/manual items

The following must come from Stanley's real accounts/recordings and should not be fabricated:

- LinkedIn profile header + About screenshot after updating the profile
- LinkedIn URL: https://www.linkedin.com/in/stanley-metone/
- `Week11_Mock_Interview.mp4` - Stanley's recorded mock interview or approved highlights reel
- Hackathon pitch recording required by the team submission

## Portfolio links

- **Stanley Metone GitHub:** https://github.com/stanley-metone
- **Project repository:** https://github.com/LosBandidox/KPC-Secure-AI-Tooling-Pipeline
- **LinkedIn:** https://www.linkedin.com/in/stanley-metone/

## Responsible-use note

This project is a hackathon/capstone prototype. Before production use, validate the solution against KPC's approved data-governance, cybersecurity, infrastructure, operational-safety and procurement requirements. Do not interpret synthetic-data performance or planning ROI as production evidence.
