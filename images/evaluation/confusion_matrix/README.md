# Confusion Matrix Analysis

## Introduction

A confusion matrix provides a much deeper understanding of a classification model than overall accuracy alone. While the ResNet50 model achieved an overall **74% accuracy** on the HAM10000 skin lesion dataset, the confusion matrix reveals **which lesion types are recognized reliably, which classes are frequently confused, and why these misclassifications occur**.

For medical image classification, this analysis is particularly important because different types of errors have different clinical consequences.

---

# Understanding the Confusion Matrix

The confusion matrix is organized as follows:

- **Rows** represent the **true labels**.
- **Columns** represent the **predicted labels**.
- Values along the **main diagonal** correspond to correctly classified images.
- Values outside the diagonal indicate **misclassifications**.

A stronger classifier produces a confusion matrix where most samples are concentrated along the diagonal.

---

# Overall Performance

According to the classification report:

**Overall Accuracy = 74%**

The confusion matrix shows that the majority of images are correctly classified, indicating that the ResNet50 model learned meaningful visual representations of dermoscopic images.

However, several lesion types remain difficult to distinguish because of their similar visual characteristics.

---

# Class-wise Analysis

## 1. AKIEC (Actinic Keratoses and Intraepithelial Carcinoma)

Confusion Matrix Row

```
37   12   2   4   9   1   0
```

Out of **65** AKIEC images:

- ✅ Correctly classified: **37**
- Misclassified as BCC: **12**
- Misclassified as MEL: **9**
- Misclassified as DF: **4**
- Other mistakes: **3**

### Recall

```
37 / 65 ≈ 57%
```

This matches the recall reported in the classification report.

### Why does this happen?

AKIEC shares several dermoscopic characteristics with Basal Cell Carcinoma, including:

- Erythematous appearance
- Surface scaling
- Hyperkeratosis
- Irregular borders

In early stages, AKIEC may also resemble Melanoma because both lesions can exhibit irregular pigmentation.

---

# 2. BCC (Basal Cell Carcinoma)

Confusion Matrix Row

```
8   75   1   2   9   3   5
```

Out of **103** BCC images:

- ✅ Correct predictions: **75**

### Recall

```
75 / 103 ≈ 73%
```

The model performs well on this class.

### Most common mistakes

- BCC → MEL
- BCC → AKIEC

### Medical explanation

Pigmented Basal Cell Carcinoma often contains dark pigmentation, making it visually similar to Melanoma in dermoscopic images.

---

# 3. BKL (Benign Keratosis-like Lesions)

Confusion Matrix Row

```
29   19   112   6   24   28   2
```

Out of **220** images:

- ✅ Correct predictions: **112**

### Recall

```
112 / 220 ≈ 51%
```

This is relatively low.

### Most frequent confusions

- BKL → AKIEC
- BKL → NV
- BKL → MEL

### Medical explanation

Benign Keratosis represents a heterogeneous group of lesions.

Its appearance varies considerably between patients, leading to significant overlap with melanocytic lesions.

---

# 4. DF (Dermatofibroma)

Confusion Matrix Row

```
1   1   0   16   2   3   0
```

Out of **23** images:

- ✅ Correct predictions: **16**

### Recall

```
16 / 23 ≈ 70%
```

Although recall is acceptable, precision is relatively low because images from other classes are sometimes predicted as DF.

The limited number of DF training samples also contributes to this issue.

---

# 5. MEL (Melanoma)

Confusion Matrix Row

```
18   16   14   4   122   45   4
```

Out of **223** Melanoma images:

- ✅ Correct predictions: **122**

### Recall

```
122 / 223 ≈ 55%
```

This is one of the most important classes in the dataset because Melanoma is a highly aggressive skin cancer.

### Largest Misclassification

```
MEL → NV = 45 images
```

### Why is Melanoma confused with Nevus?

Both lesions often share:

- Brown pigmentation
- Pigment network
- Similar lesion size
- Comparable borders
- Similar global symmetry

Even experienced dermatologists sometimes require histopathological examination to distinguish between these lesions.

---

# 6. NV (Melanocytic Nevus)

Confusion Matrix Row

```
10   34   31   25   120   1103   18
```

Out of **1341** images:

- ✅ Correct predictions: **1103**

### Recall

```
1103 / 1341 ≈ 82%
```

