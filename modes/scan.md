# Mode: scan - Portal Scanner

Scan configured job portals, filter by title relevance, and add new offers to the pipeline for later evaluation.

## Recommended execution

Run as a subagent to save main-context space.

## Configuration

Read `portals.yml`, which contains:
- `search_queries`
- `tracked_companies`
- `title_filter`

## Discovery strategy (3 levels)

### Level 1 - Direct Playwright scan

For each company in `tracked_companies`, navigate to `careers_url`, read all visible listings, and extract title + URL.

### Level 2 - ATS APIs / feeds

Use structured API or feed responses when available:
- Greenhouse
- Ashby
- BambooHR
- Lever
- Teamtailor
- Workday

### Level 3 - WebSearch

Use `search_queries` with `site:` filters for broad discovery.

## Workflow

1. Read `portals.yml`
2. Read `data/scan-history.tsv`
3. Read dedup sources from `data/applications.md` and `data/pipeline.md`
4. Run Level 1
5. Run Level 2
6. Run Level 3
7. Filter by title relevance
8. Deduplicate
9. Verify WebSearch liveness before adding to pipeline
10. Add verified new offers to `pipeline.md`
11. Record results in `scan-history.tsv`

## Title filtering

Use `title_filter`:
- at least one positive keyword must match
- no negative keyword may match
- `seniority_boost` raises priority but is not required

## Deduplication sources

- exact URLs in `scan-history.tsv`
- normalized company + role pairs in `applications.md`
- exact URLs already in `pipeline.md`

## Liveness verification for WebSearch results

Because WebSearch results may be stale, verify each new Level 3 result with Playwright before adding it.

Classify as:
- **Active**
- **Expired**

If navigation fails, mark as `skipped_expired` and continue.

## Output summary

```
Portal Scan - {YYYY-MM-DD}
Queries run: N
Offers found: N total
Filtered by title: N
Duplicates: N
Expired discarded: N
New offers added to pipeline.md: N
```

## careers_url management

Every tracked company should have a `careers_url`.

If one is missing:
1. infer from the platform pattern
2. if that fails, run a quick WebSearch
3. verify with Playwright
4. save the discovered URL back to `portals.yml`

## portals.yml maintenance

- always save `careers_url` for new companies
- add new queries when useful
- disable noisy queries when needed
- adjust title-filter keywords as the target roles evolve
- periodically re-check `careers_url` values
