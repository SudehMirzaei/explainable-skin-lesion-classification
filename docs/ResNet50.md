# ResNet50

# Introduction

ResNet50 (Residual Network with 50 layers) is one of the most influential deep convolutional neural network architectures in computer vision. Proposed by Microsoft Research in 2015, ResNet introduced the concept of **Residual Learning**, enabling the successful training of very deep neural networks without suffering from the vanishing gradient problem.

Due to its excellent balance between accuracy, computational efficiency, and transfer learning capability, ResNet50 has become one of the most widely used backbone networks in medical image analysis, including skin lesion classification.

In this project, ResNet50 serves as the feature extraction backbone for classifying dermoscopic images from the HAM10000 dataset into seven diagnostic categories.

---

# Why ResNet50?

Traditional CNNs become increasingly difficult to train as their depth increases.

Adding more layers often results in:

- Vanishing gradients
- Exploding gradients
- Slower convergence
- Reduced optimization performance
- Accuracy degradation

ResNet addresses these challenges through **Residual Learning**, allowing much deeper networks to be trained effectively.

---

# What Does "50" Mean?

The number **50** refers to the total number of learnable layers in the network.

The architecture consists of:

- Convolutional layers
- Batch Normalization layers
- Residual Blocks
- Global Average Pooling
- Fully Connected layer

Although called "ResNet50", the network contains many additional operations such as ReLU activations and shortcut connections.

---

# Residual Learning

The key innovation of ResNet is learning the **residual function** instead of directly learning the desired mapping.

Instead of learning:

```
H(x)
```

the network learns:

```
F(x) = H(x) − x
```

and computes:

```
Output = F(x) + x
```

where:

- **x** is the input feature map.
- **F(x)** is the residual mapping.
- **Output** is the final feature representation.

This identity shortcut allows gradients to flow directly through the network during backpropagation.

---

# Skip Connections

Skip (shortcut) connections bypass one or more convolutional layers and add the input directly to the output of a residual block.

```
Input
   │
   ├───────────────┐
   │               │
Conv → BN → ReLU   │
Conv → BN          │
   │               │
   └──── Add ◄─────┘
        │
      ReLU
```

Benefits include:

- Improved gradient flow
- Faster convergence
- Easier optimization
- Better feature reuse
- Reduced degradation in very deep networks

---

# Bottleneck Residual Block

ResNet50 uses **Bottleneck Blocks**, which are computationally efficient.

Each bottleneck consists of:

```
1×1 Convolution
↓

3×3 Convolution
↓

1×1 Convolution
↓

Skip Connection

↓

Addition

↓

ReLU
```

### Purpose of Each Layer

**1×1 Convolution**

- Reduces the number of channels.
- Decreases computational cost.

**3×3 Convolution**

- Extracts local spatial features.
- Detects textures, edges, and patterns.

**1×1 Convolution**

- Restores feature dimensions.
- Combines learned information.

---

# Overall Architecture

The ResNet50 architecture can be summarized as:

```
Input Image

↓

7×7 Convolution

↓

Max Pooling

↓

Residual Stage 1

↓

Residual Stage 2

↓

Residual Stage 3

↓

Residual Stage 4

↓

Global Average Pooling

↓

Fully Connected Layer

↓

Softmax
```

---

# Feature Hierarchy

ResNet50 learns hierarchical visual representations.

### Early Layers

Detect:

- edges
- corners
- color gradients
- simple textures

---

### Intermediate Layers

Learn:

- pigmentation
- lesion borders
- vascular structures
- dermoscopic patterns
- texture variations

---

### Deep Layers

Capture:

- lesion morphology
- semantic disease features
- class-specific visual characteristics
- global lesion structure

These high-level representations are essential for distinguishing visually similar skin lesions.

---

# Transfer Learning

Training a deep CNN from scratch requires millions of labeled images.

Medical datasets are usually much smaller.

Therefore, this project uses **Transfer Learning**.

The ResNet50 backbone is initialized with weights pretrained on the ImageNet dataset.

Benefits include:

- Faster convergence
- Improved feature extraction
- Better generalization
- Reduced training time
- Higher accuracy

---

# Frozen Backbone

Initially, pretrained convolutional layers can be frozen so that only the classification head is trained.

Advantages include:

- Prevents destruction of pretrained features.
- Reduces overfitting.
- Requires fewer training samples.
- Accelerates training.

Later, additional layers may be unfrozen for fine-tuning.

---

# Classification Head

The original ImageNet classifier predicts 1000 categories.

For this project, it is replaced with a custom classification head.

Example:

```python
num_features = model.fc.in_features

model.fc = nn.Sequential(
    nn.Linear(num_features, 512),
    nn.ReLU(),
    nn.Dropout(0.3),
    nn.Linear(512, 7)
)
```

This modification enables the network to classify the seven skin lesion categories in HAM10000.

---

# Why ResNet50 for Skin Lesion Classification?

Dermoscopic images contain subtle visual patterns that require a deep feature extractor.

ResNet50 is well suited because it can learn:

- pigment networks
- globules
- dots
- streaks
- vascular structures
- blue-white veil
- lesion borders
- asymmetry
- texture variations
- color distributions

These features are clinically relevant for distinguishing benign and malignant lesions.

---

# Training Strategy

The model is trained using:

- Transfer Learning
- Adam Optimizer
- Weighted Cross-Entropy Loss
- ReduceLROnPlateau Scheduler
- Early Stopping

This combination improves convergence while addressing class imbalance and reducing overfitting.

---

# Explainability with Grad-CAM

ResNet50 is compatible with Explainable AI techniques such as Grad-CAM.

Grad-CAM visualizes the regions of the image that contribute most strongly to the model's prediction.

In skin lesion classification, heatmaps should ideally focus on:

- lesion borders
- pigment network
- irregular pigmentation
- vascular structures
- clinically significant dermoscopic patterns

This improves transparency and supports trustworthy AI.

---

# Advantages

ResNet50 offers several advantages for medical image classification:

- Excellent transfer learning performance
- Deep hierarchical feature extraction
- Residual learning prevents vanishing gradients
- Stable optimization
- Strong generalization
- Well supported in PyTorch
- Extensive validation in medical AI research
- Compatible with Explainable AI methods

---

# Limitations

Despite its strengths, ResNet50 has some limitations:

- Computationally intensive
- Requires GPU for efficient training
- Larger memory footprint than lightweight models
- Not specifically designed for medical imaging
- May struggle with severe class imbalance without additional techniques
- Can learn dataset artifacts if preprocessing is insufficient

---

# ResNet50 in This Project

In this repository, ResNet50 serves as the backbone for multiclass classification of dermoscopic skin lesions from the HAM10000 dataset.

The pretrained convolutional layers extract hierarchical visual features, while a custom classification head predicts one of seven diagnostic categories.

The model is optimized using weighted cross-entropy loss, adaptive learning rate scheduling, and early stopping. Performance is evaluated using accuracy, precision, recall, F1-score, confusion matrix, and Grad-CAM visualizations to ensure both predictive performance and interpretability.

---

# Summary

ResNet50 is a powerful deep convolutional neural network that combines residual learning with transfer learning to achieve strong performance on medical image classification tasks.

Its ability to learn rich hierarchical representations, combined with stable optimization and compatibility with Explainable AI techniques, makes it an excellent choice for automated skin lesion classification. In this project, ResNet50 forms the foundation of a trustworthy AI pipeline that balances predictive accuracy with model interpretability.

