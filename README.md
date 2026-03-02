# Histopathology Tissue Classification

Classification of histopathology tissue images (pathMNIST) into five tissue types using Logistic Regression, Random Forest, and a custom CNN built with PyTorch. Achieved 93.36% test accuracy with the CNN compared to ~67% for sklearn baselines.

## Dataset

The pathMNIST dataset consists of 55,490 training images and 4,367 test images of haematoxylin and eosin (H&E) stained colorectal tissue. Each image is 28x28 pixels with 3 RGB colour channels, labelled across five tissue classes:

- Adipose
- Lymphocytes
- Normal Colon Mucosa
- Cancer-Associated Stroma
- Colorectal Adenocarcinoma Epithelium

The training and test sets are derived from different spatial locations within the tissue.

## Methods

### Preprocessing
- RGB channels retained (no grayscale conversion) to preserve diagnostic colour information from H&E staining
- Pixel values normalised to [0, 1]
- Stratified 80/20 train/validation split
- Images flattened to 2,352-dimensional vectors for sklearn models

### Baseline Models (sklearn)
- **Logistic Regression** with StandardScaler pipeline (max_iter=5,000)
- **Random Forest** with 100 trees as an additional non-linear baseline

### CNN Architecture (PyTorch)
- 3 convolutional blocks (32 → 64 → 128 filters) with BatchNorm, ReLU, and MaxPooling
- Dropout (0.25) for regularisation
- Two fully connected layers (1,152 → 256 → 5)
- Adam optimiser with lr=0.0001
- StepLR scheduler (halving every 5 epochs)
- Trained for 20 epochs with batch size 64

## Results

| Model               | Test Accuracy | Macro F1 |
|---------------------|---------------|----------|
| Logistic Regression | 67.28%        | 0.63     |
| Random Forest       | 67.67%        | 0.61     |
| CNN                 | 93.36%        | 0.92     |

## Key Findings

- The CNN substantially outperformed both sklearn baselines across all tissue classes
- Sklearn models struggled most with normal mucosa and cancer epithelium due to spatial information lost through flattening
- The CNN retained high performance on the test set because convolutional layers learn structural features (edges, textures, spatial arrangements) that generalise across tissue locations
- Including Random Forest confirmed that the performance gap is due to data representation rather than model complexity

## References

- Kather, J.N., et al. (2016). Multi-class texture analysis in colorectal cancer histology. *Scientific Reports*, 6, 27988.
- Yang, J., et al. (2023). MedMNIST v2: A large-scale lightweight benchmark for 2D and 3D biomedical image classification. *Scientific Data*, 10(1), 41.

