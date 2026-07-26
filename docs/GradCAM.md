# Grad-CAM for Explainable Skin Lesion Classification

# Introduction

Deep Convolutional Neural Networks (CNNs) have achieved remarkable performance in medical image classification, including skin lesion diagnosis. However, despite their high predictive accuracy, CNNs are often criticized for operating as **black-box models**.

In clinical applications, simply predicting a diagnosis is not sufficient. Physicians need to understand **why** a model made a particular prediction and whether the decision is based on medically meaningful visual features.

Explainable Artificial Intelligence (XAI) addresses this challenge by providing visual explanations for deep learning predictions.

One of the most widely used XAI techniques in medical imaging is **Gradient-weighted Class Activation Mapping (Grad-CAM)**.

---

# Why Explainability Matters

Medical AI systems assist clinicians in diagnosing diseases that may have serious consequences.

An incorrect prediction without explanation can reduce clinicians' trust in the system.

Explainability provides several important benefits:

- Increases physician confidence
- Improves transparency
- Helps detect incorrect model behavior
- Identifies dataset bias
- Validates learned visual features
- Supports trustworthy AI

In skin lesion classification, Grad-CAM allows us to verify whether the CNN focuses on the lesion itself rather than irrelevant image regions.

---

# What is Grad-CAM?

Grad-CAM (Gradient-weighted Class Activation Mapping) is a visualization technique that highlights image regions contributing most to a CNN prediction.

Instead of only predicting a class label, Grad-CAM generates a **heatmap** showing where the network looked before making its decision.

The resulting visualization helps determine whether the prediction is based on clinically meaningful structures.

---

# Intuition Behind Grad-CAM

Consider a dermoscopic image classified as **Melanoma**.

The CNN processes the image through multiple convolutional layers.

During prediction, Grad-CAM computes how strongly each feature map contributed to the predicted class.

These contributions are combined into a heatmap.

Areas with higher importance appear in warm colors such as:

- Red
- Orange
- Yellow

Areas with lower importance appear in:

- Blue
- Dark blue

The heatmap is then superimposed onto the original image.

---

# How Grad-CAM Works

The Grad-CAM pipeline consists of several steps.

## Step 1

A dermoscopic image is passed through the CNN.

```
Input Image

↓

ResNet50

↓

Predicted Class
```

---

## Step 2

The score of the predicted class is selected.

For example,

```
Melanoma
```

---

## Step 3

Gradients are computed with respect to the last convolutional layer.

These gradients indicate how important each feature map is for predicting the selected class.

---

## Step 4

The gradients are globally averaged.

Each feature map receives a weight.

Feature maps that contribute more strongly receive higher weights.

---

## Step 5

A weighted combination of feature maps is computed.

This produces a coarse localization map.

---

## Step 6

A ReLU activation removes negative contributions.

Only features that positively support the prediction remain.

---

## Step 7

The resulting heatmap is resized to match the original image.

Finally,

```
Original Image

+

Heatmap

↓

Grad-CAM Visualization
```

---

# Mathematical Formulation

For a target class \(c\),

the importance weight for feature map \(k\) is computed as

\[
\alpha_k^c
=
\frac{1}{Z}
\sum_i
\sum_j
\frac{\partial y^c}
{\partial A_{ij}^k}
\]

where

- \(A^k\) is the k-th feature map.
- \(y^c\) is the score of class c.
- \(Z\) is the number of pixels.

The Grad-CAM heatmap is then calculated as

\[
L_{GradCAM}^{c}
=
ReLU
\left(
\sum_k
\alpha_k^c
A^k
\right)
\]

---

# Why the Last Convolutional Layer?

Grad-CAM is usually computed from the last convolutional layer because it provides the best compromise between:

- semantic understanding
- spatial information

Earlier layers detect:

- edges
- colors
- textures

Later convolutional layers detect:

- lesion structures
- pigmentation
- vascular patterns
- disease morphology

Fully connected layers lose spatial information.

Therefore they cannot localize important image regions.

---

# Grad-CAM in ResNet50

