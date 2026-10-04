# FDNY Tier 3 Pension Calculator

A public, single-page pension calculator for FDNY uniformed members in Tier 3 (appointed on or after July 1, 2009). The owner is the GitHub user `emmetsmeehan`. He built it to share with fellow members, who will use it when thinking about real retirement decisions, so the numbers and the wording have to be right.

This repository is the source of truth. The calculator used to live on a claude.ai page; that copy is retired.

## How the site is published

- The whole site is `index.html`: markup, CSS and JavaScript in one file. The only outside resource is the Source Sans 3 font from Google Fonts.
- GitHub Pages serves the `main` branch from the repository root. A push to `main` goes live in a minute or two at https://emmetsmeehan.github.io/tier3-pension/
- `.nojekyll` stops GitHub from processing the files. Leave it in place.
- The owner's standing instruction (Oct. 2026): once a change he asked for is tested, merge it to `main` so it goes live, unless he says otherwise.

## How to work on it

- Make small, targeted edits. Do not rewrite the page to make a change.
- Keep it working on phones. After a layout change, check about 390px and 320px wide. The tight spots are the service slider's stop buttons (22 and 22½ sit side by side on the lower row) and the comparison table.
- Every color is a CSS token defined for light and dark themes at the top of the stylesheet. Use the tokens; do not hard-code colors.
- After changing the script, confirm it parses, and test the affected formula at its boundaries (for example 24 years 11 months, 25, 29, 30 and 35 years for the longevity bonus).
- Check pay figures and legal claims against primary sources before putting them on the page. If something could not be verified, tell the owner plainly.
- When a rule changes, update every place that describes it: the hint next to the input, the FAQ, and "How it's calculated".
- The owner wants direct answers. When a request conflicts with a rule below, or the right reading is unclear, ask him before changing the math.

## Look and wording

The owner asked for a plain look: one sans-serif family, sentence-case labels, no monospace text, no small uppercase letter-spaced labels, no colored stripes on card edges, squared corners. Keep to that. Do not add any mention of AI or Claude to the page.

The page carries a disclaimer in two places (under the intro and at the bottom): it is unofficial, not affiliated with the FDNY or the NYC Fire Pension Fund, and actual numbers may vary. Keep both.

## Calculation rules

These are the rules the page implements. They are the owner's decisions; do not change them without asking.

