# My Fire Pension: FDNY Tier 2 and Tier 3 pension calculators

A public, single-page guide and pension calculator for FDNY uniformed members in Tier 3 (appointed on or after July 1, 2009). The owner is the GitHub user `emmetsmeehan`. He built it to share with fellow members, who will use it when thinking about real retirement decisions, so the numbers and the wording have to be right.

This repository is the source of truth. The calculator used to live on a claude.ai page; that copy is retired.

## How the site is published

- Site layout (Oct. 11, 2026): `index.html` at the root is a small tier chooser (two big boxes, Tier 2 and Tier 3, then "How to tell" by appointment date, and the disclaimer). It also sends old Tier 3 links (`/#basics`, `/#bills`, `/#quick`, `/#personal`) to `tier3/`. The Tier 3 calculator is `tier3/index.html`; the Tier 2 calculator is `tier2/index.html`. Each calculator page is one file: markup, CSS and JavaScript. Each calculator's home screen starts with a "Want to see Tier 2/3 instead?" button (`.switch`) linking to the other tier. The rest of this file describes the Tier 3 page unless it says Tier 2.
- Domain move (Oct. 11, 2026): the site moved from `tier3pension.com` to `myfirepension.com` (`CNAME` changed the same day, after the owner set up DNS and verified the domain on GitHub). `tier3pension.com` is to redirect at Cloudflare (proxied records, Redirect Rules: `/tier2*` to `https://myfirepension.com/tier2/`, everything else to `https://myfirepension.com/tier3/`); the owner sets that up. Saved numbers don't carry over to a new domain.
- Each calculator page's outside resources: the Source Sans 3 font from Google Fonts, and Tesseract.js 5.1.1 from cdn.jsdelivr.net, loaded only when someone uploads a screenshot (it also fetches its worker, core and English data from jsdelivr). The owner approved Tesseract (Oct. 2026).
- Members' numbers are saved in the browser's localStorage under `fdny-tier3-v2`, on their own device only. "Clear my numbers" at the bottom of the personal calculator erases them. Nothing is sent anywhere except the question box.
- GitHub Pages serves the `main` branch from the repository root. A push to `main` goes live in a minute or two at https://myfirepension.com (custom domain since Oct. 11, 2026, set by the `CNAME` file; before that tier3pension.com, from Oct. 8, 2026; the old https://emmetsmeehan.github.io/tier3-pension/ address forwards there). Leave `CNAME` in place.
- Both domains were bought through Cloudflare Registrar, with DNS at Cloudflare. `myfirepension.com`: four A records for `@` (185.199.108.153, .109.153, .110.153, .111.153) and a CNAME `www` → `emmetsmeehan.github.io`, all "DNS only" (grey cloud), which GitHub needs to issue and renew the HTTPS certificate (the owner asked about Cloudflare's proxy warning; keep it grey). A TXT record verifies the domain to the owner's GitHub account, so no one else can point it at their own GitHub site; leave it in place. Turn on "Enforce HTTPS" in the repository's Pages settings once the certificate is issued. `tier3pension.com` keeps its own verification TXT record. Saved numbers in localStorage don't carry over between addresses, since browsers store them per address.
- Link previews (Oct. 11, 2026): each page (`index.html`, `tier2/`, `tier3/`) has Open Graph tags with absolute `https://myfirepension.com/` URLs and its own 1200×630 image at the root (`preview.png`, `preview-tier2.png`, `preview-tier3.png`), rendered from an HTML template in Source Sans 3. If the domain or the headline changes, update the tags and redo the images.
- `.nojekyll` stops GitHub from processing the files. Leave it in place.
- The owner's standing instruction (Oct. 2026): once a change he asked for is tested, merge it to `main` so it goes live, unless he says otherwise.

## How to work on it

- Make small, targeted edits. Do not rewrite the page to make a change. (The owner asked for one full redesign in Oct. 2026; that is done.)
- Most users are on phones and are not tech-savvy. Keep tap targets large, wording plain, and the answer close to the inputs. After a layout change, check about 390px and 320px wide, light and dark. The tight spots are the service slider's stop buttons (22 and 22½ sit side by side on the lower row), the earnings table and the comparison table.
- Every color is a CSS token defined for light and dark themes at the top of the stylesheet. Use the tokens; do not hard-code colors.
- After changing the script, confirm it parses, and test the affected formula at its boundaries (for example 24 years 11 months, 25, 29, 30 and 35 years for the longevity bonus).
- Check pay figures and legal claims against primary sources before putting them on the page. If something could not be verified, tell the owner plainly.
- When a rule changes, update every place that describes it: the hints in both calculators, the Pension basics answers, the fine print at the bottom of Pension basics, and the bill cards.
- The owner wants direct answers. When a request conflicts with a rule below, or the right reading is unclear, ask him before changing the math.

## Look and wording

The owner wants it modern and clean but simple: one sans-serif family (Source Sans 3), sentence-case labels, no monospace text, no small uppercase letter-spaced labels, no colored stripes on card edges. Slightly rounded corners and simple line icons are approved (Oct. 2026). FDNY red is the accent. Do not add any mention of AI or Claude to the page.

The disclaimer appears under the home intro and in the footer on every screen: unofficial, not affiliated with the FDNY or the NYC Fire Pension Fund, actual numbers may vary. Keep both.

## Page structure

Home (headline "Learn about your pension.", sub "Everything you need to know, and what you'll collect in retirement.") with four big buttons, then the question box. Each section opens as its own screen at a hash address with a sticky "Home" back button; the browser back button works:

- `#basics` Pension basics: four key-number tiles, tap-to-open questions in plain English (this replaced the old FAQ), and "The fine print: how the calculators work".
- `#bills` Bills in Albany: cards built from the `BILLS` array in the script. Each card: title, introduced date, summary, a vertical status track (green checks for done steps, an amber pulsing clock for the step in progress, grey dashed circles for later steps; a law gets a gold pen on "Signed into law"), "Read the full bill" links at the bottom, and "See what it means for you" which turns that bill on in the personal calculator.
- `#quick` Quick calculator: final average salary, years (slider), 3/4s checkbox, and "If these bills pass" checkboxes. Shows only the first-year monthly pension, with a note on whether it grows (none, partial, full escalation, at 3% inflation). A typed FAS is used exactly as typed, with no growth.
- `#personal` Your personal numbers (renamed from "Your personal calculator" at the owner's request, Oct. 11, 2026; in running text it's "the personal numbers page"): 1 About you (dates), 2 Your earnings (choose "Type in each year" or "Import from NYCAPS"; one or the other, each keeps its own numbers), 3 Your retirement (3/4s, slider, bill checkboxes, Social Security, inflation under "More options"). Results: the headline card (sticky on desktop) leads with the first-year total, pension plus VSF, a year and a month; then a breakdown (pension with the longevity bonus called out as included, VSF, total), the banked variable as a highlighted lump sum, and "Your pension grows X% a year", which opens a compact year-by-year table (pension a month, pension plus VSF a year, the Social Security offset at 62 marked). Then "Set a goal", comparison of 20, 23 and 25 years, and the power of escalation. On phones a bar at the bottom shows the monthly pension and jumps to the results.

## Calculation rules

These are the rules the page implements. They are the owner's decisions; do not change them without asking.

- **Pension:** 50% of final average salary (FAS) at 20 or more years of service, under S4727 (Chapter 692 of the Laws of 2025, signed Dec. 19, 2025). Tier 3 is capped at 50%.
- **FAS:** the best 3 consecutive calendar years. A year counts only up to 110% of the average of the two years before it. The owner confirmed 3 years (Oct. 2026), though the April 2024 plan description says 5. The personal calculator uses full calendar years before the retirement year.
- **Future years:** by default each year after the last known one repeats that year's pay. A checkbox fills them with a yearly raise (default 2.5%) instead. Future years after this one are folded under "Show future years".
- **Escalation:** the lesser of CPI or 3%, compounding, on the whole pension. (The bills' fiscal notes say CPI above 3% is banked and applied in later years; the page does not model that yet. Raised with the owner Oct. 4, 2026.) Full escalation needs collection to start at the **23-year** mark (FY2027 state budget, signed May 27, 2026; it was 25). The rate drops by 1/36 for each month collection starts earlier, so 20 years gets none. `LAW_ESC` in the script.
- **Longevity bonus (law):** at 25+ years, 5% of the rank's top pay plus 1% for each year past 25, capped at 15% at 35, added straight to FAS (FY2027 state budget, signed May 27, 2026; originally S9202/A10392). Always applied for service retirements, never for 3/4s. `LAW_BONUS` in the script. The rank picker shows in the personal calculator, and in the quick calculator at 25+ years. The page assumes collection starts at retirement. It used to offer deferring collection to the 25-year date; the owner had it removed (Oct. 2026) because the option was confusing.
- **COLA:** without full escalation, the pension gets the greater of escalation or COLA. COLA is half of CPI (minimum 1%, maximum 3%) on the first $18,000 only, starting at age 62 after 5 years retired or age 55 after 10 years retired. COLA reduces the VSF until 62.
- **VSF and banked variable:** the Variable Supplements Fund pays $12,000 a year to service retirees with 20 or more years. Each year worked past 20 banks one payment, paid as a lump sum at retirement. The $12,000 is fixed in the code; the owner had the editable assumption removed.
- **Social Security offset:** at 62 the pension drops by 50% of the primary Social Security benefit.
- **3/4s disability:** a large "3/4s (Disabled)" checkbox above the service slider. From the Fire Pension Fund's Tier 3 Enhanced Summary Plan Description (UFA copy dated April 18, 2024, PDF from the owner Oct. 4, 2026): accident disability pays 75% of FAS at any service, generally not taxed; no escalation (COLA only, after 5 years retired at any age); no VSF or banked variable; no Social Security offset; health insurance regardless of service; no workers' comp offset mentioned. The owner decided: the checkbox is in both calculators; no longevity bonus on disability; no 20/22½/25 comparison (the comparison, escalation section, bill checkboxes and Social Security field are hidden); slider runs from 1 year with stops every 5 years. Ordinary (non-line-of-duty) disability is not modeled. A typed FAS is not shrunk for service under 20 years.
- **Earnings import:** from the NYCAPS ESS "Tax Summary for All Years" table (Pay and Tax Information tile, then Pay and Tax Information > Tax Summary (W-2, 1127 & 1095C)), by copy and paste or by uploading a screenshot. It uses the Medicare Wages column (the third number after the year) and skips the current unfinished year. Pasted lines with a year and one number are also accepted. Screenshots are read on the device with Tesseract.js; because recognition drops commas and decimal points, screenshot amounts are read as digits with the last two as cents (every Tax Summary amount has cents). Either way the member checks the numbers on a review screen (amounts under $5,000 or over $400,000 are flagged), then locks them in. Locked years show "Verified" and can't be edited; other years can be typed. "Start over" clears the import. If ESS changes its menu names or columns, update the steps on the page and the parser. Tested with mock screenshots at computer, sideways-phone and upright-phone sizes, and a cut-off one; not yet with a real NYCAPS screenshot.
- **Goal:** the member enters a monthly pension goal. The page shows the FAS it takes (goal × 24, or × 16 on a 3/4s), the earliest retirement (by month, up to 35 years or age 62) where their earnings reach it, or, if they never do, the pay needed in the 3 years before their chosen retirement.
- **Service slider:** 20 to 35 years, with quick stops at 20, 23 (lower row), 25, 30 and 35. The comparison is 20, 23 and 25 years. The readout shows years on one line and months underneath ("0 months" on whole years), so its width doesn't change as the slider moves.

### Bills

**Became law May 27, 2026, in the FY2027 state budget** (owner's union rep, The Chief, and a union law firm's budget summary; the budget's exact text and chapter number have not been read). Because they passed inside the budget, the standalone bill pages on nysenate.gov still say "in committee"; the bill cards explain that.
- Full escalation at 23 years: S9204 (Jackson) / A11269 (Pheffer Amato).
- Longevity bonus, +1% a year: S9202 / A10392. The competing steps version (S9306 / A10438: 5/10/15%) did not pass and was removed from the page.

**Pending:** S9203 / A10254, full escalation at 20 years. In Senate Civil Service and Pensions as of Oct. 4, 2026; Assembly status not confirmed. It is the only "If this bill passes" checkbox, off by default.

Other bills from the owner's union rep (not pension-math, not on the page yet, statuses unconfirmed): widows' COLA S10636/A11555, WTC 25-to-35 extension S10085/A11213 (one source says signed Sept. 10, 2026 as Chapter 295), widows' property tax exemption S9502A/A10885.

When any status changes, update its entry in the `BILLS` array (`step`, dates, `law`, `signed`, `chapter`, `note`), the "last checked" date in Bills in Albany, the Pension basics answers and the fine print.

### S4727

50% at 20 years: Chapter 692 of the Laws of 2025, signed Dec. 19, 2025. Shown in Bills in Albany as law. Its introduction date was not found, so the card says "Introduced in 2025".

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

## Tier 2 page (`tier2/index.html`)

Added Oct. 11, 2026 at the owner's request, for members appointed July 1, 1973 to June 30, 2009. Served at https://myfirepension.com/tier2/. It is a separate single file with the same look, CSS and earnings import as the Tier 3 page; a fix to shared code (import parser, slider, question box) usually needs making in both files. It saves to localStorage under `fdny-tier2-v1`, separate from Tier 3. The Tier 3 home disclaimer, its "Am I in Tier 3?" answer and its Tier 2 warning link to it; the Tier 2 home links back only through the "Want to see Tier 3 instead?" button at the top (the owner had a separate Tier 3 card removed as confusing, Oct. 11, 2026). Same disclaimer rules. No Bills section (the owner chose to skip bills until he confirms which matter; S5058 of 2025, 1/60th to 1/40th, status unknown). Question box subject: "Tier 2 calculator: new question".

Sources: the Fire Pension Fund's Annual Comprehensive Financial Report for FY2025 (in the owner's Google Drive as `fire-pension-fund-cafr.pdf`; summary of Tier I and II benefits and the notes), and page 27 of the Tiers 1 & 2 Summary Plan Description dated Sept. 12, 2014 (vesting only; PDF from the owner). Rules the page implements, all owner decisions (Oct. 11, 2026):

- **Service pension:** 50% of final salary at 20 years, plus 1/60th of total earnings after the 20th anniversary, plus a pension from ITHP contributions after 20 years, plus an annuity from excess contributions. No cap. Slider 20 to 40 years (stops 20, 25, 30, 35, 40; 25 and 35 on the lower row).
- **Final salary (changed Oct. 12, 2026, owner's decision):** the higher of (a) earnings in the 12 months before retirement or (b) the average of the best 3 consecutive full calendar years before the retirement year, overtime included. **20% lookback** (owner): each best-3 year counts only up to 120% of the average of the 2 years before it, and the last 12 months only up to 120% of the average of the two 12-month periods before them (applying it to the last 12 months is our reading; tell the owner if the Fund says otherwise). **Not confirmed with the Fund**: the ACFR doesn't define final salary, and it says Chapter 372 of the Laws of 2000 revised the Tier 2 salary base. Longevity pay counts (it's in Medicare wages); the 2014 SPD excludes it only for vesting. The personal calculator splits calendar years evenly by month for the last 12 months and the 1/60th; the earnings table marks the years used ("Best 3" or "Final 12") and any amount over the 20% limit. The quick calculator uses the typed final salary as is and assumes each year past 20 paid about that much.
- **ITHP:** estimated as 5% of pay after the 20th anniversary (whole career on a 3/4s), credited 8.25% a year to retirement, then paid level to age 85 at 7%. The 7%/age-85 payout is our own assumption, labeled rough. An optional box ("Your ITHP and annuity from the Fund", per month) replaces the estimate for every plan. Excess contributions are not estimated. The quick calculator (owner's request, Oct. 11, 2026) shows the same ITHP estimate figured on the typed final salary, assuming appointment at age 25, plus the same optional box (shown past 20 years, or on a 3/4s); it also mentions the banked variable lump sum.
- **COLA only, no escalation; no Social Security offset** (the ACFR lists the offset only for Tier 3). COLA same rule as Tier 3; it reduces the VSF until 62.
- **VSF $12,000 and banked variable** as in Tier 3 (ACFR notes: max $12,000 since 2008 for firefighters and officers).
- **3/4s:** 75% of final salary + 1/60th after 20 + ITHP reserve + annuity of accumulated deductions; the 75% is federally tax-free, the rest federally taxed, all free of NYS/NYC tax. No VSF. COLA after 5 years retired.
- **Comparison:** "Retire soon or work longer?": the soonest date ahead (20 years or next month), then +1, +3 and +5 years, plus "Your plan". No escalation chart.
- **Age limit:** the page warns past 65 and the goal search stops at 65 or 40 years. The Tier 2 mandatory retirement age has not been verified.
- Not modeled: ordinary disability, vested retirement, other credited service (1/80th), loans and shortages, Tier 1, options.

## Open items

- **Question box.** Live: "What else would you like to see here?" on the home screen posts to Web3Forms (`ASK_KEY` near the end of the script; the key is public by design). Messages go to the address the owner registered with Web3Forms. The Pension basics answer about saving says numbers stay on the device and only the question box sends anything.
- **AI question box (planned, not started).** The owner wants to try it later (Oct. 11, 2026). Plan discussed: a Cloudflare Worker holds the Anthropic API key (never in the page or the repo; the owner enters it himself), the page sends only the question (not the member's numbers), the prompt carries that page's calculation rules, answers are labeled as AI with "check with the Fund", with a monthly spending cap and per-visitor limits. Before building, ask him: OK to label it as AI (it changes the no-AI-mention rule), which pages, and the cap. The "saved or sent" answers would need updating.
- **Credit.** Whether the page names him or stays anonymous is undecided. Do not add his name to the page until he says so.
- **Phone check.** The live site has not been checked on an actual phone since the move to GitHub Pages.
