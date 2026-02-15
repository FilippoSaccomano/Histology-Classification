# Histology-Classification

This repository contains the work of the **Backprop. Brothers** team for the **AN2DL Challenge 2** (histopathology image classification).

The task is to classify each whole-slide image into one of 4 classes:
- HER2(+)
- Luminal A
- Luminal B
- Triple Negative

The full methodology and experimental discussion are documented in [`Report.pdf`](./Report.pdf).

## Project idea (from the report)

The core solution is a **patch-based Multiple Instance Learning (MIL)** pipeline:
1. Split each high-resolution image into patches.
2. Keep informative tissue patches (HSV-based tissue filtering).
3. Extract frozen embeddings with a **Path Foundation** model (TensorFlow/Keras, cached to disk).
4. Build image-level bags of patch embeddings.
5. Train a **Gated-Attention MIL** head in PyTorch.
6. Use multi-seed ensembling for more stable inference.

Why this design: histology cues are local, while images are very large. MIL allows image-level supervision while learning which patches matter most.

## Notebooks

The three main notebooks test different ways to solve the **same 4-class classification task**:

- [`1-best.ipynb`](./1-best.ipynb)  
  Main/strongest pipeline: frozen Path Foundation embeddings + Gated-Attention MIL + multi-seed ensemble, with extensive diagnostics (confusion matrix, uncertainty, attention analysis).

- [`2-second.ipynb`](./2-second.ipynb)  
  Alternative experimental variant of the same patch/MIL workflow (different head/training choices and regularization settings).

- [`3-third.ipynb`](./3-third.ipynb)  
  Third experimental variant for the same task, used as an additional comparison baseline.

Additional preprocessing/EDA notebook:

- [`Preprocessing/challenge2_eda.ipynb`](./Preprocessing/challenge2_eda.ipynb)  
  Data quality checks, bug/corruption detection, mask consistency analysis, preprocessing visualization, and dataset organization utilities.

> Note: `Preprocessing/download_and_dataframming.py` and `Preprocessing/shrek_and_bug_detector.py` are currently empty placeholders in this snapshot.

## Reported result (main model)

From `Report.pdf`, the main ensemble reaches approximately:
- **Validation Macro-F1:** `0.5192`
- **Validation Accuracy:** `0.5268`

## Repository structure

```text
Histology-Classification/
├── 1-best.ipynb
├── 2-second.ipynb
├── 3-third.ipynb
├── Report.pdf
├── README.md
└── Preprocessing/
    ├── challenge2_eda.ipynb
    ├── download_and_dataframming.py
    └── shrek_and_bug_detector.py
```

## Notes

- Notebook paths are currently configured for Kaggle-style directories (as used during the challenge).
- The report is the authoritative reference for motivation, design choices, and experiment interpretation.

