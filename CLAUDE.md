# FDNY Tier 3 Pension Calculator

A public, single-page pension calculator for FDNY uniformed members in Tier 3 (appointed on or after July 1, 2009). The owner is the GitHub user `emmetsmeehan`. He built it to share with fellow members, who will use it when thinking about real retirement decisions, so the numbers and the wording have to be right.

This repository is the source of truth. The calculator used to live on a claude.ai page; that copy is retired.

## How the site is published

- The whole site is `index.html`: markup, CSS and JavaScript in one file. The only outside resource is the Source Sans 3 font from Google Fonts.
- GitHub Pages serves the `main` branch from the repository root. A push to `main` goes live in a minute or two at https://emmetsmeehan.github.io/tier3-pension/
- `.nojekyll` stops GitHub from processing the files. Leave it in place.

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
- **Escalation:** the lesser of CPI or 3%, compounding, on the whole pension. Full escalation needs collection to start at the 25-year mark. The rate drops by 1/36 for each month collection starts earlier, so 22 years or less gets none. A member can retire earlier and defer collection to the 25-year date.
- **COLA:** without full escalation, the pension gets the greater of escalation or COLA. COLA is half of CPI (minimum 1%, maximum 3%) on the first $18,000 only, starting at age 62 after 5 years retired or age 55 after 10 years retired. COLA reduces the VSF until 62.
- **VSF and banked variable:** the Variable Supplements Fund pays $12,000 a year to service retirees with 20 or more years. Each year worked past 20 banks one payment, paid as a lump sum at retirement.
- **Social Security offset:** at 62 the pension drops by 50% of the primary Social Security benefit.
- **Service slider:** 20 to 35 years, with quick stops at 20, 22, 22½, 25, 30 and 35.

### 25-year longevity bonus (proposed, not law)

This models a bill, A10438 / S9306, that would add section 15-110.1 to the NYC Administrative Code. The page labels it "proposed bill, not yet law", and the checkbox that includes it is on by default.

- The bill adds a percentage of "the highest grade of pay under the applicable collective bargaining agreement" of the rank the member retires in to the salary used for the pension: **5% at 25 years, 10% at 30 years, 15% at 35 years**. These are steps. There is no per-year increase in between, so 26 through 29 years stay at 5%.
- The bill's fiscal note applies the increase straight to FAS for Tier 3, without the 110% limit. The page does the same.
- Status when last checked (Oct. 4, 2026): S9306 was referred to Senate Civil Service and Pensions on Feb. 27, 2026, and A10438 to Assembly Governmental Employees on Mar. 6, 2026. Neither had moved. Re-check before any update that touches the bonus: https://www.nysenate.gov/legislation/bills/2025/S9306
- If the bill passes, is amended, or dies, tell the owner and update the page wording and the FAQ answer.

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

- **Question box.** The owner wants a text box with a Submit button that emails him what people write. It needs a form-to-email service: he signs up with the address he wants questions sent to and supplies the form's endpoint. Then build the box on the page, with clear sent and failed states.
- **Domain name.** He wants a cleaner address than the github.io one. Not bought yet.
- **Credit.** Whether the page names him or stays anonymous is undecided. Do not add his name to the page until he says so.
- **Phone check.** The live site has not been checked on an actual phone since the move to GitHub Pages.
