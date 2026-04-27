# Mode: deep - Deep Research Prompt

Generate a structured prompt for Perplexity, Claude, or ChatGPT across 6 axes:

```
## Deep Research: [Company] - [Role]

Context: I am evaluating an application for [role] at [company]. I need actionable information for interview prep.

### 1. AI Strategy
- What products or features use AI/ML?
- What is their AI stack?
- Do they have an engineering blog?
- What papers or talks have they published?

### 2. Recent moves (last 6 months)
- Relevant AI/ML/product hires
- Acquisitions or partnerships
- Product launches or pivots
- Funding rounds or leadership changes

### 3. Engineering culture
- How do they ship?
- Monorepo or multi-repo?
- What languages and frameworks do they use?
- Remote-first or office-first?
- Glassdoor/Blind signals about engineering culture

### 4. Likely challenges
- What scaling problems do they likely have?
- Reliability, cost, latency challenges?
- Are they migrating anything?
- What pain points show up in reviews?

### 5. Competitors and differentiation
- Who are the main competitors?
- What is the moat?
- How do they position themselves?

### 6. Candidate angle
Given my profile (read from cv.md and profile.yml):
- What unique value would I bring to this team?
- Which of my projects are most relevant?
- What story should I tell in the interview?
```

Customize each section using the specific evaluated role context.
