# Build plan (~3–4 weeks)

Tick each box when done. Commit at the end of every milestone.

## Week 1 — Prove it works
- [ ] M1: Notebook — fetch TP53 ClinVar missense variants, parse positions, cache raw data
- [ ] M2: Notebook — load AlphaFold structure, colour pathogenic (red) vs benign (blue) in py3Dmol
- [ ] M3: Look at the result. Write 3 bullet points in NOTES.md on what you see and why
         (e.g. pathogenic variants cluster in the DNA-binding domain)

## Week 2 — Add the ML
- [ ] M4: Move notebook logic into `pipeline/` modules + pytest tests
- [ ] M5: ESM-1v masked-marginal scores for every variant (CPU, cached)
- [ ] M6: Add AlphaMissense scores; compute AUROC for both; write `data/processed/TP53.json`
- [ ] M7: Add 2 more genes (e.g. BRCA1, CFTR) via config only

## Week 3 — Make it look good
- [ ] M8: Vite + TS + Mol* front end reading the JSON; 3D view with colour toggle
- [ ] M9: Variant table (sort/filter), click-to-highlight in 3D, gene switcher
- [ ] M10: ROC curve chart, ESM vs AlphaMissense
- [ ] M11: Polish: loading states, mobile layout, short "how to read this" panel

## Week 4 — Ship it
- [ ] M12: Deploy (Vercel / Netlify / GitHub Pages); embed or link from Astro portfolio
- [ ] M13: README with GIF, architecture diagram, results, limitations
- [ ] M14: Record a 30–45s demo video
- [ ] M15: LinkedIn post: the problem → what you built → one interesting finding → link
