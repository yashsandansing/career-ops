# Mode: oferta - Full Evaluation A-G

When the candidate pastes a job posting as text or URL, always deliver all 7 blocks.

## Step 0 - Archetype detection

Classify the role into one of the 6 archetypes from `_shared.md`. If it is hybrid, name the 2 closest ones. This determines:
- which proof points to prioritize in Block B
- how to rewrite the summary in Block E
- which STAR stories to prepare in Block F

## Block A - Role summary

Include:
- detected archetype
- domain
- function
- seniority
- remote model
- team size if mentioned
- one-sentence TL;DR

## Block B - CV match

Read `cv.md`. Create a table mapping JD requirements to exact CV lines.

Adapt by archetype:
- FDE -> fast delivery and client-facing proof points
- SA -> systems design and integrations
- PM -> product discovery and metrics
- LLMOps -> evals, observability, pipelines
- Agentic -> multi-agent, HITL, orchestration
- Transformation -> change management and adoption

Include a **gaps** section with mitigation for each gap:
1. hard blocker or nice-to-have?
2. is there adjacent experience?
3. is there a project that covers the gap?
4. concrete mitigation plan

## Block C - Level and strategy

1. detected level in the JD vs the candidate's natural level
2. a plan to "sell senior without lying"
3. a plan for "if they downlevel me"

## Block D - Comp and demand

Use WebSearch for:
- current salary data
- company compensation reputation
- demand trend for the role

Show a table with sources. If data is missing, say so.

## Block E - Personalization plan

| # | Section | Current State | Proposed Change | Why |
|---|---------|---------------|-----------------|-----|
| 1 | Summary | ... | ... | ... |

List the top 5 CV changes and top 5 LinkedIn changes.

## Block F - Interview plan

Create 6-10 STAR+R stories mapped to JD requirements.

| # | JD Requirement | STAR+R Story | S | T | A | R | Reflection |
|---|----------------|--------------|---|---|---|---|------------|

If `interview-prep/story-bank.md` exists, reuse existing stories when possible and append new ones when needed.

Archetype framing:
- FDE -> delivery speed and client-facing stories
- SA -> architecture decisions
- PM -> discovery and trade-offs
- LLMOps -> metrics, evals, production hardening
- Agentic -> orchestration, error handling, HITL
- Transformation -> adoption and organizational change

Also include:
- one recommended case study
- red-flag questions and how to answer them

## Block G - Posting legitimacy

Analyze whether this looks like a real, active opening.

### Signals to analyze

**1. Posting freshness**
- posting date or "X days ago"
- apply button state
- whether the URL redirected to a generic page

**2. Description quality**
- specificity of tech/tools/frameworks
- org and reporting context
- realism of requirements
- clarity of expected scope
- salary transparency
- role-specific vs boilerplate ratio
- contradictions

**3. Company hiring signals**
- layoffs
- hiring freeze
- whether negative news affects the same org as this role

**4. Reposting detection**
- whether the same company + similar role appeared before

**5. Role market context**
- does the role make sense for this company?
- is the seniority level one that naturally takes longer to fill?

### Output format

**Assessment:** one of:
- High Confidence
- Proceed with Caution
- Suspicious

Include a signals table and context notes.

---

## Post-evaluation

Always do the following after Blocks A-G:

### 1. Save the report

Save the full evaluation to `reports/{###}-{company-slug}-{YYYY-MM-DD}.md`.

Report format:

```markdown
# Evaluation: {Company} - {Role}

**Date:** {YYYY-MM-DD}
**Archetype:** {detected}
**Score:** {X/5}
**Legitimacy:** {High Confidence | Proceed with Caution | Suspicious}
**PDF:** {path or pending}

---

## A) Role Summary
...
```

### 2. Register in the tracker

Always register the evaluation in `data/applications.md` with:
- next sequential number
- date
- company
- role
- score
- status
- PDF indicator
- report link
