# NAP-2: Naturalness & Privacy-Preserving Text Rewriting

*A benchmark dataset and code for evaluating privacy‑preserving text rewriting via deleting and obscuring strategies.*
This work is accepted by the Findings of The 2025 Conference on Empirical Methods in Natural Language Processing(EMNLP 2025).

---

## Overview

NAP‑2 provides paired inputs and rewrites that target **privacy‑preserving generation** while maintaining **utility/naturalness**. The dataset includes:

- **Human rewrites** (evaluation focus)
- **Synthetic rewrites** produced by GPT‑3/4 variants (training/ablation)
- Train/validation splits in efficient **Apache Arrow** format
- A lightweight notebook for **data visualization**


Use cases include: privacy‑aware rewriting, anonymization/sanitization, controllable generation, evaluation of privacy vs. utility trade‑offs.

---

## What’s in this repository

```
nap2/
├─ data/
│  ├─ human_rewrites/                 # Human rewrite CSV files (eval)
│  ├─ real/                           # Training split (Arrow)
│  ├─ real_valid/                     # Validation split (Arrow)
│  ├─ synth_gpt3_dp/                  # Synthetic rewrites from GPT‑3‑based pipeline
│  ├─ synth_gpt4/                     # Synthetic rewrites from GPT‑4‑based pipeline
├─ notebooks/
│  └─ visulizeData_pre_process_copy.ipynb  # Quick EDA & visualization
├─ LICENSE-DATA                       # CC BY‑NC 4.0 (dataset)
├─ NOTICE                             # Attributions and licensing summary
└─ README.md
```

---


### B) Train/validation in Arrow format

You can load Arrow directly via **pyarrow** or via **Hugging Face Datasets**.

**Using pyarrow**

```python
import pyarrow.dataset as ds
train = ds.dataset('data/real', format='arrow')
valid = ds.dataset('data/real_valid', format='arrow')
# Convert to table or pandas for inspection
train_tbl = train.to_table()
print(train_tbl.schema)
```

**Using datasets**

```python
from datasets import load_from_disk
train_ds = load_from_disk('data/real')
synth_gpt4 = load_from_disk('data/synth_gpt4')

print(train_ds)
```

## Task definition 

Given an input text and a privacy preference (user persona), generate a rewrite that **reduces personal/private information exposure** while **preserving utility and naturalness**. NAP‑2 includes two principal strategies:

- **Delete**: remove private spans or attributes entirely based on persona
- **Obscure**: replace with aliases or more generalized surrogates

Evaluation typically balances **privacy** (e.g., attribute leakage or recognizability),**utility**(semantic preservation) and **naturalness** (fluency). See the paper for recommended metrics and caveats.

---

## Splits & recommended use

- **Train**: `data/real/` (Arrow) — human rewrites suitable for training
- **Validation**: `data/real_valid/` (Arrow)
- **Test / Final evaluation**: `data/human_rewrites/` (CSV) — use only for final model selection and reporting
- **Augmentation**: `data/synth_gpt3_dp/`, `data/synth_gpt4/`


---

## Visualization

Open the notebook to preview distributions and examples:

```
notebooks/visulizeData_pre_process_copy.ipynb
```

You can run it in Jupyter or Colab. The notebook performs light EDA (counts per strategy/split, length histograms, example pairs, etc.).

---

## Licensing

- **Dataset (text files):** [Creative Commons BY‑NC 4.0](LICENSE-DATA). Research and other **non‑commercial** uses are permitted with attribution.
- **Code:** [Apache‑2.0](LICENSE).
- **NOTICE:** Attribution summary and third‑party credits are in `NOTICE`.

> If you need **commercial use** of the dataset, please contact the maintainers to discuss alternative terms.
---

## How to cite

If you use NAP‑2 in your research, please cite the paper and this repository.

**BibTeX**

```bibtex
@article{huang2024nap,
  title={NAP\^{} 2: A Benchmark for Naturalness and Privacy-Preserving Text Rewriting by Learning from Human},
  author={Huang, Shuo and MacLean, William and Kang, Xiaoxi and Xu, Qiongkai and Li, Zhuang and Yuan, Xingliang and Haffari, Gholamreza and Qu, Lizhen},
  journal={arXiv preprint arXiv:2406.03749},
  year={2024}
}
```



## Changelog

- **v1.0.0** — Initial public release of NAP‑2 dataset.

