# Personal Spending Analyzer

A spreadsheet method and template for turning messy bank, card and Amazon exports into one clean, formula-driven budget. It's for anyone who has tried to budget and got stuck on the data before reaching the decisions.

> All data in this repo is **synthetic**. Every merchant, item and amount is invented. My real financial data never left my own files.

## The problem

My spending was spread across three places: a checking account, a credit card (yay points!) and Amazon orders. Each one exports in its own format, uses its own category labels, and is full of rows that aren't really spending: payroll, transfers between my own accounts, card payments, refunds, cancelled orders.

This is a classification problem, and a familiar one. Ops and policy teams deal with it all the time when outside taxonomies don't match their own, like IAB (Interactive Advertising Bureau) content categories vs. a platform's policy labels. The fix is the same in both places: map every outside label onto one consistent taxonomy, decide clearly what's in and out of scope. Handle edge cases manually, and keep an audit trail. Get the map wrong and the decisions downstream go wrong too. In ad policy, that means enforcement, brand safety and user safety. In a budget, a mislabeled transfer makes it look like you spent money you didn't.

## What it does

- **Separates real spending from noise.** It excludes income, internal transfers, card payments, P2P transfers, cash withdrawals and bank fees, and keeps every excluded row on an audit tab with the reason.
- **Prevents double counting.** Amazon charges are removed from the card file because the Amazon export itemizes them, and a card payment from checking isn't counted on top of the card's own charges. *The ops version: "I think I saw that same advertiser two tabs ago."*
- **Normalizes messy categories** through a single Category Map. Rename or merge a category once and every summary updates. *The ops version: retiring unused or misused labels, and taming the infamous "Other".*
- **Separates steady costs from one-offs** with a Category × Month grid, so budget targets come from real patterns.
- **Compares actual against target**, including a "Once a Year" column so annual fees don't distort the monthly picture.

## How I built it

**Tools**
- **Claude** (chat plus code execution) did the data work across four working sessions.
- **Python** (a programming language) with *pandas* (for cleaning tables) and *openpyxl* (for writing Excel files). Claude wrote and ran this code. I didn't write any.
- **Excel formulas**: SUMIFS, COUNTIFS, INDEX/MATCH and TEXT. Every total is a live formula, not a typed-in number.
- **LibreOffice**, a free spreadsheet program, used behind the scenes to recalculate formulas so the files show real values.

**What AI did vs. what I decided**

| Claude | Me |
|---|---|
| Explained each export file before changing anything | Decided what counts as "real spending", which was a personal judgment call |
| Proposed a category list and a consistent "Group - Detail" naming style | Edited categories by hand in Excel (95+ changes across two files) |
| Wrote the cleaning code and built the formula-driven workbooks | Gave numbered, itemized edit instructions round by round |
| Diffed my edited re-uploads and applied my changes automatically | Confirmed ambiguous transactions, e.g. what one recurring transfer really was |
| Raised method questions before building the combined file | Made the method calls: same 8 months for every source, average over all months, annual fees separate, merge near-duplicate categories |
| Checked that combined totals matched every source file | Set budget targets and added the "Once a Year" column |

**What I learned**
- **Explain first, build second.** Asking the AI to describe the data before touching it showed me what it could do, and surfaced patterns I didn't expect it to find.
- **Make the AI ask before it assumes.** The best session was the one where Claude raised four method questions instead of guessing. The [WDAI (Women Defining AI)](https://www.womendefiningai.org) lessons paid off :)
- **Numbered instructions plus confirmed counts** ("removed X rows, Y remain") is how I supervised AI work that would have taken me days to check line by line.
- **Edit in Excel, then let the AI find the differences.** I didn't have to describe my 62 manual changes. It diffed the files.
- **Always keep an untouched raw copy.** It's the audit trail, the same principle as in policy enforcement.

The full method, plus technical gotchas for anyone repeating this, is in [METHOD.md](METHOD.md).

## Try it / see it

- **[budget-template.xlsx](budget-template.xlsx)**: the finished workbook pattern, filled with three months of synthetic data. Open it in Excel or Google Sheets and start with the *How to Use* tab.
- **[sample-data/](sample-data/)**: the fake "messy" exports it was built from (checking, credit card, Amazon orders and returns), including the kinds of noise rows the method removes.

## Status & what's next

**Status:** Working. I built my real budget with this method across four sessions in September 2026, and this repo is the cleaned-up public version.

**Next:**
- Track actual vs. target month by month, not only as an average
- Turn the cleaning steps into reusable Python scripts that run on the sample data
- Add a short guide on writing prompts for this kind of task

## About me

Trust & Safety and ad policy leader learning to build with AI.
[LinkedIn](https://www.linkedin.com/in/YOUR-LINKEDIN-HANDLE)