The model performs very well on this class.

### Largest Misclassification

```
NV → MEL = 120 images
```

This means that 120 benign nevi were classified as Melanoma.

### Clinical Interpretation

From a screening perspective, this type of error is generally preferable to the opposite scenario.

Misclassifying a benign nevus as melanoma leads to additional clinical evaluation.

However, misclassifying melanoma as a benign nevus may delay treatment of a potentially life-threatening cancer.

Therefore, reducing **false negatives** for melanoma is generally considered more important than reducing false positives.

---

# 7. VASC (Vascular Lesions)

Confusion Matrix Row

```
0   3   0   2   0   0   23
```

Out of **28** images:

- ✅ Correct predictions: **23**

### Recall

```
23 / 28 ≈ 82%
```

The model demonstrates strong sensitivity for vascular lesions.

However, precision remains lower because several images from other classes are incorrectly predicted as VASC.

---

# Overall Misclassification Patterns

The confusion matrix reveals several dominant error patterns.

## MEL ↔ NV

```
MEL → NV = 45

NV → MEL = 120
```

This is the most significant challenge in the dataset.

Both lesion types belong to melanocytic lesions and share many dermoscopic characteristics.

---

## BKL ↔ NV

```
28 images
```

Expected because benign keratoses often contain pigmentation resembling melanocytic nevi.

---

## BKL ↔ MEL

```
24 images
```

Likely caused by similar pigment distribution and irregular structures.

---

## AKIEC ↔ BCC

```
12 images
```

These lesions share clinical features such as:

- Hyperkeratosis
- Scaling
- Irregular erythematous appearance

---

# Strengths of the Model

The ResNet50 classifier performs particularly well for:

- ✅ Melanocytic Nevus (NV)
- ✅ Basal Cell Carcinoma (BCC)
- ✅ Vascular Lesions (VASC)

Moderate performance:

- Dermatofibroma (DF)

More challenging classes:

- AKIEC
- BKL
- MEL

---

# Consistency with the Classification Report

| Class | Recall | Correct Predictions |
|--------|--------|--------------------|
| AKIEC | 57% | 37 / 65 |
| BCC | 73% | 75 / 103 |
| BKL | 51% | 112 / 220 |
| DF | 70% | 16 / 23 |
| MEL | 55% | 122 / 223 |
| NV | 82% | 1103 / 1341 |
| VASC | 82% | 23 / 28 |

The confusion matrix is fully consistent with the reported recall values.

---

# AI Perspective

From a deep learning perspective, the confusion matrix indicates that ResNet50 has greater difficulty distinguishing **melanocytic lesions** than non-melanocytic lesions.

The most problematic lesion groups are:

- Melanoma (MEL)
- Melanocytic Nevus (NV)
- Benign Keratosis-like Lesions (BKL)

These classes exhibit highly similar:

- Color distribution
- Pigment network
- Border morphology
- Texture
- Dermoscopic structures

In contrast, lesion types such as **Basal Cell Carcinoma (BCC)** and **Vascular Lesions (VASC)** possess more distinctive dermoscopic features (e.g., arborizing vessels and vascular patterns), making them easier for the CNN to recognize.

---

# Potential Improvements

The confusion matrix suggests several possible improvements for future versions of the model.

- Apply targeted **Data Augmentation** for underrepresented classes such as AKIEC, DF, and VASC.
- Replace standard Cross-Entropy Loss with **Focal Loss** or **Class-Balanced Loss** to reduce the impact of class imbalance.
- Use **Grad-CAM** to verify whether the CNN focuses on clinically meaningful dermoscopic structures such as pigment networks, blue-white veil, vascular structures, regression areas, and lesion borders.
- Perform detailed analysis of frequently confused lesion pairs, particularly **MEL ↔ NV** and **BKL ↔ MEL**, to better understand the limitations of the learned visual representations.

---

# Conclusion

The confusion matrix demonstrates that the ResNet50 model successfully learned discriminative features for several skin lesion categories while still encountering challenges in distinguishing visually similar melanocytic lesions.

Although the overall classification accuracy reached **74%**, the detailed analysis reveals that model performance varies substantially across lesion types. Such analyses are essential for developing trustworthy medical AI systems because they highlight clinically important failure modes that cannot be observed from overall accuracy alone.

