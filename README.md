# SemiAD01 🔬
### Unsupervised Semiconductor Anomaly Detection

Semiconductor fabs cannot label every defect — defects are rare, varied, 
and often unseen during development. SemiAD01 detects and localises defects 
on real IC substrate and wafer images without any defect examples during 
training, using a comparison-based PatchCore approach.

---

## The Problem
- Traditional defect detection requires large labelled defect datasets
- Real fabs rarely have clean, comprehensive defect labels
- Defect types evolve — models trained on known defects miss new ones

## What SemiAD01 Does
| Capability | Details |
|---|---|
| **Unsupervised Detection** | Trains only on normal images — no defect labels needed |
| **Pixel-level Localisation** | Anomaly heatmaps show exactly where defects are |
| **Three Subset Coverage** | Ball-side IC, Chip-side IC, Patterned Wafer |
| **No Retraining Required** | Memory bank rebuilds instantly on new normal data |
| **Research-grade Evaluation** | Image AUROC, Pixel AUROC, PRO score |

---

## Results

Subset    | Image AUROC | Pixel AUROC | PRO Score | Recall | F1
----------|-------------|-------------|-----------|--------|-------
Wafer     | 0.9073      | 0.98        | 0.8298    | 0.8542 | 0.3411
Ball Side | 0.8110      | 0.99        | 0.3966    | 0.7160 | 0.2489
Chip Side | 0.7833      | 0.95        | 0.3691    | 0.7687 | 0.1070

**Key finding:** High recall across all subsets confirms strong defect 
sensitivity. Low precision reflects a conservative threshold — the model 
flags suspicious regions aggressively, which is the safer failure mode in 
semiconductor manufacturing where missing a defect is far more costly than 
a false alarm.

**Smoothing finding:** Gaussian smoothing (sigma=4) was tested and rejected 
— it suppressed anomaly peaks, reducing Image AUROC by up to 0.06 with no 
meaningful gain in Pixel AUROC.

---

## Approach — PatchCore
- Frozen pretrained ResNet18 used purely as a feature extractor
- Memory bank of normal patch features built from training images
- At test time, patch distances to memory bank produce anomaly scores
- No gradient updates, no backpropagation — entirely training-free

---

## Dataset
**Semi-AD** — IEEE DataPort  
(https://ieee-dataport.org/documents/semi-ad-semiconductor-anomaly-detection-dataset)

Three subsets: IC substrate ball-side, IC substrate chip-side, patterned wafer  
Provides aligned reference-test pairs and pixel-level ground truth masks

---

## Notebooks
| Notebook | Description |
|---|---|
| 01_EDA | Dataset exploration, image pair visualisation, ground truth masks |
| 02_PatchCore | Memory bank construction, anomaly scoring, full metric evaluation |
| 03_PatchCore_GaussianSmoothing | Smoothing experiment — tested and rejected |
| 04_Test | Image tester, Best/worst/false positive visualisation and Matrics scores |

---

## Tech Stack
Python 3.9 | PyTorch | torchvision | scikit-learn | NumPy | Matplotlib | PIL

---

## Project Structure
SemiAD01/
├── notebooks/     ← analysis notebooks
├── reports/       ← saved figures and results
├── memory_banks/  ← excluded (regenerate from notebook 02)
└── scores/        ← excluded (regenerate from notebook 02)

---

## Limitations & Next Steps
- Low precision due to score distribution overlap between normal and defect
- pair_set untouched — reserved for Siamese network implementation
- Next: comparison-based Siamese network using reference-test pairs to 
  reduce false positives and improve precision

---

## Author
**Ahmed Zabir Hussain** — Physics & Data Science  
Part of the SemiSight semiconductor analytics platform  
[SemiSight Repository](https://github.com/Ahmed-Zabir/SemiSight)  
Open to research collaborations and consulting engagements in semiconductor analytics.