- **Pension:** 50% of final average salary (FAS) at 20 or more years of service, under S4727 (Chapter 692 of the Laws of 2025, signed Dec. 19, 2025). Tier 3 is capped at 50%.
- **FAS:** the best 3 consecutive calendar years. A year counts only up to 110% of the average of the two years before it. A typed-in FAS is grown by the pay-growth assumption for each year past 20.
- **Escalation:** the lesser of CPI or 3%, compounding, on the whole pension. (The bills' fiscal notes say CPI above 3% is banked and applied in later years; the page does not model that yet. Raised with the owner Oct. 4, 2026.) Full escalation needs collection to start at the 25-year mark. The rate drops by 1/36 for each month collection starts earlier, so 22 years or less gets none. The page assumes collection starts at retirement. It used to offer deferring collection to the 25-year date; the owner had it removed (Oct. 2026) because the option was confusing.
- **COLA:** without full escalation, the pension gets the greater of escalation or COLA. COLA is half of CPI (minimum 1%, maximum 3%) on the first $18,000 only, starting at age 62 after 5 years retired or age 55 after 10 years retired. COLA reduces the VSF until 62.
- **VSF and banked variable:** the Variable Supplements Fund pays $12,000 a year to service retirees with 20 or more years. Each year worked past 20 banks one payment, paid as a lump sum at retirement. The $12,000 is fixed in the code; the owner had the editable assumption removed.
- **Social Security offset:** at 62 the pension drops by 50% of the primary Social Security benefit.
- **3/4s disability:** a large "3/4s (Disabled)" checkbox above the service slider. From the Fire Pension Fund's Tier 3 Enhanced Summary Plan Description (UFA copy dated April 18, 2024, PDF from the owner Oct. 4, 2026): accident disability pays 75% of FAS at any service, generally not taxed; no escalation (COLA only, after 5 years retired at any age); no VSF or banked variable; no Social Security offset; health insurance regardless of service; no workers' comp offset mentioned. The owner decided: no longevity bonus on disability, no 20/22½/25 comparison (the table, escalation section, proposed-bills box and Social Security field are hidden), slider runs from 1 year with stops every 5 years. Ordinary (non-line-of-duty) disability is not modeled. A typed FAS is not shrunk for service under 20 years.
- **Earnings import:** the FAS worksheet takes a copy-and-paste of the NYCAPS ESS "Tax Summary for All Years" table (Pay and Tax Information tile, then Pay and Tax Information > Tax Summary (W-2, 1127 & 1095C)). It uses the Medicare Wages column (the third number after the year), skips the current unfinished year, and runs entirely in the browser. Lines with a year and one number are also accepted. If ESS changes its menu names or columns, update the steps on the page and the parser.
- **Service slider:** 20 to 35 years, with quick stops at 20, 22, 22½, 25, 30 and 35. The readout shows years on one line and months underneath ("0 months" on whole years), so its width doesn't change as the slider moves.

### Proposed bills (not law)

The form has a "Proposed bills, not yet law" box. "Bills in Albany" is a separate view in the same file, opened by a button under the intro (and a link in the proposed-bills box) at `#bills`, with a back link to the calculator; the browser back button works too. It summarizes each bill with a progress bar, status, and a "Try it in the calculator" link that sets that option. The owner asked for it to be off the main page so the calculator isn't overwhelming. Bill text for the Senate versions was read from PDFs the owner downloaded on Oct. 4, 2026.

- **Full escalation at** (default 25 years, current law). Options:
  - 23 years, S9204 (Jackson) / A11269 (Pheffer Amato): full escalation at 23 years, 1/36 less for each month short, so partial after 20 years and none at 20 or less.
  - 20 years, S9203 / A10254: full escalation at 20 years.
- **25-year longevity bonus** (checkbox on by default), in two versions. Both add section 15-110.1 to the NYC Administrative Code, so only one could pass as written.
  - Steps (default), S9306 / A10438: 5% of the rank's top pay at 25 years, 10% at 30, 15% at 35. Nothing extra in between.
  - +1% a year, S9202 / A10392: 5% at 25 plus 1% for each year past 25, capped at 15% at 35.
- The fiscal notes apply the bonus straight to FAS for Tier 3, without the 110% limit. The page does the same.
- **Status when last checked (Oct. 4, 2026), from nysenate.gov screenshots:** S9202, S9203, S9204 and S9306 are in Senate Civil Service and Pensions; A11269 is in Assembly Governmental Employees. None has passed either house. A10438 was referred to Assembly Governmental Employees on Mar. 6, 2026 (earlier check). The Assembly status of A10254 and A10392 has not been confirmed from a primary source, so the page does not state it.
- When any status changes, update the Bills in Albany view (bar, status line, "last checked" date), the hints in the form, the FAQ and "How it's calculated". If a bill becomes law, its option becomes the default and the wording changes from proposed to law.

### S4727

50% at 20 years: Chapter 692 of the Laws of 2025, signed Dec. 19, 2025. Shown in Bills in Albany as law.

### Rank top pay

The bonus uses top-step annual **base** salary for the rank, with no longevity pay, night differential or overtime:

| Rank | Top-step base | Source |
|---|---|---|
| FF | $109,352 | UFA rate schedule effective Aug. 1, 2024 |
| LT | $140,212 | UFOA rate schedule effective July 31, 2025 |
| CAPT | $160,941 | UFOA rate schedule effective July 31, 2025 |
| BC | $209,563 | UFOA rate schedule effective July 31, 2025 |

The bill's fiscal note uses higher figures (FF $140,392, LT $157,751, CAPT $179,842, Chiefs $255,863), which are 2024 salaries increased for assumed inflation. The owner chose to keep base-only figures because he does not want to assume inflation. The page says the bill's wording could come out higher than base salary, and members can type over the figure.

The rate schedule PDFs are not in this repository. When a new contract or schedule comes out, ask the owner for it and update the table above along with the page.

## Open items

- **Question box.** Live: a "Questions or suggestions" section after the FAQ posts to Web3Forms (`ASK_KEY` near the end of the script; the key is public by design). Messages go to the address the owner registered with Web3Forms. The FAQ answer about nothing being sent mentions the box.
- **Domain name.** He wants a cleaner address than the github.io one. Not bought yet.
- **Credit.** Whether the page names him or stays anonymous is undecided. Do not add his name to the page until he says so.
- **Phone check.** The live site has not been checked on an actual phone since the move to GitHub Pages.
