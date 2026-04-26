# Advanced Prompting vs LoRA Fine‑Tuning for Safety‑Critical Cosmetology Recommendations  

**Can we out‑prompt fine‑tuning when safety is on the line?**

A small, opinionated case study comparing **LoRA fine‑tuning** against **five prompt‑engineering strategies** for personalised skincare recommendations.  
Built with ❤️ for a diploma thesis, a poster session, and anyone who’s ever wondered if *“just write a better prompt”* actually works.

[![Open in Kaggle](https://kaggle.com/static/images/open-in-kaggle.svg)](https://www.kaggle.com/code/dianaminnakhmetova/nlp-case-study)
[![Dataset on Kaggle](https://img.shields.io/badge/Dataset-Kaggle-blue)](https://www.kaggle.com/datasets/dianaminnakhmetova/koyash)
---

## What’s inside?

- **Model** — Mistral‑7B‑Instruct, quantised to 8‑bit so it fits in a single T4 GPU.
- **Data** — 55 real expert cosmetology consultations + a 71‑product catalogue, both self‑made and publicly available on Kaggle.
- **Methods** — 6 variants:
  - LoRA fine‑tuned (rank 16, ~16 min training)
  - Zero‑shot
  - Few‑shot (with real examples)
  - Chain‑of‑Thought
  - Role‑based (“board‑certified dermatologist”)
  - Structured JSON
- **Metrics** — Name validity (does it use real products?), budget adherence, allergen safety, and output format quality.

---

## Why this matters

Most LLM benchmarks focus on fluency or factuality.  
But in **beauty, health, and any safety‑critical domain**, there are hard constraints:

- You **must not** recommend a product the client is allergic to.
- You **must** stay within a strict budget.
- You **must** use products that actually exist.

This project asks a very practical question:  
*“If I can’t afford to fine‑tune, can I just craft a smarter prompt and get the same safety guarantees?”*

(Spoiler: **mostly yes**, and sometimes even better.)

---

## Key findings (quick version)

| Method | Name Validity | Budget Adherence | Safety | Format Score |
|--------|--------------:|-----------------:|-------:|-------------:|
| **LoRA** | 0.977 | 0.818 | 1.000 | 0.750 |
| **Zero‑shot** | 0.909 | **0.955** | 1.000 | **0.750** |
| Few‑shot | 0.773 | 0.636 | 1.000 | 0.136 |
| Chain‑of‑Thought | 0.705 | 0.909 | 1.000 | 0.418 |
| Role‑based | 0.659 | 0.636 | 1.000 | 0.091 |
| Structured JSON | 0.614 | 0.545 | 1.000 | 0.614 |

- **LoRA** wins on product‑name accuracy.  
- **Zero‑shot** wins on budget control and **matches LoRA on safety & format** — with zero training cost.  
- **Complex prompts** (few‑shot, role‑based, CoT, JSON) consistently **degrade** output quality — they introduce structural corruption, hallucinated names, or budget parsing failures.

💡 *The takeaway: for safety‑critical recommendation tasks, start with zero‑shot. Fine‑tune only if brand precision is business‑critical. Avoid prompt over‑engineering.*

Full results, qualitative examples, and a detailed discussion → [poster PDF](Lora_vs_LLM_poster.pdf)

---

## How to reproduce

1. Open the notebook on Kaggle:  
   [https://www.kaggle.com/code/dianaminnakhmetova/nlp-case-study](https://www.kaggle.com/code/dianaminnakhmetova/nlp-case-study)
2. The required datasets are already attached to the notebook:
   - `/kaggle/input/datasets/dianaminnakhmetova/koyash/consultations.csv`
   - `/kaggle/input/datasets/dianaminnakhmetova/koyash/product_catalog.csv`
3. Run all cells top‑to‑bottom (GPU T4 ×2 is enough; the LoRA training takes  about 17 minutes)
4. You’ll get:
   - A metric table printed in the output.
   - A saved `results_comparison.png` chart.
   - `final_results.csv` and `experiment_meta.json`.

---

## Diploma context

This case study is part of a diploma.  
The counter‑intuitive finding — that simpler prompts outperform elaborate ones — motivates deeper research into lightweight guardrail layers and post‑generation validation, rather than heavy prompt crafting or expensive fine‑tuning.

---

## Credits

- **All consultations & product data** — originally created by me (Diana Minnakhmetova) for this research; publicly available on Kaggle.
- **Model** — Mistral‑7B‑Instruct by Mistral AI.
- **Frameworks** — Hugging Face Transformers, PEFT, BitsAndBytes.
If you find this work useful, a ⭐ on GitHub or a citation in your own research would mean the world 🌟

---

*Built with curiosity, Kaggle GPUs, and a healthy obsession with skincare ingredients.*
```
