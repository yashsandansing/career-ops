# Mode: pipeline - URL Inbox

Process the accumulated job URLs in `data/pipeline.md`.

## Workflow

1. Read `data/pipeline.md` and find `- [ ]` items in the "Pending" section
2. For each pending URL:
  a. Calculate the next sequential `REPORT_NUM`
   b. Extract the JD using Playwright -> WebFetch
   c. If the URL is not accessible, mark it as `- [!]` with a note and continue
   d. Run the full auto-pipeline
   e. Move it from "Pending" to "Processed"
3. If there are 3+ pending URLs, launch agents in parallel
4. At the end, show a summary table

```
| # | Company | Role | Score | PDF | Recommended Action |
```

## pipeline.md format

```markdown
## Pending
- [ ] https://jobs.example.com/posting/123
- [ ] https://boards.greenhouse.io/company/jobs/456 | Company Inc | Senior PM
- [!] https://private.url/job - Error: login required

## Processed
- [x] #143 | https://jobs.example.com/posting/789 | Acme Corp | AI PM | 4.2/5 | PDF ✅
```

## Smart JD extraction

1. Playwright
2. WebFetch

Special cases:

- LinkedIn may require login
- PDFs should be read directly
- `local:` paths should load local files

## Automatic numbering

1. List all files in `reports/`
2. Extract the numeric prefix
3. Use max + 1

## Source synchronization

Before processing any URL, run:

```bash
node cv-sync-check.mjs
```

If there is drift, warn the user before continuing.