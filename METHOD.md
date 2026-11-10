# Method

How the workbook was built, step by step, so anyone can repeat it with their own exports. Examples refer to the synthetic sample data.

## 1. Define what counts as spending

The core rule: **keep only real out-of-pocket spending for the period.** Everything else is excluded and logged with a reason on the *Excluded* tab.

| Excluded | Why |
|---|---|
| Payroll and other income | Not spending |
| Transfers between your own accounts | Money moving, not money spent |
| Credit card payments from checking | The card is analyzed on its own, so counting the payment too would count everything twice |
| P2P transfers (Zelle, Venmo) | Often reimbursements or shared costs. Review them case by case |
| Cash withdrawals | Can't be categorized |
| Bank fees, tax items, loan and brokerage flows | Outside the scope of a spending budget |
| Cancelled and digital Amazon orders | Never charged, or out of scope |
| Amazon charges on the card | Already itemized in the Amazon file |

Some rows need a human. If you don't know what a recurring transaction is, ask the AI to explain the money flow, then you decide.

## 2. Clean each source separately

Each source gets the same five-tab pattern before anything is combined:

1. **Transactions**: cleaned working data. Editable columns (like Category) are highlighted.
2. **Category Map**: a lookup table. Summaries use INDEX/MATCH against it, so a rename happens in one place.
3. **Category Summary**: SUMIF/COUNTIF by category.
4. **Monthly Summary**: SUMIFS by month, using a month key from `=TEXT(date,"YYYY-MM")`.
5. **Original or Excluded Transactions**: an untouched audit copy, never linked to any formula.

Use a consistent **"Group - Detail"** naming style for categories (e.g. `Auto - Insurance`, `Shopping - Clothes`) so similar things sort together.

## 3. Make the method decisions before combining

Write them down on the *Settings* tab:

1. **Same period for every source.** Drop partial months so averages compare like with like.
2. **Merge near-duplicate categories.** Typos (`Shoping/clothes`), trailing spaces and alternate wording are merged in the Category Map.
3. **Monthly average = total ÷ months in period**, not ÷ months with spending. Otherwise a single car repair looks like a monthly cost.
4. **Annual fees get their own category**, budgeted once a year rather than spread into monthly spending.
5. **Returns:** delete the original purchase row instead of adding a negative refund row (Amazon). Card refunds that come through as a credit stay as a negative amount in their category.

## 4. Combine and verify

- *All Transactions* is the **single source** for every formula in the combined workbook.
- **Check:** the combined total must equal the sum of the per-source totals.
- **Check:** no merchant appears in two sources (e.g. Amazon on the card *and* in the Amazon file).
- **Check:** no row shows `UNMAPPED` in the Budget Category column.

## 5. Set targets

Use *Category by Month* to see which categories are steady and which are one-off, then enter targets in *Category Summary*: a monthly target for regular costs and a "Once a Year" amount for annual ones.

---

## Technical gotchas (for anyone repeating this with AI)

- **Formulas need a recalculation step.** Files written by Python (openpyxl) contain formulas but no computed values until a spreadsheet program recalculates them. We used a LibreOffice-based recalc script, then read the values back to check them.
- **Amazon's data export** is a zip file. Physical orders are in `Your Amazon Orders/Order History.csv`. Returns are in separate files, and refund amounts only appear there. Grocery orders sit in the same file, marked by a different `Website` value.
- **Mixed date formats.** Newer Amazon rows use ISO 8601 dates (`2026-02-14T10:00:00Z`). Parse with `pd.to_datetime(..., format='ISO8601', utc=True)`. Using `errors='coerce'` **silently drops** those rows.
- **Amounts arrive as text** (`"$12.99"`). Strip non-numeric characters before converting.
- **Deleting rows means updating every formula range** and the filter range, then recalculating. The template uses a fixed ceiling (row 1000) to leave room.
- **Diffing an edited re-upload:** a composite key of (formatted date, description, amount rounded to 2 decimals) matched rows reliably.
- **Save versioned checkpoints** between edit rounds (e.g. `categorized.json`, `categorized_v2.json`) so you can always go back.
