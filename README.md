# Explainable Skin Lesion Classification

An explainable deep learning project for skin lesion classification using the HAM10000 dataset and a pretrained ResNet50 model.
The main goal of this project is not only to classify skin lesions, but also to investigate why the model makes its predictions using explainability techniques such as Grad-CAM and Grad-CAM++.

## 📌 Project Overview

Skin lesion classification is an important application of deep learning in medical imaging. However, high classification accuracy alone is not sufficient for a trustworthy medical AI system.
A model may correctly classify an image while relying on irrelevant regions or visual artifacts. Therefore, this project combines:

- Deep learning-based image classification
- ResNet50 feature extraction
- Class-weighted training
- Model evaluation
- Confusion matrix analysis
- Grad-CAM visualization
- Grad-CAM++ visualization
- Class-specific analysis
- Trustworthy AI principles

The overall workflow is:
```

HAM10000 Dataset
│
▼
Image Preprocessing
│
▼
ResNet50
│
▼
Skin Lesion Classification
│
├──────────────► Quantitative Evaluation
│                  ├── Accuracy
│                  ├── Loss
│                  └── Confusion Matrix
│
└──────────────► Explainability
├── Grad-CAM
└── Grad-CAM++

```

## 🧠 Dataset

This project uses the **HAM10000** (Human Against Machine with 10000 training images) dataset.
HAM10000 contains dermatoscopic images representing seven different diagnostic categories of pigmented skin lesions.

The seven classes used in this project are:

| Code | Lesion |
|------|--------|
| AKIEC | Actinic Keratoses / Intraepithelial Carcinoma |
| BCC | Basal Cell Carcinoma |
| BKL | Benign Keratosis-like Lesions |
| DF | Dermatofibroma |
| MEL | Melanoma |
| NV | Melanocytic Nevi |
| VASC | Vascular Lesions |

For more information about the dataset:
👉 [HAM10000 Dataset Documentation](docs/HAM10000.md)

## 🔬 Skin Lesion Classes

Each lesion category has its own documentation describing the characteristics of the class and its relevance to the classification problem.

- [AKIEC](lesions/AKIEC.md) — Actinic Keratoses / Intraepithelial Carcinoma
- [BCC](lesions/BCC.md) — Basal Cell Carcinoma
- [BKL](lesions/BKL.md) — Benign Keratosis-like Lesions
- [DF](lesions/DF.md) — Dermatofibroma
- [MEL](lesions/MEL.md) — Melanoma
- [NV](lesions/NV.md) — Melanocytic Nevi
- [VASC](lesions/VASC.md) — Vascular Lesions

These documents provide the medical and visual context needed to better understand the classification task.

## 🏗️ Model Architecture

The classification model is based on **ResNet50**, a deep convolutional neural network introduced for image recognition.
A pretrained ResNet50 backbone is used as the feature extractor, followed by a custom classification head adapted for the seven HAM10000 classes.

The ResNet50 architecture and the modifications used in this project are documented here:
👉 [ResNet50 Documentation](docs/ResNet50.md)

## 📊 Model Evaluation

Model performance is evaluated using several complementary approaches.

### Accuracy

Accuracy provides an overall measure of how many test samples are classified correctly.
👉 [Accuracy Analysis](Evaluation/Accuracy/README.md)

### Loss

Training and validation loss are analyzed to understand the optimization process and possible signs of underfitting or overfitting.
👉 [Loss Analysis](Evaluation/Loss/README.md)

### Confusion Matrix

The confusion matrix provides a class-level view of the model's predictions and makes it possible to identify which lesion categories are frequently confused with each other.
👉 [Confusion Matrix Analysis](Evaluation/Confusion%20Matrix/README.md)

## 🔎 Explainability

A central component of this project is **Explainable AI (XAI)**.
In medical imaging, understanding the regions that influence a model's prediction is particularly important. Therefore, this project uses two gradient-based visualization techniques:

### Grad-CAM

Grad-CAM identifies image regions that contribute to the model's prediction by using gradients flowing into the final convolutional layers.
👉 [Grad-CAM Analysis](Evaluation/GRADCAM/README.md)

### Grad-CAM++

Grad-CAM++ extends Grad-CAM and can provide more detailed localization, particularly when multiple relevant regions contribute to a prediction.
👉 [Grad-CAM++ Analysis](Evaluation/GRADCAM++/README.md)

These visualizations help investigate whether the model is focusing on clinically meaningful lesion regions rather than irrelevant image areas.

## 🩺 Trustworthy AI

Medical AI systems should not be evaluated solely based on predictive performance.
A trustworthy system should also consider:

- Explainability
- Transparency
- Reliability
- Robustness
- Interpretability
- Potential sources of bias
- Clinical relevance

The role of these concepts in this project is discussed in:
👉 [Trustworthy AI Documentation](docs/Trustworthy_AI.md)

## 🧩 Understanding Skin Lesion Classification

Before analyzing the model, it is useful to understand the underlying medical classification problem, the seven lesion categories, and the visual characteristics that distinguish them.
👉 [Skin Lesions Documentation](docs/Skin_Lesions.md)

## 📈 Evaluation & Explainability Overview

The repository organizes the experimental analysis into two major components:

### Quantitative Evaluation

```

Evaluation/
│
├── Accuracy
├── Loss
└── Confusion Matrix

```

These analyses answer: **How well does the model perform?**

### Explainability

```

Evaluation/
│
├── GRADCAM
└── GRADCAM++

```

These analyses answer: **Why does the model make its prediction?**

Combining these two perspectives provides a more complete evaluation of the model.

## 🎯 Project Goals

The main objectives of this project are:

- Build a deep learning model for seven-class skin lesion classification.
- Use a pretrained ResNet50 architecture for image representation and classification.
- Evaluate the model using quantitative metrics.
- Analyze class-specific errors using the confusion matrix.
- Visualize model attention using Grad-CAM.
- Compare model explanations using Grad-CAM++.
- Investigate whether the model focuses on relevant lesion regions.
- Connect model performance with principles of Trustworthy AI.

## 📚 Documentation

The repository documentation is organized into three main sections.

### Dataset & Theory

- [HAM10000](docs/HAM10000.md)
- [Skin Lesions](docs/Skin_Lesions.md)
- [ResNet50](docs/ResNet50.md)
- [Trustworthy AI](docs/Trustworthy_AI.md)

### Lesion Classes

- [AKIEC](lesions/AKIEC.md)
- [BCC](lesions/BCC.md)
- [BKL](lesions/BKL.md)
- [DF](lesions/DF.md)
- [MEL](lesions/MEL.md)
- [NV](lesions/NV.md)
- [VASC](lesions/VASC.md)

### Evaluation

- [Accuracy](Evaluation/Accuracy/README.md)
- [Loss](Evaluation/Loss/README.md)
- [Confusion Matrix](Evaluation/Confusion%20Matrix/README.md)
- [Grad-CAM](Evaluation/GRADCAM/README.md)
- [Grad-CAM++](Evaluation/GRADCAM++/README.md)

## 🔬 Research Perspective

This project is designed not only as a classification experiment but also as an exploration of explainable and trustworthy medical AI.

The central research question is:

> Can a deep learning model classify skin lesions accurately while also providing meaningful visual evidence for its predictions?

The combination of classification performance, class-level error analysis, and visual explanations provides a foundation for investigating this question.

