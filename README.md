# 🌐 nlp-low-resource-mt-aug

> **Back-Translation and Data Augmentation for Low-Resource Machine Translation**  
> English → Malayalam Neural Machine Translation using mBART-50, NLLB-200, Helsinki-NLP (Opus-MT), and mT5-Base

---

## 📌 Overview

Neural Machine Translation (NMT) has advanced significantly for high-resource languages, but **low-resource languages like Malayalam** still suffer from a critical lack of high-quality parallel corpora. This project tackles that challenge head-on.

We evaluate **four NMT architectures** on a constrained baseline of **8,000 English–Malayalam sentence pairs**, then apply a **Back-Translation (BT) based augmentation pipeline** to generate **10,000 additional synthetic parallel sentences**, significantly improving translation quality across all models.

---

## 🧠 Models Evaluated

| Model | Architecture | Parameters | Key Trait |
|---|---|---|---|
| `Helsinki-NLP/opus-mt-en-ml` | MarianMT Transformer | ~76M | Lightweight, translation-optimized |
| `facebook/mbart-large-50` | Seq2Seq Denoising Autoencoder | 600M+ | Strong morphological understanding |
| `facebook/nllb-200-distilled-600M` | Mixture-of-Experts Transformer | 600M | State-of-the-art zero-shot for LRLs |
| `google/mt5-base` | Text-to-Text Transfer Transformer | 580M | Generic generation, flexible |

---

## 📊 Results

### BLEU Score Comparison (200-sample test set)

| Model | Baseline BLEU | Augmented BLEU | Gain (ΔBLEU) |
|---|---|---|---|
| mT5-Base | 1.03 | 2.15 | +1.12 |
| mBART-50 | 1.44 | 3.80 | +2.36 |
| NLLB-200 | 2.86 | 5.12 | +2.26 |
| **Helsinki-NLP** | **4.40** | **6.85** | **+2.45** |

> ✅ **Helsinki-NLP** achieved the best performance despite having the fewest parameters — demonstrating that task-specific optimization beats over-parameterization in low-resource settings.

---

## ⚙️ Methodology

### Back-Translation Pipeline

```
Monolingual Malayalam Data (ML)
        ↓
   Back-Translation Model
        ↓
  Generate Synthetic English (EN*)
        ↓
  Synthetic Pairs (EN* ↔ ML)
        ↓
  Fine-tune EN → ML model
```

1. **Baseline Training** — All four models fine-tuned on 8,000 EN–ML sentence pairs
2. **Back-Translation** — Malayalam monolingual data fed through mBART/NLLB to generate synthetic English
3. **Augmented Training** — Models retrained on 18,000 pairs (8k original + 10k synthetic)
4. **Evaluation** — BLEU and chrF scores on held-out test set

---

## 📁 Project Structure

```
nlp-low-resource-mt-aug/
│
├── google_mt5_base.ipynb          # mT5-Base fine-tuning notebook
├── mbart.ipynb                    # mBART-50 + back-translation notebook
├── NLLB_200.ipynb                 # NLLB-200 fine-tuning notebook
├── Helsinki_NLP_opus_mt_en_ml.ipynb  # MarianMT fine-tuning notebook
│
├── reports/
│   ├── mt5_enml_report.txt
│   └── ...
│
└── README.md
```

---

## 🗃️ Dataset

- **Source:** [English to Malayalam Machine Translation Dataset](https://www.kaggle.com/datasets/parvmodi/english-to-malayalam-machine-translation-dataset) by P. Modi (Kaggle, 2021)
- **Baseline corpus:** 8,000 EN–ML sentence pairs
- **After augmentation:** ~18,000 pairs (10,000 synthetic via Back-Translation)
- **Split:** Train / Eval / Test

---

## 🔍 Key Findings

- **mT5-Base** suffered catastrophic hallucination at baseline (BLEU 1.03), repeating tokens like "വിശ്വാസം" over 30 times
- **Helsinki-NLP** achieved the highest semantic accuracy even at baseline, correctly rendering complex phrases
- **mBART-50** showed mixed-script outputs (Malayalam + Roman) before augmentation
- **All models** improved meaningfully after BT-based augmentation, with the augmented dataset converging faster and reaching lower validation loss

---

## 🚀 Getting Started

### Prerequisites

```bash
pip install transformers datasets sacrebleu kagglehub sentencepiece accelerate
```

### Run a Notebook

Open any of the `.ipynb` files in **Google Colab** (GPU recommended):

- `Helsinki_NLP_opus_mt_en_ml.ipynb` — Best starting point (lightest model)
- `mbart.ipynb` — Includes the full back-translation pipeline
- `NLLB_200.ipynb` — Best synthetic data quality
- `google_mt5_base.ipynb` — Generic seq2seq baseline

---

## 📐 Evaluation Metrics

- **BLEU** (Bilingual Evaluation Understudy) — n-gram precision with brevity penalty
- **chrF** (Character n-gram F-score) — Better suited for morphologically rich languages like Malayalam

---

## 👥 Authors

| Name | Institution |
|---|---|
| Raj Narayanan | Lovely Professional University, Phagwara |
| Enjula Uchoi | Lovely Professional University, Phagwara |
| Kanika Debbarma | National Institute of Technology, Arunachal Pradesh |
| Avnish Thakur | Lovely Professional University, Phagwara |

---

## 📄 Citation

If you use this work, please cite:

```
@article{narayanan2024backtranslation,
  title     = {Back-Translation and Data Augmentation for Low-Resource Machine Translation},
  author    = {Raj Narayanan and Enjula Uchoi and Kanika Debbarma and Avnish Thakur},
  year      = {2024},
  note      = {Lovely Professional University / NIT Arunachal Pradesh}
}
```

---

## 📚 References

- Fadaee et al., "Data Augmentation for Low-Resource NMT," ACL 2017
- Kimera et al., "Data Augmentation With Back-Translation for Low Resource Languages," NLPIR 2024
- Costa-jussà et al., "No Language Left Behind," Meta AI 2022
- Tiedemann & Thottingal, "OPUS-MT," EAMT 2020
- Dabre et al., "IndicTrans," ACL 2022

---

> 💡 *This project contributes to the broader goal of making machine translation accessible to underrepresented language communities.*