In this project, Grad-CAM is generated from the final convolutional block of ResNet50.

```
Input

↓

Conv Layers

↓

Residual Blocks

↓

Last Conv Layer

↓

Grad-CAM

↓

Global Average Pooling

↓

Fully Connected Layer

↓

Prediction
```

The last convolutional features preserve spatial information while encoding disease-specific patterns.

---

# Expected Heatmaps for Each Skin Lesion

## AKIEC

Expected focus:

- Hyperkeratotic regions
- Surface scaling
- Irregular lesion borders

---

## BCC

Expected focus:

- Arborizing vessels
- Blue-gray globules
- Ulcerated regions
- Pigmented structures

---

## BKL

Expected focus:

- Milia-like cysts
- Comedo-like openings
- Keratin structures
- Pigment distribution

---

## DF

Expected focus:

- Central white scar-like area
- Peripheral pigment network

---

## MEL

Expected focus:

- Blue-white veil
- Irregular pigmentation
- Asymmetric structures
- Peripheral streaks
- Regression structures

---

## NV

Expected focus:

- Symmetric pigment network
- Regular globules
- Uniform pigmentation

---

## VASC

Expected focus:

- Red vascular structures
- Blood vessels
- Vascular lacunae

---

# Correct Grad-CAM Behavior

A reliable CNN should concentrate on

- lesion borders
- pigmentation
- vascular structures
- texture
- asymmetry
- diagnostic dermoscopic features

The heatmap should largely overlap with the lesion itself.

---

# Incorrect Grad-CAM Behavior

Grad-CAM may reveal undesirable model behavior.

Examples include attention focused on:

- image corners
- dark background
- ruler markings
- color calibration charts
- hair
- air bubbles
- illumination artifacts

Such behavior suggests that the CNN has learned spurious correlations rather than medically meaningful features.

---

# Grad-CAM and Trustworthy AI

Grad-CAM contributes to several principles of Trustworthy AI.

## Transparency

Visualizes the reasoning process of the CNN.

---

## Interpretability

Provides human-understandable explanations.

---

## Reliability

Verifies whether predictions rely on clinically relevant image regions.

---

## Fairness

Can reveal dataset biases.

For example,

if all Melanoma images contain rulers,

Grad-CAM may incorrectly highlight the ruler instead of the lesion.

---

## Debugging

Grad-CAM helps identify

- annotation errors
- dataset artifacts
- preprocessing mistakes
- shortcut learning

---

# Limitations of Grad-CAM

Although widely used, Grad-CAM has several limitations.

- Heatmaps are relatively coarse.
- Small diagnostic structures may be blurred.
- Explanations depend on the selected convolutional layer.
- Heatmaps indicate important regions but do not explain the exact reasoning.
- Different CNN architectures may generate different heatmaps for the same image.

Therefore, Grad-CAM should be considered a supportive visualization tool rather than definitive evidence of model reasoning.

---

# Applications in Medical Imaging

Grad-CAM has been successfully applied to numerous medical imaging tasks.

Examples include:

- Skin lesion classification
- Brain tumor MRI classification
- Chest X-ray analysis
- Diabetic retinopathy detection
- Histopathology image analysis
- Retinal OCT classification

---

# Grad-CAM in This Project

In this repository, Grad-CAM is used to visualize the regions responsible for ResNet50 predictions on dermoscopic skin lesion images.

The generated heatmaps allow qualitative evaluation of the model by verifying whether the CNN focuses on medically meaningful dermoscopic structures rather than irrelevant background regions.

Together with quantitative metrics such as Accuracy, Precision, Recall, F1-score, and the Confusion Matrix, Grad-CAM provides an additional level of model interpretability that is essential for trustworthy medical AI systems.

---

# Summary

Grad-CAM is a powerful Explainable AI technique that transforms CNN predictions from black-box decisions into interpretable visual explanations.

For skin lesion classification, it enables clinicians and researchers to verify whether the network attends to clinically relevant dermoscopic structures, increasing confidence in model predictions while facilitating debugging, bias detection, and trustworthy deployment in medical applications.

