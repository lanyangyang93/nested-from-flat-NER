
# Nested-from-Flat NER (Reproduction on eznlp)

Train with **flat** (non-overlapping) entity annotations, but at test time **recognize nested entities**.  
This repo documents a lightweight reproduction based on **[eznlp](https://github.com/syuoni/eznlp)**.

## Motivation

Nested NER requires costly nested annotations, while many corpora (e.g., CoNLL 2003) are flat. The **Nested-from-Flat (NFF)** setting asks:  
> *Can we train on flat data only and still detect nested entities?*  
We build on span-based models (e.g., DSpERT) and simple training rules to mitigate unlabeled nested entities inside gold spans. :contentReference[oaicite:0]{index=0}

---

## Method (high level)

1. **Enumerate spans** and classify them (span-based NER).
2. **Within-entity spans** (spans fully inside any gold entity) are risky because they may contain **unlabeled nested** mentions.
3. Two training options:
   - **Ignore** all within-entity spans in the loss.
   - **Sample** a tiny portion of within-entity spans as negatives with rate **γ** to balance precision/recall  
     (empirically γ≈0.005 on ACE04/05; γ≈0.05 on GENIA). :contentReference[oaicite:1]{index=1}
4. **Inference**: classify *all* spans; nested predictions emerge naturally.

For background on deep span representations and their benefits for nested structures, see **DSpERT**. :contentReference[oaicite:2]{index=2}

---

## Datasets

- **ACE 2004 / ACE 2005** (news/broadcast, nested entities)  
- **GENIA** (biomedical; DNA/RNA/Protein/Cell Line/Cell Type)  
- (Optional) **KBP 2017**, **NNE** for extra evaluation

**NFF setup**: remove nested labels from **train/dev** (to make supervision flat); keep **test** with gold nested labels for evaluation. :contentReference[oaicite:3]{index=3}

---

## Representative Results (from the paper)

**Within-entity F1** on nested benchmarks (train with flat supervision only, test on nested):  
| Dataset | Within-Entity F1 (%) |
|:--|--:|
| ACE 2004 | **54.8** |
| ACE 2005 | **54.2** |
| GENIA    | **41.1** |

These show NFF is feasible with strong span models. (Numbers quoted from the NFF paper.) :contentReference[oaicite:4]{index=4}

---

## License

This reproduction is for **academic use only**.
Please follow the licenses/terms of each dataset and the Apache-2.0 license of `eznlp`.
