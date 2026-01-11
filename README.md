# Comparative Analysis of Loss Functions for Brain Tumor Segmentation

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![PyTorch](https://img.shields.io/badge/PyTorch-Deep%20Learning-red)
[![Dataset](https://img.shields.io/badge/Dataset-Kaggle_Brain_MRI_Segmentation-blue?logo=kaggle)](https://www.kaggle.com/datasets/mateuszbuda/lgg-mri-segmentation)

## Project Overview
This project implements a **U-Net** architecture from scratch to perform semantic segmentation on brain MRI scans (LGG Segmentation Dataset). The primary goal was to conduct a controlled experiment comparing **Dice Loss** against the standard **Binary Cross-Entropy (BCE) Loss** to address the class imbalance problem inherent in medical imaging.

Unlike standard tutorials that import pre-built models, this repository features a custom PyTorch implementation of the U-Net architecture to ensure full control over the encoding/decoding pathways.

---

## Key Results
The hypothesis was that optimizing for the Dice Coefficient would yield sharper tumor boundaries than pixel-wise cross-entropy. The experiment confirmed this, with the Dice Loss model significantly outperforming the baseline.

| Metric | Baseline (BCE Loss) | Proposed Method (Dice Loss) | Improvement |
| :--- | :--- | :--- | :--- |
| **IoU (Intersection over Union)** | ~0.48 | **0.6691** | +39% |
| **Dice Score** | ~0.64 | **0.8012** | +25% |
| **Precision** | N/A | **0.7955** | - |
| **Recall (Sensitivity)** | N/A | **0.8098** | - |

### Convergence Analysis
*Below is the training history comparing the two loss functions over 8 epochs.*

![Training Graph](final_research_graph.png)

> **Observation:** The BCE model (grey) stagnated around epoch 6, whereas the Dice model (green) continued to optimize the shape overlap, validating its effectiveness for small targets.

---

## Visual Demonstrations
Quantitative metrics don't tell the whole story. Below is a comparison of the model's predictions on unseen validation patients.

![Patient Predictions](patient_scan_1.png)

* **Left:** Original MRI Scan.
* **Middle:** Ground Truth (Radiologist's annotation).
* **Right:** Model Prediction.

Note the high structural similarity between the Ground Truth and the Prediction, even for irregular tumor shapes.

---

## Methodology & Tech Stack

### Architecture: Custom U-Net
Instead of using `segmentation-models-pytorch`, I implemented the U-Net class manually to understand the feature concatenation process.
* **Encoder:** 4 blocks of Double Convolutions + Max Pooling.
* **Decoder:** 4 blocks of Up-sampling + Skip Connections (concatenation).
* **Bottleneck:** Bridge between encoder/decoder with 1024 feature maps.

### Data Processing
* **Dataset:** [LGG MRI Segmentation](https://www.kaggle.com/datasets/mateuszbuda/lgg-mri-segmentation)
* **Preprocessing:**
    * Filtered dataset to remove non-tumor slices (focusing training on positive samples).
    * Resized images to `256x256`.
    * Normalized pixel values to `[0, 1]`.

---

## 💻 How to Run

### 1. Installation
Clone the repo and install dependencies:
```bash
git clone [https://github.com/your-username/brain-tumor-segmentation-research.git](https://github.com/your-username/brain-tumor-segmentation-research.git)
cd brain-tumor-segmentation-research
pip install -r requirements.txt
