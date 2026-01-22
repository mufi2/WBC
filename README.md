# White Blood Cell Analysis via Transformer-Based Segmentation and Classification

## Overview
This project presents an end-to-end computational pipeline for automated white blood cell (WBC) analysis from microscopy images. The system is designed around a **segmentation-first paradigm**, in which a transformer-based semantic segmenter is used to localize the target cell prior to downstream classification with a Vision Transformer (ViT).

By decoupling localization from semantic recognition, the pipeline reduces background bias, improves robustness to staining and illumination variability, and yields intermediate representations that are both interpretable and reusable for further analysis.

---

## Problem Definition
Microscopy images used in hematological analysis typically exhibit:
- Dense cellular backgrounds dominated by erythrocytes
- High inter-class morphological similarity among leukocytes
- Significant variability in staining, acquisition hardware, and illumination

End-to-end classification models that operate directly on raw images often conflate localization and recognition, leading to brittle decision boundaries and poor generalization.  
This work addresses the problem by explicitly modeling **cell localization and cell classification as separate learning tasks**.

---

## Methodology Overview

The pipeline consists of three sequential stages:

1. **Semantic segmentation of WBC regions**
2. **Region-of-interest (ROI) extraction**
3. **Transformer-based classification of the extracted cell**

