# Implementation Details

How `index.html` — the East Lansing HQ Dashboard — was put together, and why.

## Overview

The dashboard summarizes higher education in East Lansing, MI. Research turned
up that Michigan State University (MSU) is the only degree-granting institution
physically located within East Lansing city limits, so the dashboard is
intentionally single-institution in scope, with a short callout explaining why
a second nearby site (Lansing Community College – East Campus) doesn't get its
own stats.

The page was built in three passes, each committed separately:
1. Enrollment stat tiles + gender/housing comparison bars
2. Top 5 majors table
3. Notable student spots map

## Constraints that shaped it

- **Single self-contained HTML file.** The dashboard is published as a Claude
  Artifact, which forbids external network calls (no CDN scripts/fonts, no
  external images, no API calls). Everything — CSS, JS, SVG — is inlined in
  `index.html`. This is also why there's no build step or framework.
- **No real map embed.** A literal Google Maps/Mapbox embed was ruled out by
  the same no-external-requests constraint. The "notable student spots" map is
  a hand-placed, illustrative layout (divs positioned by percentage over a
  two-zone background), explicitly labeled "not to scale" rather than
  presented as GPS-accurate.
- **Light/dark mode, no server.** Since there's no backend to detect a theme
  preference, theming is done entirely in CSS: `@media
  (prefers-color-scheme: dark)` as the default signal, plus
  `.viz-root[data-theme="dark"|"light"]` overrides so a host page's theme
  toggle can force a mode.

## Section-by-section choices

**Stat tiles (enrollment, undergrad, grad, student:faculty ratio).**
Plain numbers get a plain stat tile, not a chart — a bar or donut for a single
headline figure would just be decoration.

**Gender split / housing bars.**
Two-category proportions, so a single stacked bar reads faster than a pie and
avoids angle-judgment problems. Housing bar's note explicitly calls out that
MSU is a *hybrid* campus (59% off-campus) rather than letting the bar alone
imply "commuter school" or "residential campus."

**Top 5 majors table.**
Ranked by IPEDS-style completions data. One deliberate exclusion: "Multi-
Disciplinary Studies," a ~13,000-completion catch-all category that would
otherwise dominate the top of the list — it's a general-studies umbrella, not
a specific major, so it's footnoted out rather than silently dropped. The
"famous or awarded" column uses a status-green checkmark + label (not color
alone) because all 5 top majors turned out to have a verifiable national
distinction (e.g., MSU Supply Chain Management ranked #1 for 15 years,
Organizational Psychology ranked #1 nationally).

**Notable student spots map.**
Split into two zones — "Downtown East Lansing" and "MSU Campus" — divided by
a labeled "Grand River Ave" line, which mirrors the real geography (Grand
River Ave is the literal boundary between the two). A dashed "Red Cedar
River" path is decorative/orientational only, not surveyed. Categories
(Academic, Food & drink, Study spot, Social/hangout) map to the fixed
categorical color order. Each of the 9 places was chosen from web research on
places MSU students actually frequent, not arbitrary landmarks (e.g., The
Rock's daily-repaint tradition, El Azteco's decades-long run near campus).

## Data sourcing

All figures came from live web research rather than model memory, since
enrollment/ranking numbers change yearly. Where sources disagreed (e.g.,
DataUSA vs. College Factual gave different major completion counts, likely
different snapshot years), the numbers were cross-checked against multiple
sources and the most internally consistent set was used, with the
discrepancy noted inline where material (the Multi-Disciplinary Studies
footnote). Every section links its sources at the bottom of the page.

## Known limitations

- The campus map is illustrative and intentionally not to scale — pin
  positions approximate relative geography (downtown vs. campus, north vs.
  south) rather than actual coordinates.
- Major completion counts and enrollment figures aren't all from the exact
  same academic year, since no single public source publishes all of them
  together.
- Lansing Community College – East Campus is mentioned but has no stats of
  its own in this dashboard, since it's a satellite site of LCC rather than
  an independently reporting institution.
