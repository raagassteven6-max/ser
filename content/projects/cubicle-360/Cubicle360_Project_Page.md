---
cover: cubicle360-symbol-on-graphite.jpg
title: Cubicle 360
summary: A data workstation for turning messy office CSV exports into clear, trustworthy reports.
tech: [HTML, CSS, Vanilla JS]
date: 2026-11-25
link: https://raagassteven6-max.github.io/cubicle-360/
titleFont: sans
titleColor: lime
techFont: oswald
techColor: lime
---



::: banner
src: file_00000000e4388211bab7b5eb96712643.png
:::

**A data workstation for turning messy office CSV exports into clear, trustworthy reports. It runs entirely in your browser, offline, from a single file.**



> A personal project by mysel. Built for real BPO-style reporting work: CSAT, QA, attendance, tickets, productivity. It's free to use and not a commercial product.

---

## <text color="lime">Why</text> I built it

Working in contact centers and hospitality, I kept running into the same job: someone hands you a raw export, and you need a report from it by the end of the shift. That usually meant hours of Excel formulas, pivot tables, and a nagging worry that one wrong filter had quietly changed the numbers.

Cubicle360 is my answer to that. You drop in a CSV, it checks what it's looking at, you pick a goal in plain language, and it builds the report. It also tells you when it has to make an assumption, instead of making it silently.

::: image
src: template-library.png
size: full
:::

## <text color="lime">What</text> it does

- **Reads your file and explains it.** Detects column types (numbers, dates, durations, categories) and runs a health check for duplicate rows, spellings that differ only by case, ambiguous dates like 03/04/2026, and placeholder values such as "N/A".
- **Starts from a goal, not a formula.** A Template Library covers common tasks: customer satisfaction, rankings, quality scores, attendance and leave, tickets and SLA, productivity summaries, and general data exploration.
- **Guided builder for custom reports.** Choose a group, a metric, and a sort order. Advanced users get a full manual builder with filters, multi-level grouping and calculations.
- **Multiple visuals per report.** Add as many charts as you like, each with its own chart style and its own metric: bar, horizontal bar, line, pie or donut.
- **Exports and prints.** Download a formatted Excel workbook with native charts, or print a report straight from the page.
- **Remembers your rules, never your data.** Saved report settings and column mappings stay in your browser. Your spreadsheet rows are never saved.


::: image
src: home.png
size: large
:::


## <text color="lime">Where</text> your data goes

Nowhere. Files are read inside your browser and are never uploaded. There are no accounts, no ads, and no tracking. In my own test run of the app in a browser, it made no network requests at all. It also works with the internet turned off.

## Built to be <text color="lime">trusted</text>

A report is only useful if the numbers are right, so most of the work went into correctness rather than looks.

- **An audit first.** Before polishing anything, I had the calculation engine audited and then fixed what it found. Examples: a "lowest first, top 2" ranking that returned the highest rows, period-over-period changes that compared across different teams, and an attendance rate that silently ignored unusual absence codes.
- **No silent guessing.** When a rule needs a decision, such as what score counts as "satisfied" or which statuses mean "absent", the app either asks you, states the assumption it used, or declines to calculate. It does not quietly pick one.
- **Small samples are flagged.** Trends and leaderboards show how many responses sit behind each number, with a caution when the count is small.
- **Self-tests built in.** The app carries its own test suite (62 tests whose expected values were worked out by hand, not copied from the app's output).

::: image
src: csat-report.png
size: large
:::




## <text color="lime">Under the hood</text>

- One self-contained HTML file (about 0.5 MB)
- Plain JavaScript, with no frameworks, no dependencies and no build step
- A hand-written Excel (.xlsx) writer, including native charts
- Local storage in the browser (IndexedDB) for saved settings only
- Tested in a desktop Chromium browser at phone, tablet, laptop and wide-desktop widths

## <text color="red">Status</text> and <text color="red">limits</text>

It's a personal project that I use and keep improving, so treat it as a work in progress. Known gaps today: leave and holiday days can't yet be excluded from attendance rates, charts in Excel exports keep a plainer look than the on-screen charts, and I've tested it in Chromium only, so other browsers may behave differently.

::: callout
type: important
title: Be attentive

Numbers always deserve a second look before they go into a report that affects people. Please check anything important.
:::

## Feedback

Ideas, bug reports and requests are welcome. 
Send me a message at <text color="lime">drawdewindsor@gmail.com</text>

---

It's free you can try it out!
