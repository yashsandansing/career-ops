# Mode: pdf — ATS-Optimized PDF Generation

## Full pipeline

1. Read `cv.md` as source of truth
2. Ask the user for the JD if not in context (text or URL)
3. Extract 15-20 keywords from the JD
4. Detect JD language → CV language (EN default)
5. Detect company location → paper format: `letter`
6. Detect role archetype → adapt framing
7. Rewrite Professional Summary with JD keywords and the candidate's real exit narrative from `config/profile.yml`. Never use generic bridge phrases, evaluation language ("candidate", "strong fit", "match", "targeting", etc.), or claims not present in `cv.md`, `config/profile.yml`, `_profile.md`, or `article-digest.md`.
8. Select top 3-4 projects most relevant to the offer
9. For each experience role: maintain reverse chronological order (most recent first, always) → score each bullet by JD relevance → keep top 3 strictly (never more) → discard the rest. Prioritize bullets with quantifiable metrics and JD keywords.
10. Normalize job titles to match the JD title family where scope genuinely overlaps:
    - Filmic Technologies ALWAYS keeps the "(Intern)" tag regardless of JD type — e.g. "Software Engineer (Intern) | Filmic Technologies". Never drop it.
    - All other roles (Learno.AI, Signimus, Expertrons) are flexible — rename to match the JD title family when the actual work overlaps.
    - Mapping examples:
      - JD = MLE/AI Engineer → Filmic: unchanged | Learno.AI, Signimus, Expertrons: → Machine Learning Engineer
      - JD = SDE/Software Engineer → all roles: → Software Engineer
      - JD = Backend Engineer → all roles: → Backend Engineer
      - JD = Founding Engineer → Filmic & Signimus: → Founding Engineer | others: → Software Engineer / Machine Learning Engineer (whichever fits closer to the JD)
    - NEVER rename a title to something the candidate didn't do. Only change the label, not the content.
11. Inject keywords naturally into existing achievements (NEVER invent). Keep work-experience and project bullets close to the base CV's XYZ pattern: outcome/metric first where possible, then the implementation method. (XYZ pattern: Accomplished X, as measured by Y, by doing Z.)
12. Generate complete HTML from template + personalized content
13. Read `name` from `config/profile.yml` → normalize to kebab-case lowercase (e.g. "John Doe" → "john-doe") → `{candidate}`
14. Write HTML to `/tmp/cv-{candidate}-{company}.html`
15. Run: `node generate-pdf.mjs /tmp/cv-{candidate}-{company}.html output/cv-{candidate}-{company}-{YYYY-MM-DD}.pdf --format={letter|a4}`
15. Report: PDF path, page count, keyword coverage %

## ATS Rules (clean parsing)

- Single-column layout (no sidebars, no parallel columns)
- Standard headers: "Professional Summary", "Work Experience", "Education", "Skills", "Certifications", "Projects"
- No text in images/SVGs
- No critical info in PDF headers/footers (ATS ignores them)
- UTF-8, selectable text (not rasterized)
- No nested tables
- JD keywords distributed: Summary (top 5), first bullet of each role, Skills section

## PDF Design

- **Font**: DM Sans (all text, self-hosted in `fonts/`)
- **Name**: 35px uppercase bold, centered
- **Contact row**: 9px, centered, pipe-separated
- **Section headers**: 9.5px uppercase bold, black bottom border 1px
- **Body text**: 9.5px, line-height 1.35-1.45
- **Margins**: 0.35in top/bottom, 0.45in left/right
- **Colors**: black and white only — no accent colors

## Work Experience format (MANDATORY)

Each job header must be a single line:
```
Role | Company                                           Date
```
HTML structure:
```html
<div class="job-header">
  <span class="job-title-company">Role | Company</span>
  <span class="job-period">Mon YYYY - Mon YYYY</span>
</div>
```
- **Max 3 bullets per role, no exceptions**
- No separate location line
- No italic sub-title line

## Section order (optimized for "6-second recruiter scan")

1. Header (name centered, contact row)
2. Professional Summary (3-4 lines, keyword-dense, no mention of CGPA or university)
3. Work Experience (reverse chronological)
4. Projects (top 3-4 most relevant)
5. Education & Certifications
6. Skills (languages + technical)

## Keyword injection strategy (ethical, truth-based)

Examples of legitimate reformulation:
- JD says "RAG pipelines" and CV says "LLM workflows with retrieval" → change to "RAG pipeline design and LLM orchestration workflows"
- JD says "MLOps" and CV says "observability, evals, error handling" → change to "MLOps and observability: evals, error handling, cost monitoring"
- JD says "stakeholder management" and CV says "collaborated with team" → change to "stakeholder management across engineering, operations, and business"

**NEVER add skills the candidate doesn't have. Only reformulate real experience with the exact vocabulary of the JD.**

## HTML Template

Use the template in `cv-template.html`. Replace `{{...}}` placeholders with personalized content:

