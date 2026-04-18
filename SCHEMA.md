# BoostKlient Wiki — Schema

## Purpose
Persistent knowledge base for the BoostKlient Pinterest SaaS platform.
Accumulates knowledge about Pinterest niches, trends, content strategies,
competitors, and platform analytics. LLM maintains all wiki content;
human curates sources and directs analysis.

## Directory Structure

```
wiki/
├── niches/       # Pinterest niches (DIY, Fashion, Food, etc.)
├── trends/       # Trends by niche and time period
├── strategies/   # Content strategies and best practices
├── competitors/  # Competitor analysis and examples
├── concepts/     # Pinterest concepts (algorithm, SEO, formats)
└── analytics/    # Platform-level analytics and insights
raw/              # Immutable source documents (never modify)
└── assets/       # Downloaded images from articles
index.md          # Content catalog (LLM maintains)
log.md            # Chronological operation log (append-only)
SCHEMA.md         # This file
```

## Page Types

### Niche Page (`wiki/niches/<slug>.md`)
```yaml
---
title: <Niche Name>
type: niche
tags: [niche, <keywords>]
sources: <count>
updated: <YYYY-MM-DD>
---
```
Sections: Overview, Audience, Top Content Formats, Seasonal Trends,
Key Competitors, Content Strategy Notes, Related Niches, Sources

### Trend Page (`wiki/trends/<slug>.md`)
```yaml
---
title: <Trend Name>
type: trend
niche: <niche-slug>
period: <YYYY-QN or YYYY-MM>
status: rising | peak | declining
sources: <count>
updated: <YYYY-MM-DD>
---
```
Sections: Description, Evidence, Related Niches, Content Opportunities,
Expiry Estimate, Sources

### Strategy Page (`wiki/strategies/<slug>.md`)
```yaml
---
title: <Strategy Name>
type: strategy
niches: [<niche-slugs>]
sources: <count>
updated: <YYYY-MM-DD>
---
```
Sections: Overview, When to Use, Implementation Steps, Examples,
Contraindications, Related Strategies, Sources

### Competitor Page (`wiki/competitors/<slug>.md`)
```yaml
---
title: <Brand/Account Name>
type: competitor
niche: <niche-slug>
platform_handle: <@handle>
sources: <count>
updated: <YYYY-MM-DD>
---
```
Sections: Overview, Content Patterns, Strengths, Weaknesses,
Top Pins Analysis, Posting Cadence, Sources

### Concept Page (`wiki/concepts/<slug>.md`)
```yaml
---
title: <Concept Name>
type: concept
tags: [<keywords>]
sources: <count>
updated: <YYYY-MM-DD>
---
```
Sections: Definition, How It Works on Pinterest, Best Practices,
Common Mistakes, Related Concepts, Sources

### Analytics Page (`wiki/analytics/<slug>.md`)
```yaml
---
title: <Metric or Report Name>
type: analytics
period: <YYYY-MM or YYYY-QN>
sources: <count>
updated: <YYYY-MM-DD>
---
```
Sections: Summary, Key Metrics, Insights, Anomalies,
Recommendations, Related Pages, Sources

## Workflows

### Ingest (new source)
1. Read source document
2. Discuss key takeaways with human if needed
3. Write summary in appropriate `wiki/` subdirectory
4. Update or create referenced entity/concept pages
5. Check for contradictions with existing pages — flag explicitly
6. Update `index.md` (add new pages, update source counts)
7. Append entry to `log.md`: `## [YYYY-MM-DD] ingest | <Title>`

### Query
1. Read `index.md` to find relevant pages
2. Read relevant pages
3. Synthesize answer with page citations (`[[page-title]]`)
4. If answer is reusable knowledge — file it as a new wiki page
5. Append entry to `log.md`: `## [YYYY-MM-DD] query | <Question summary>`

### Pinterest Sync Ingest
Triggered automatically when BoostKlient syncs Pinterest data.
1. Receive structured data: boards, pins, analytics, trends
2. Update relevant niche pages (new pins as examples)
3. Update or create trend pages (rising/falling signals)
4. Update analytics pages
5. Log: `## [YYYY-MM-DD] pinterest-sync | <account> | <boards count> boards`

### Lint (periodic health check)
1. Scan all pages for: broken links, stale dates (>90 days), orphan pages
2. Check for contradictions between pages
3. List concepts mentioned but lacking own page
4. Suggest new sources to investigate
5. Log: `## [YYYY-MM-DD] lint | <issues found> issues`

## Cross-referencing Conventions
- Use Obsidian wiki-links: `[[wiki/niches/fashion]]`
- Always link entity names on first mention in a page
- Each page should have at least 2 inbound links (no orphans)
- Source references: `> Source: [[raw/article-name]]`

## Contradiction Handling
When new source contradicts existing claim:
- Add `> ⚠️ Contradicted by [[raw/new-source]] (YYYY-MM-DD)` below the claim
- Do NOT silently overwrite — flag it for human review
- After human confirms → update and remove the flag

## Naming Conventions
- File names: `kebab-case.md`
- Pinterest niches: use English slugs (`home-decor`, `diy-crafts`)
- Dates: ISO 8601 (`2026-04-18`)
- Periods: `2026-Q2`, `2026-04`
