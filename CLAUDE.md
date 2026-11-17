# Variant Effect Explorer — project brief for the coding agent

## What this is
An interactive web tool that shows where disease-causing (pathogenic) and harmless (benign)
missense variants sit on a protein's 3D structure, and how well a protein language model
(ESM) predicts which is which. Portfolio project aimed at SWE roles at biotech companies.

Start gene: **TP53** (UniProt P04637). Design everything so adding a gene = one config entry.

## Architecture (keep it this way)
1. **Offline Python pipeline** (`pipeline/`) does all heavy work and writes static JSON to
   `data/processed/<gene>.json`.
2. **Static web front end** (`web/`) reads that JSON and renders it. No backend, no server,
   no database. Deploys free and embeds into my Astro portfolio site.

## Data sources
- **ClinVar** via NCBI E-utilities (esearch + esummary/efetch). Keep only single-amino-acid
  missense variants with review status of at least 1 star. Labels: Pathogenic/Likely
  pathogenic → `pathogenic`; Benign/Likely benign → `benign`. Drop VUS and conflicting.
- **AlphaFold DB** API: `https://alphafold.ebi.ac.uk/api/prediction/<UNIPROT_ID>` → use the
  returned PDB/CIF URL. Keep pLDDT confidence per residue.
- **ESM**: use `fair-esm`, model `esm1v_t33_650M_UR90S_1`, masked-marginal scoring
  (log P(mutant) − log P(wild type) at the masked position). Must run on CPU.
- **AlphaMissense** precomputed scores as a baseline to compare against.
- Cache every raw download in `data/raw/`. Never re-hit an API if the cache exists.
  Respect NCBI rate limits (≤3 req/s without an API key).

## Output JSON schema (per gene)
```
{ gene, uniprot_id, sequence, structure_url, plddt: [..],
  variants: [ { pos, wt, mut, hgvs_p, label, review_stars, esm_score, alphamissense } ],
  metrics: { esm_auroc, alphamissense_auroc, n_pathogenic, n_benign } }
```

## Front end
- Vite + TypeScript + Mol* (or 3Dmol.js if Mol* is painful). Tailwind for styling.
- Views: 3D structure coloured by label or by ESM score (toggle); a variant table you can sort
  and filter; click a variant → highlight it in 3D; ROC curve comparing ESM vs AlphaMissense.
- Clean, dark, minimal. It has to look good in a 20-second screen recording.

## How to work with me
- Before anything bigger than a small fix: give me a short plan and wait for my OK.
- After each milestone in PLAN.md: tell me what you built, how to run it, and **explain any
  biology or ML decision in 2–3 plain sentences** so I can explain it in interviews.
- Small commits with clear messages. Don't commit `data/raw/` or model weights.
- Write tests for the parsing and scoring logic (pytest). The UI doesn't need tests.
- If something is scientifically questionable (e.g. data leakage, cherry-picking), flag it.
- Prefer simple, readable code over clever code. No unnecessary abstractions.

## Commands
- Python env: `python -m venv .venv && source .venv/bin/activate && pip install -r requirements.txt`
- Run pipeline: `python -m pipeline.run --gene TP53`
- Tests: `pytest`
- Web: `cd web && npm install && npm run dev`
