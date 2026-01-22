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


---

## Stage I — Transformer-Based Segmentation

### Model
- Architecture: SegFormer (MiT backbone)
- Pretrained weights: ADE20K-finetuned checkpoint
- Task: Binary semantic segmentation (cell vs background)

### Rationale
SegFormer combines hierarchical transformer encoders with efficient multi-scale feature fusion, enabling the model to capture both global context and fine-grained boundaries. This is particularly well-suited for biomedical images, where contextual cues are often as important as local texture.

### Processing Pipeline
- RGB microscopy image ingestion
- Standardized preprocessing (resize, normalization, tensorization)
- Forward pass through SegFormer
- Logit upsampling to original resolution
- Probability thresholding to obtain binary masks
- Optional connected-component filtering for noise suppression

### Outputs
- Binary segmentation mask
- Pixel-aligned overlay for qualitative inspection
- Checkpointed segmentation model for reuse

---

## Stage II — ROI Extraction

The segmentation output is used to derive a classifier-ready region of interest.

- Largest connected component is selected as the primary WBC instance
- Bounding box is computed and expanded by a fixed margin
- Crop is extracted from the original RGB image
- Optional background suppression using the segmentation mask

This stage acts as a structural bridge between segmentation and classification, ensuring that downstream learning focuses exclusively on morphologically relevant information.

---

## Stage III — Vision Transformer Classification

### Model
- Architecture: ViT-B/16
- Input resolution: 224 × 224 RGB
- Output: Multi-class probability distribution over WBC types

### Rationale
Vision Transformers model long-range spatial dependencies through self-attention, making them well-suited for capturing global morphological patterns such as nucleus shape, cytoplasmic texture, and relative spatial organization—features that are central to hematological classification.

### Processing Pipeline
- ROI resizing and normalization
- Patch embedding and transformer encoding
- Classification via learned token representation
- Softmax normalization to obtain class probabilities

---

## Dataset
- Total samples: 1,560 microscopy images
- Number of classes: 8 WBC categories
- Data split: 60% training, 20% validation, 20% testing

Directory structure:

---

## Evaluation Protocol
Evaluation is performed independently for segmentation and classification.

**Segmentation**
- Dice coefficient
- Intersection-over-Union (IoU)
- Qualitative visual overlays

**Classification**
- Accuracy
- Precision and recall
- Confusion matrix analysis

---

## Results
*Reserved for quantitative results.*

---

## Visualizations
*Reserved for qualitative visualizations and overlays.*

---

## Implementation
The project is implemented in PyTorch and Hugging Face Transformers and is organized into two primary notebooks:

- `WBC_Segmentation.ipynb`  
  Transformer-based semantic segmentation

- `WBC_classification.ipynb`  
  Vision Transformer classification using segmentation-derived ROIs

Each notebook is modular, reproducible, and designed for extension.

---

## Significance
This work demonstrates a principled application of transformer architectures to medical image analysis by:
- Explicitly separating localization and recognition
- Improving interpretability through intermediate segmentation outputs
- Aligning computational modeling with clinical reasoning workflows

The proposed framework is extensible to multi-instance analysis, uncertainty estimation, and integration into decision-support systems.


