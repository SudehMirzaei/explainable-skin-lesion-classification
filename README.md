# Explainable Skin Lesion Classification

An end-to-end deep learning project for **multi-class skin lesion classification** using **ResNet50**, **Transfer Learning**, and **Explainable Artificial Intelligence (XAI)** techniques on the **HAM10000** dermoscopic image dataset.

The primary goal of this project is not only to achieve high classification performance but also to improve the **interpretability**, **transparency**, and **trustworthiness** of AI models used for medical image analysis.

---

# Project Objectives

- Multi-class skin lesion classification
- Transfer Learning with ResNet50
- Handling class imbalance using Weighted CrossEntropy Loss
- Learning Rate Scheduling
- Early Stopping
- Model evaluation using multiple metrics
- Explainability using Grad-CAM
- Building a Trustworthy AI pipeline for medical image analysis

---

# Dataset

This project uses the **HAM10000 (Human Against Machine with 10,000 Training Images)** dataset.

The dataset contains dermoscopic images belonging to seven diagnostic categories.

For detailed information, see:

📄 **Dataset Documentation**

➡️ [docs/HAM10000.md](docs/HAM10000.md)

---

# Skin Lesion Classes

The dataset consists of seven diagnostic categories.

| Class | Description |
|--------|-------------|
| AKIEC | Actinic Keratoses and Intraepithelial Carcinoma |
| BCC | Basal Cell Carcinoma |
| BKL | Benign Keratosis-like Lesions |
| DF | Dermatofibroma |
| MEL | Melanoma |
| NV | Melanocytic Nevus |
| VASC | Vascular Lesions |

Detailed medical and AI-oriented documentation is available for every lesion.

| Lesion | Documentation |
|----------|--------------|
| AKIEC | [medical/AKIEC.md](medical/AKIEC.md) |
| BCC | [medical/BCC.md](medical/BCC.md) |
| BKL | [medical/BKL.md](medical/BKL.md) |
| DF | [medical/DF.md](medical/DF.md) |
| MEL | [medical/MEL.md](medical/MEL.md) |
| NV | [medical/NV.md](medical/NV.md) |
| VASC | [medical/VASC.md](medical/VASC.md) |

Each documentation includes:

- Medical definition
- Risk factors
- Clinical presentation
- Dermoscopic features
- CNN perspective
- Histopathology
- Disease progression
- Differential diagnosis
- Explainability (Grad-CAM)
- AI challenges
- Summary

---

# Deep Learning Model

The classification model is based on **ResNet50** pretrained on ImageNet.

Main components include:

- Transfer Learning
- Frozen Backbone
- Custom Classification Head
- Weighted CrossEntropy Loss
- Adam Optimizer
- ReduceLROnPlateau Scheduler
- Early Stopping

More details:

➡️ [docs/ResNet50.md](docs/ResNet50.md)

---

# Explainable AI

Understanding **why** a CNN makes a prediction is essential for clinical applications.

This repository uses **Grad-CAM** to visualize the image regions responsible for model predictions.

Documentation:

- [docs/GradCAM.md](docs/GradCAM.md)
- [docs/Trustworthy_AI.md](docs/Trustworthy_AI.md)

---

# Model Evaluation

Model performance is evaluated using several complementary metrics.

## Classification Report

- Precision
- Recall
- F1-score
- Support

---

## Confusion Matrix

The confusion matrix provides a detailed analysis of class-wise performance and common misclassifications.

📄 Documentation

➡️ [images/evaluation/confusion_matrix/README.md](images/evaluation/confusion_matrix/README.md)

---

## Accuracy Curve

The accuracy curve illustrates how the model learns throughout training and whether it generalizes well.

📄 Documentation

➡️ [images/evaluation/accuracy_curve/README.md](images/evaluation/accuracy_curve/README.md)

---

## Loss Curve

The loss curve provides insight into optimization, convergence, and overfitting.

📄 Documentation

➡️ [images/evaluation/loss_curve/README.md](images/evaluation/loss_curve/README.md)

---

# Project Structure

```text
explainable-skin-lesion-classification/

├── docs/
│   ├── HAM10000.md
│   ├── Skin_Lesion.md
│   ├── ResNet50.md
│   ├── GradCAM.md
│   ├── Trustworthy_AI.md
│
├── medical/
│   ├── AKIEC.md
│   ├── BCC.md
│   ├── BKL.md
│   ├── DF.md
│   ├── MEL.md
│   ├── NV.md
│   └── VASC.md
│
├── images/
│   └── evaluation/
│       ├── confusion_matrix/
│       ├── accuracy_curve/
│       └── loss_curve/
│
├── train.py
├── requirements.txt
└── README.md
```

---

# Results

The ResNet50 model achieved an overall classification accuracy of approximately **74%** on the HAM10000 validation dataset.

Major findings include:

- Strong performance on **NV**, **BCC**, and **VASC**
- Moderate performance on **DF**
- Greater difficulty distinguishing **MEL**, **BKL**, and **AKIEC**
- Most common misclassification: **Melanoma ↔ Nevus**
- Stable training without severe overfitting
- Good convergence using ReduceLROnPlateau and Early Stopping

Detailed evaluation can be found in the corresponding documentation.

---

# Technologies

- Python
- PyTorch
- Torchvision
- NumPy
- Pandas
- Matplotlib
- Scikit-learn
- OpenCV

---

# Future Work

Possible future improvements include:

- EfficientNet
- DenseNet121
- Vision Transformer (ViT)
- ConvNeXt
- Swin Transformer
- Focal Loss
- Test-Time Augmentation
- Ensemble Learning
- SHAP
- LIME
- Attention Maps
- Clinical Metadata Integration

---

# References

- HAM10000 Dataset
- ImageNet
- ResNet50
- Grad-CAM
- PyTorch Documentation

---

# License

This project is intended for educational and research purposes.

It should not be used as a standalone clinical decision-making system without professional medical supervision.
