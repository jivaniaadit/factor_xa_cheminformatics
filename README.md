# Low-data QSAR and drug repurposing

**Goal:** take existing, approved, and shelved drugs, predict which ones may inhibit
**PRMT5** (an underserved cancer target), and structurally confirm the most promising
few, producing a ranked, honestly-caveated shortlist of repurposing candidates on a
pipeline proven to be trustworthy.

## The two targets, and why both

- **PRMT5 (primary, the subject).** Oncology target with synthetic lethality in
  MTAP-deleted tumors. Genuinely underserved and chemically diverse. Everything real,
  the repurposing screen, the disease relevance, happens here.
- **Factor Xa (control, the reference).** A data-rich, decades-optimized anticoagulation
  target. It exists only to **calibrate** the pipeline: run the identical code on a target
  where good models are known to be achievable, and its score tells you whether the
  pipeline works, so PRMT5's score becomes interpretable rather than floating.

Factor Xa is a **positive control**, the same logic as running a known-active sample in an
assay so a low reading on the unknown means "the drug is inactive," not "the assay is
broken." The comparison between the two is also the project's distinctive scientific
claim: how much harder does honest modeling get moving from a saturated target to an
underserved one, and what survives that move.

## Headline result (LightGBM, test R2)

| Split    | Factor Xa (control) | PRMT5 (primary) |
|----------|---------------------|-----------------|
| random   | 0.804               | 0.714           |
| scaffold | 0.614               | 0.407           |
| cluster  | 0.621               | 0.464           |

On every split the data-rich control generalizes better, and the gap is widest on the
scaffold split, the hardest honest test. PRMT5 is harder because it is sparse and
scaffold-diverse (diversity ratio 0.526 vs 0.41), not because of a modeling flaw.

## The three stages

1. **Stage 1, the engine** — interpretable activity model (QSAR + GNN), honest splits,
   applicability-domain check. *(In progress.)*
2. **Stage 2, the repurposing screen** — score approved, withdrawn, and shelved drugs
   against PRMT5, with an applicability-domain flag on every prediction.
3. **Stage 3, structural confirmation** — dock / co-fold top candidates into PRMT5 and
   cross-check against the Stage 1 interpretability story.

## Roadmap

| Phase | Work | Status |
|-------|------|--------|
| 0 | Cheminformatics fundamentals on Factor Xa (cleaning, fingerprints, clustering, leakage) | done |
| A | Model zoo, both targets | done |
| A | Consolidate to GitHub | **now** |
| A | Grouped cross-validation, lock the model | next |
| A | SHAP interpretability (PRMT5) | |
| A | Graph neural network, both targets | |
| B | Stage 2 repurposing screen | |
| C | Stage 3 structural confirmation | |
| D | Writeup, figures, optional structure-poor-target extension | |

## Notebooks

| Notebook | Purpose |
|----------|---------|
| `01_factor_xa_benchmark.ipynb` | control target, same pipeline |
| `02_prmt5_benchmark.ipynb` | primary target: pull -> features -> benchmark |
| `03_interpretability.ipynb` | SHAP on the locked model *(coming)* |
| `04_gnn.ipynb` | graph neural network comparison *(coming)* |
| `05_repurposing_screen.ipynb` | Stage 2 *(coming)* |

## Reproduce

```bash
pip install -r requirements.txt
```

Each notebook is self-contained and pulls its own data from ChEMBL (cached to Drive after
the first run).
