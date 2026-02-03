# Case Study: AI-Driven Survey Analytics & ETL Automation

## TL;DR
Designed and delivered a metadata-driven, AI-assisted system to automate survey variable classification and ETL maintenance.  
The solution reduced manual intervention, improved robustness to survey changes, and enabled faster, more reliable analytics delivery through a **multi-agent architecture with human-in-the-loop validation**.

---

## Context & Problem Statement

Survey-based analytics pipelines are highly sensitive to change:
- Question wording evolves
- Variables are added, removed, or re-coded
- KPIs are derived differently across waves

The existing process relied on:
- Manually maintained ETL mappings (Excel-based)
- Hard-coded variable logic in Python/SQL pipelines
- Human interpretation of SPSS files for each update

This resulted in:
- High operational overhead
- Increased risk of silent data errors
- Slow turnaround when survey instruments changed

---

## Objectives & Constraints

### Objectives
- Reduce manual effort required to update ETL mappings
- Increase resilience to survey version changes
- Preserve analyst control over KPI definitions
- Improve transparency and auditability of changes

### Constraints
- **No access to respondent-level data** (metadata only)
- Variable names are inconsistent; **labels are the primary signal**
- No fixed mapping between question codes and KPIs
- Core ETL pipelines are stable and should not be frequently modified
- Solution must scale across multiple surveys and versions

---

## Solution Overview

I designed a **metadata-driven AI system** that:
1. Reads SPSS **variable-level metadata** (labels, values, types)
2. Uses LLM-based agents to reason over variable intent
3. Classifies variables into conceptual KPI groups
4. Detects differences between survey versions or changelogs
5. Proposes ETL and mapping updates for human review
6. Produces structured outputs consumable by existing pipelines

The system acts as a **decision support layer**, not an autonomous ETL writer.

---

## High-Level Architecture

```

SPSS Metadata
│
▼
Metadata Extraction
│
▼
Classification Agent
│
├── KPI Alignment Agent
│
├── Change Detection Agent
│
└── Validation Agent
│
▼
Proposed Mapping Updates
│
▼
Human Review & Approval
│
▼
ETL Configuration (Excel / Config Files)
│
▼
Existing Python & SQL Pipelines

````

---

## Key Design Decisions

### 1. Metadata-Only Reasoning
The system intentionally operates **only on variable metadata**:
- Variable labels
- Value labels
- Question context

This:
- Eliminates data privacy concerns
- Improves generalizability
- Forces semantic reasoning over statistical shortcuts

---

### 2. Agent Specialization over Monolithic Prompts
Instead of a single “do everything” prompt:
- **Classification Agent** → assigns conceptual groupings (e.g. awareness, consideration, usage)
- **Change Detection Agent** → compares versions and flags deltas
- **Validation Agent** → checks consistency with historical mappings and rules

This improved:
- Interpretability
- Debuggability
- Targeted human intervention

---

### 3. Human-in-the-Loop as a First-Class Feature
AI outputs are **proposals**, not automatic changes.

Analysts can:
- Accept, reject, or modify mappings
- Override classifications
- Feed corrections back into the system

This preserves domain expertise while removing repetitive work.

---

### 4. Non-Disruptive Integration
The system outputs:
- Structured mapping tables
- Change summaries
- Rationale for each suggestion

Existing ETL code remains untouched.

---

## Example Outputs (Simplified)

**Variable Classification**
```json
{
  "variable": "Q12_3",
  "label": "Likelihood to recommend the brand",
  "proposed_kpi": "Advocacy",
  "confidence": 0.92,
  "reasoning": "Measures recommendation intent, aligns with NPS-style metrics"
}
````

**Change Detection**

```json
{
  "change_type": "label_modified",
  "previous_label": "Brand consideration",
  "new_label": "Likelihood to consider",
  "impact": "No KPI reassignment required"
}
```

---

## Impact

* Significant reduction in manual ETL maintenance effort
* Faster onboarding of new survey versions
* Lower risk of undetected KPI misclassification
* Improved transparency into why changes were made

Qualitatively, the system shifted the team from **reactive fixes** to **proactive review**.

---

## Risks & Mitigations

| Risk                             | Mitigation                                |
| -------------------------------- | ----------------------------------------- |
| Over-confident misclassification | Confidence thresholds + mandatory review  |
| Label ambiguity                  | Cross-variable context reasoning          |
| Analyst distrust                 | Explainable outputs + override capability |
| Model drift                      | Periodic review using historical mappings |

---

## What I Would Extend Next

* Feedback-loop learning from analyst overrides
* Confidence-based auto-approval for low-risk changes
* Integration with data quality checks post-ETL
* Versioned audit logs for governance and compliance

---

## My Role

* Problem framing and solution design
* System architecture and agent orchestration
* KPI framework alignment
* Delivery planning and stakeholder alignment
* Risk management and rollout strategy

---

## Key Takeaway

This project demonstrates how **AI can augment analytics operations** without compromising control, governance, or trust — by embedding intelligence *around* stable systems rather than constantly rewriting them.

```
