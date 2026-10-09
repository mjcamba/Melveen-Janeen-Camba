# Melveen-Janeen Camba — feedback, sweep of 2026-10-05

## Research paper review — pre-deadline read

**What I read.** `drafts/BUS-620_Research_Paper_Draft_10.1.2026.pdf` in full · `capabilities/economic-research/spec.md` and `docs/briefs/research-brief.md` in full · `data/README.md` and the two food CSVs · `analysis/figures/README.md` and the panel figures · the commits since my last read.

**What I did not open this pass.** `analysis/research-paper.pdf` (the older file) beyond its date · `prompt-log.md` · your Case 1 files · `AGENTS.md`, `CLAUDE.md`, `README.md`. If something in those changes an item below, say so and I will look.

---

Last time I asked for one event, one price and two groups; the draft has the Maui fires, households below the poverty line against those at or above it, and food CPI as context. The decision-maker is there (the Department of Health's nursing outreach), the elasticity of necessities and budget share are in the paper, and `data/README.md` names every series with its source. A local subject like this, with a real price and real households in it, is exactly right for this paper.

**Does a gap that predates the fires support the budget-share story?** On pages 7–8 the income gap sits almost entirely in barriers that existed before the fires (RR 1.79), while new barriers are close to equal across the two groups (RR 1.11, p = .43). The paper then calls this "consistent with the budget-share explanation," which is about how a household absorbs a new shock. If the difference was already there before August 2023, is that the budget-share mechanism or a different one? What would the paper need to show to tell the two apart, and can your MauiWES tables show it?

**Who was below the poverty line before the fires?** On pages 5–6 the cohort is 2.9 times as likely to be below the poverty line as the county, and the paper lists three explanations it cannot separate. MauiWES was collected after the fires, and 74 percent of respondents report reduced income. How many in your below-the-line group may have been at or above it before August 2023? And does that change who the recommendation's screening would find in advance?

**Make the PDF the paper you upload.** The food-price figures (32.9 percent for Hawaiʻi and 29.6 percent for the U.S. since 2019, 13.7 and 17.1 percent above trend) and the χ²(2) = 10.05 in the Table 1 note are not in any committed file; the raw CSVs are there, the calculation is not. A short script or a few lines in `data/README.md` would let a reader trace them. The body also runs about 2,000 words across pages 2–10. The limit is four pages, not counting the title page, graphs, bibliography and appendix, so check the PDF against it. And `analysis/research-paper.pdf` is an older file: make sure the PDF you upload to Lamaku is the current draft.

**Bring the brief up to date with the paper.** The brief is unchanged since my last read. It still reads "Status: Skeleton. Headings below are mine to fill in," still has a lone `capabilities/economic-research/spec.md.` line, and still asks the broad question about environmental disruption, chronic illness and financial stress, while your spec and paper ask the MauiWES question. Amending the brief to match the spec is the sanctioned move, and nothing else in the paper depends on it.

- **On github.com**: open `docs/briefs/research-brief.md`, click the pencil icon, make the edits, and commit.
- Or, in **Claude Code or Codex** opened in your portfolio repository: "Remove the skeleton status line and the stray spec path from `docs/briefs/research-brief.md`, and show me the research-question paragraph next to the spec's question so I can rewrite it."

**In order:**

1. Decide whether a gap that predates the fires supports the budget-share mechanism or another one, and answer it in the text.
2. Answer in the text how many in the below-the-line group may have been above it before the fires, and what that means for the screening.
3. Show where the CPI figures and the χ² come from, check the body against four pages, and upload the current PDF.
4. Clear the brief's skeleton line and stray path, and bring its question in line with the spec.