| Placeholder | Content |
|-------------|---------|
| `{{LANG}}` | `en` or `es` |
| `{{PAGE_WIDTH}}` | `8.5in` (letter) or `210mm` (A4) |
| `{{NAME}}` | (from profile.yml) |
| `{{PHONE}}` | (from profile.yml — candidate.phone) |
| `{{EMAIL}}` | (from profile.yml) |
| `{{LINKEDIN_URL}}` | (from profile.yml) |
| `{{LINKEDIN_DISPLAY}}` | (from profile.yml) |
| `{{PORTFOLIO_URL}}` | (from profile.yml) |
| `{{PORTFOLIO_DISPLAY}}` | (from profile.yml) |
| `{{GITHUB_URL}}` | `https://` + profile.yml candidate.github |
| `{{GITHUB_DISPLAY}}` | profile.yml candidate.github (without https://) |
| `{{LOCATION}}` | (from profile.yml) |
| `{{SECTION_SUMMARY}}` | Professional Summary |
| `{{SUMMARY_TEXT}}` | Personalized summary with keywords |
| `{{SECTION_EXPERIENCE}}` | Work Experience |
| `{{EXPERIENCE}}` | HTML of each role with reordered bullets |
| `{{SECTION_PROJECTS}}` | Projects |
| `{{PROJECTS}}` | HTML of top 3-4 projects |
| `{{SECTION_EDUCATION}}` | Education |
| `{{EDUCATION}}` | HTML of education |
| `{{SECTION_CERTIFICATIONS}}` | Certifications |
| `{{CERTIFICATIONS}}` | HTML of certifications |
| `{{SECTION_SKILLS}}` | Skills |
| `{{SKILLS}}` | HTML of skills |

## Canva CV Generation (optional)

If `config/profile.yml` has `canva_resume_design_id` set, offer the user a choice before generating:
- **"HTML/PDF (fast, ATS-optimized)"** — existing flow above
- **"Canva CV (visual, design-preserving)"** — new flow below

If the user has no `canva_resume_design_id`, skip this prompt and use the HTML/PDF flow.

### Canva workflow

#### Step 1 — Duplicate the base design

a. `export-design` the base design (using `canva_resume_design_id`) as PDF → get download URL
b. `import-design-from-url` using that download URL → creates a new editable design (the duplicate)
c. Note the new `design_id` for the duplicate

#### Step 2 — Read the design structure

a. `get-design-content` on the new design → returns all text elements (richtexts) with their content
b. Map text elements to CV sections by content matching:
   - Look for the candidate's name → header section
   - Look for "Summary" or "Professional Summary" → summary section
   - Look for company names from cv.md → experience sections
   - Look for degree/school names → education section
   - Look for skill keywords → skills section
c. If mapping fails, show the user what was found and ask for guidance

#### Step 3 — Generate tailored content

Same content generation as the HTML flow (Steps 1-11 above):
- Rewrite Professional Summary with JD keywords + exit narrative
- Reorder experience bullets by JD relevance
- Select top competencies from JD requirements
- Inject keywords naturally (NEVER invent)

**IMPORTANT — Character budget rule:** Each replacement text MUST be approximately the same length as the original text it replaces (within ±15% character count). If tailored content is longer, condense it. The Canva design has fixed-size text boxes — longer text causes overlapping with adjacent elements. Count the characters in each original element from Step 2 and enforce this budget when generating replacements.

#### Step 4 — Apply edits

a. `start-editing-transaction` on the duplicate design
b. `perform-editing-operations` with `find_and_replace_text` for each section:
   - Replace summary text with tailored summary
   - Replace each experience bullet with reordered/rewritten bullets
   - Replace competency/skills text with JD-matched terms
   - Replace project descriptions with top relevant projects
c. **Reflow layout after text replacement:**
   After applying all text replacements, the text boxes auto-resize but neighboring elements stay in place. This causes uneven spacing between work experience sections. Fix this:
   1. Read the updated element positions and dimensions from the `perform-editing-operations` response
   2. For each work experience section (top to bottom), calculate where the bullets text box ends: `end_y = top + height`
   3. The next section's header should start at `end_y + consistent_gap` (use the original gap from the template, typically ~30px)
   4. Use `position_element` to move the next section's date, company name, role title, and bullets elements to maintain even spacing
   5. Repeat for all work experience sections
d. **Verify layout before commit:**
   - `get-design-thumbnail` with the transaction_id and page_index=1
   - Visually inspect the thumbnail for: text overlapping, uneven spacing, text cut off, text too small
   - If issues remain, adjust with `position_element`, `resize_element`, or `format_text`
   - Repeat until layout is clean
d. Show the user the final preview and ask for approval
e. `commit-editing-transaction` to save (ONLY after user approval)

#### Step 5 — Export and download PDF

a. `export-design` the duplicate as PDF (format: a4 or letter based on JD location)
b. **IMMEDIATELY** download the PDF using Bash:
   ```bash
   curl -sL -o "output/cv-{candidate}-{company}-canva-{YYYY-MM-DD}.pdf" "{download_url}"
   ```
   The export URL is a pre-signed S3 link that expires in ~2 hours. Download it right away.
c. Verify the download:
   ```bash
   file output/cv-{candidate}-{company}-canva-{YYYY-MM-DD}.pdf
   ```
   Must show "PDF document". If it shows XML or HTML, the URL expired — re-export and retry.
d. Report: PDF path, file size, Canva design URL (for manual tweaking)

#### Error handling

- If `import-design-from-url` fails → fall back to HTML/PDF pipeline with message
- If text elements can't be mapped → warn user, show what was found, ask for manual mapping
- If `find_and_replace_text` finds no matches → try broader substring matching
- Always provide the Canva design URL so the user can edit manually if auto-edit fails

## Post-generation

Update tracker if the offer is already registered: change PDF from ❌ to ✅.
