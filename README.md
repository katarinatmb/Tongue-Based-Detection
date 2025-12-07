 Cardiovascular Disease Detection from Tongue Images

## 🔗 Related Projects

This project is part of a multi-modal cardiovascular disease detection research series:

- **👁️ [Eye-Based Detection](https://github.com/katarinatmb/eye-heart-disease-ml)** - Retinal fundus image analysis using CNN and logistic regression (AUC: 0.732)
- **👅 Tongue-Based Detection** (This Repository) - Tongue image classification using deep learning CNNs

## Overview

This project explores whether tongue photographs can reveal indicators of cardiovascular disease through deep learning analysis. Building upon centuries of Traditional Chinese Medicine practices that associate tongue appearance with internal organ function, this research applies modern convolutional neural networks to automatically discover diagnostic patterns in tongue images. Unlike the hand-crafted feature engineering approach used in the [eye-based detection project](https://github.com/yourusername/cardiovascular-eye-detection), this work employs end-to-end deep learning to extract features directly from raw image data, investigating whether subtle tongue characteristics including color, coating, texture, and moisture can serve as biomarkers for cardiovascular health.

The project analyzes a dataset of 830 tongue photographs from individuals with and without heart disease, developing a custom CNN architecture to classify cardiovascular status. This complements our eye-based research by exploring a different physiological manifestation of cardiovascular function, with the ultimate goal of combining both modalities in a multi-modal diagnostic system.

## Methodology

### Data Collection & Quality Control

The workflow began with a challenging dataset: full facial photographs showing individuals extending their tongues, rather than standardized clinical tongue images. These images varied significantly in lighting conditions, camera angles, tongue extension degree, and background environments—reflecting real-world screening scenarios but requiring extensive preprocessing.

A critical first step involved manual quality control to curate usable images from the raw dataset. Each image underwent individual visual inspection to identify and exclude problematic cases. Images with inadequate or inconsistent lighting that obscured tongue characteristics were removed, as were images with severe shadows that could be misinterpreted as coating or discoloration. Additionally, some subjects inadvertently presented the ventral (bottom) surface or lateral edges rather than the diagnostically relevant dorsal (top) surface. Only images clearly displaying the dorsal tongue surface with adequate lighting and proper orientation were retained.

This manual curation process, while labor-intensive, proved essential for ensuring data quality. Additional exclusions included motion-blurred images, extreme close-ups or distant shots preventing scale consistency, and images where the tongue was partially obscured by lips, teeth, or facial structures. The manual review ultimately retained 404 images from healthy controls and 426 images from individuals with documented heart disease, establishing a reasonably balanced dataset of 830 total images.

### Image Preprocessing

Following quality control, all retained images underwent standardized preprocessing. Each photograph was resized to 1024×1024 pixel resolution using Lanczos resampling, a high-quality interpolation method that preserves fine textural and chromatic details. This relatively high resolution was deliberately chosen to maintain subtle tongue surface characteristics—coating distribution, papillae patterns, and localized color variations—that might prove diagnostically relevant but would be lost at lower resolutions.

The dataset was then limited to 400 images per class through random sampling to ensure balanced representation and prevent class imbalance bias. Images were partitioned into training and test sets using an 80-20 split with stratification, yielding 640 training images and 160 held-out test images for unbiased evaluation. Before neural network training, images were further resized to 224×224 pixels for computational efficiency and normalized using ImageNet statistics.

### Deep Learning Architecture

The classification system employed a custom convolutional neural network designed to balance learning capacity against overfitting risk given the modest dataset size. The architecture comprised three convolutional blocks, each containing:
- A convolutional layer with 3×3 kernels (16, 32, and 64 filters respectively)
- ReLU activation for nonlinearity
- Max pooling with 2×2 windows for spatial downsampling

This sequential progression extracts increasingly abstract feature representations: the first layer detects low-level visual primitives like edges and color gradients, the second layer learns combinations representing coating textures and color regions, and the third layer captures high-level semantic features potentially corresponding to diagnostic tongue characteristics.

After three convolution-pooling rounds, the spatial feature maps were flattened and fed into a fully connected layer with 128 hidden units, creating a compact embedding of discriminative information. A dropout layer with 50% probability provided regularization to combat overfitting, followed by a final softmax classification layer producing probability estimates for heart disease presence.

### Training Procedure

Model training employed the Adam optimizer with a learning rate of 0.001 and cross-entropy loss over 10 epochs. The training trajectory showed characteristic deep learning dynamics: starting at 56.41% accuracy (barely above chance), the model progressively improved to 71.72% training accuracy by epoch 9, demonstrating the network discovered meaningful patterns despite the limited training data.

## Results & Performance

### Test Set Evaluation

Evaluation on the held-out test set revealed modest but above-chance classification performance:

| Metric | Value |
|--------|-------|
| **Test Accuracy** | 53.75% |
| **Precision** | 54.17% |
| **Recall** | 48.75% |
| **F1-Score** | 51.32% |
| **AUC** | 0.605 |

**Confusion Matrix:**
```
                Predicted
                Healthy  Disease
Actual Healthy     47       33
       Disease     41       39
```

The model correctly identified 47 of 80 healthy controls and 39 of 80 heart disease patients, showing similar performance across both classes without strong classification bias.

### Performance Analysis

The achieved AUC of 0.605 represents meaningful signal detection above the 0.5 random baseline, confirming that tongue images contain some cardiovascular information. However, this performance falls substantially short of clinical utility thresholds and notably lags behind our parallel [eye-based detection system](https://github.com/yourusername/cardiovascular-eye-detection) which achieved AUC 0.732 using hand-crafted features.

This performance gap likely reflects several factors:
1. **Feature Engineering vs. End-to-End Learning:** The eye-based system employed 67 expertly designed features targeting known cardiovascular manifestations, while the CNN learned features automatically from limited data
2. **Dataset Size:** 640 training images may prove insufficient for deep learning to discover robust discriminative patterns
3. **Biological Signal Strength:** Periorbital circulation patterns may manifest cardiovascular status more strongly than tongue characteristics
4. **Architecture Simplicity:** The relatively shallow 3-layer CNN may lack the capacity to capture complex diagnostic patterns

Despite modest standalone performance, these results establish proof-of-concept that computational tongue analysis can detect cardiovascular signals, laying groundwork for multi-modal integration.

## Documentation

For detailed methodology, results analysis, and future research directions, see:
- [📄 Comprehensive Project Summary](./docs/Tongue_Heart_Disease_Summary.docx)
