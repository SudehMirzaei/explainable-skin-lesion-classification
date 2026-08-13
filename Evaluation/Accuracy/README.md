# Accuracy Curve Analysis

## Overview

The accuracy curve illustrates how the **ResNet50** model learns throughout the training process on the HAM10000 skin lesion dataset. By comparing the **training accuracy** and **validation accuracy** over multiple epochs, we can assess whether the model is learning meaningful features, converging properly, or suffering from underfitting or overfitting.

Unlike the confusion matrix, which evaluates the final classifier, the accuracy curve provides insight into the **learning dynamics** of the model during optimization.

---

# Training Configuration

| Parameter | Value |
|-----------|-------|
| Backbone | ResNet50 |
| Dataset | HAM10000 |
| Number of Classes | 7 |
| Optimizer | Adam |
| Loss Function | CrossEntropy Loss (Class Weighted) |
| Learning Rate Scheduler | ReduceLROnPlateau |
| Early Stopping | Patience = 5 |
| Epochs | 15 |

---

# Understanding the Accuracy Curve

The figure contains two curves:

- **Training Accuracy (Blue):** Classification accuracy measured on the training dataset after each epoch.
- **Validation Accuracy (Orange):** Classification accuracy measured on the validation dataset after each epoch.

The objective of training is for both curves to increase and eventually stabilize while remaining relatively close to one another.

---

# Overall Learning Trend

Both training and validation accuracies steadily increase during training.

Approximate values:

| Epoch | Train Accuracy | Validation Accuracy |
|--------|---------------:|--------------------:|
| 1 | 53% | 60% |
| 5 | 64% | 68% |
| 10 | 66% | 72% |
| 13 | 67% | 74% |
| 15 | 68% | 70% |

This trend demonstrates that the CNN gradually learns discriminative dermoscopic features from the training images.

---

# Why is Validation Accuracy Higher?

An interesting observation is that the validation accuracy remains slightly higher than the training accuracy during most epochs.

Normally, one expects:

```
Training Accuracy ≥ Validation Accuracy
```

However, the opposite behavior is not uncommon and can be explained by several factors.

## 1. Data Augmentation

The training images are typically augmented using techniques such as:

- Random rotation
- Horizontal flipping
- Vertical flipping
- Color jitter
- Random cropping

These transformations make the training task more difficult, reducing training accuracy while improving the model's generalization ability.

Since validation images are not augmented, the model often achieves higher validation accuracy.

---

## 2. Regularization

During training, layers such as:

- Dropout
- Batch Normalization

operate in training mode.

During validation, the model switches to:

```python
model.eval()
```

which disables dropout and uses stable Batch Normalization statistics.

Consequently, validation accuracy may become slightly higher.

---

## 3. Validation Dataset Characteristics

Another possible explanation is that the validation subset may contain slightly easier examples than the augmented training data.

---

# Temporary Performance Drop

Around **Epoch 8**, the validation accuracy decreases noticeably before recovering in subsequent epochs.

Possible reasons include:

- Stochastic optimization noise
- Mini-batch variability
- Learning rate adjustment
- Class imbalance
- Random augmentation effects

Because the validation accuracy quickly recovers, this fluctuation does not indicate instability.

---

# Best Validation Performance

The highest validation accuracy is achieved around **Epoch 13–14**, reaching approximately:

```
74%
```

At this point, the training script saves the model weights:

```python
torch.save(best_model, "best_resnet50_skin.pth")
```

This ensures that the best-performing model is preserved even if later epochs produce lower validation performance.

---

# Does the Model Overfit?

One of the primary purposes of the accuracy curve is to detect overfitting.

Typical overfitting appears as:

```
Training Accuracy  ↑↑↑

Validation Accuracy ↓↓↓
```

This behavior is **not observed**.

Instead:

- Both curves increase steadily.
- The gap between them remains relatively small.
- Validation accuracy does not collapse.

Therefore, there is **no evidence of severe overfitting**.

---

# Model Convergence

The accuracy curves gradually flatten after approximately **Epoch 10**.

This indicates that:

- the CNN has learned most discriminative visual features,
- additional epochs would likely produce only marginal improvements,
- optimization is approaching convergence.

---

# Clinical Interpretation

Increasing accuracy indicates that the network progressively learns medically relevant visual patterns such as:

- lesion asymmetry
- border irregularity
- color variation
- pigment distribution
- vascular structures
- texture
- lesion-background contrast

These features are fundamental for differentiating the seven diagnostic categories in the HAM10000 dataset.

---

# AI Perspective

From a deep learning perspective, the smooth increase in both curves indicates that ResNet50 successfully extracts hierarchical image representations.

Early layers learn:

- edges
- color gradients
- simple textures

Intermediate layers learn:

- pigment structures
- lesion borders
- vascular patterns

Deeper layers learn:

- disease-specific semantic features
- lesion morphology
- global structural relationships

The consistent increase in validation accuracy suggests that these learned representations generalize well to unseen dermoscopic images.

---

# Effect of Learning Rate Scheduler

The training process uses:

```python
ReduceLROnPlateau
```

which automatically decreases the learning rate when validation loss stops improving.

This strategy:

- stabilizes optimization,
- reduces oscillations,
- allows the optimizer to converge toward a better local minimum,
- contributes to the smooth behavior of the accuracy curves.

---

# Effect of Early Stopping

The project also employs an Early Stopping mechanism.

Once validation loss no longer improves for several consecutive epochs, training terminates automatically.

This prevents:

- unnecessary computation,
- excessive parameter updates,
- potential overfitting.

Although the training was configured for 15 epochs, the model had already reached near-optimal performance by approximately Epoch 13–14.

---

# Summary

The accuracy curve demonstrates a stable and well-behaved learning process.

Key observations include:

- Continuous improvement in both training and validation accuracy.
- No evidence of severe overfitting.
- Small gap between training and validation curves.
- Validation accuracy slightly exceeds training accuracy, likely due to data augmentation and regularization.
- Stable convergence after approximately Epoch 10.
- Best validation accuracy of approximately **74%** achieved near Epoch 13–14.

Overall, the accuracy curve indicates that the ResNet50 model successfully learned discriminative dermoscopic representations while maintaining good generalization performance on unseen validation images.


