# Mode: tracker - Applications Tracker

Read and show `data/applications.md`.

**Tracker format:**
```markdown
| # | Date | Company | Role | Score | Status | PDF | Report |
```

Possible statuses:
`Evaluated` -> `Applied` -> `Responded` -> `Contact` -> `Interview` -> `Offer` / `Rejected` / `Discarded` / `SKIP`

- `Applied` = the candidate submitted the application
- `Responded` = a recruiter or company reached out and the candidate replied
- `Contact` = the candidate proactively contacted someone at the company

If the user asks to update a status, edit the matching row.

Also show stats:
- total applications
- counts by status
- average score
- % with PDF generated
- % with report generated
