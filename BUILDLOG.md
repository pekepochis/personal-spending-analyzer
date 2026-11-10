# Build Log

### 2026-11-10
- Published this repo: my first public project. Rebuilt the workbook pattern with synthetic data so I could share the method without sharing my finances.
- Learned: the privacy scrub took real work. File names, tab names and notes all carried account identifiers. "Is it safe to publish?" is a T&S question I know well, just applied to my own data this time.
- Next: track actual vs. target month by month, and turn the cleaning steps into reusable scripts.

### 2026-09-30
- Cleaned the credit card file (806 rows → 558) and the Amazon export (191 items), then combined all three sources into one workbook.
- Learned: editing in Excel and having Claude diff my re-upload saved hours. It found my 62 changes without me listing them. Also, Claude asking me four method questions before building was the most useful thing it did all project.
- Surprised me: an Amazon date-format quirk silently dropped rows until we caught it. Silent failures are the dangerous ones.
- Next: add budget targets and an "Once a Year" column for annual costs.

### 2026-09-21
- First session: the checking account export. Started by asking Claude to explain the file before changing anything.
- Learned: of about 430 rows, only 40 were real expenses. Most of a bank export is money moving, not money spent. Deciding what counts was my call, not the AI's.
- Next: apply the same pattern to the credit card and Amazon data.
