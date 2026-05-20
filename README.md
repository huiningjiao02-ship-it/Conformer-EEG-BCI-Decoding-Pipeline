# Conformer-based EEG/BCI Decoding Pipeline for Motor Imagery

## Overview

This project develops a modular EEG/BCI decoding pipeline for four-class motor imagery classification using a convolutional Transformer architecture.

The project was initially motivated by EEG-Conformer, but it is not limited to simple model reproduction. Instead, this upgraded version focuses on building a cleaner and more reliable EEG decoding workflow, including:

- Pure motor imagery protocol refinement
- Leakage-controlled preprocessing
- Training-only data augmentation
- Leave-One-Subject-Out cross-subject evaluation
- Training/validation curve analysis
- Small-sample overfitting analysis

The main purpose of this project is to examine how Transformer-based EEG decoding performs under a stricter and more realistic BCI evaluation setting.

---

## Task

The task is four-class motor imagery EEG decoding.

The model takes EEG trials as input and predicts one of the following imagined motor actions:

```text
0 = left fist imagery
1 = right fist imagery
2 = both fists imagery
3 = both feet imagery
```

In this project, the selected trials correspond to imagined motor actions rather than overt executed movements.

---

## Dataset

This project uses the PhysioNet EEG Motor Movement/Imagery Dataset, also known as EEGMMIDB.

The original prototype used a mixed motor-task setting. In this upgraded version, the protocol is refined to pure motor imagery runs:

```text
Runs: 4, 6, 8, 10, 12, 14
```

This setting focuses on imagined motor actions and avoids mixing executed movement trials with imagined movement trials.

Each EEG trial contains:

```text
64 EEG channels
Approximately 4 seconds of EEG signal
Four motor imagery classes
```

---

## Project Design

The project is organized around five parts:

```text
Framework
Data
Training
Evaluation
Insight
```

---

## 1. Framework

A modular EEG/BCI decoding pipeline was developed using PyTorch and MNE-Python.

The pipeline includes:

- EEG data loading
- Trial extraction
- Label assignment
- Preprocessing
- Data augmentation
- EEG-Conformer model training
- Leave-One-Subject-Out evaluation
- Metric reporting
- Visualization

The goal is to move beyond a simple reproduction notebook and build a reusable EEG decoding workflow.

---

## 2. Data

The EEGMMIDB protocol was refined from mixed executed/imagined motor-task runs to pure motor imagery runs.

The selected pure motor imagery runs are:

```text
4, 6, 8, 10, 12, 14
```

Run-dependent task labels are handled to assign EEG trials into four classes:

```text
left fist
right fist
both fists
both feet
```

This step ensures that the task definition is consistent with motor imagery BCI decoding.

---

## 3. Training

A leakage-controlled training workflow was implemented.

The workflow includes:

- Train/validation/test split before normalization
- Channel-wise standardization fitted only on the training set
- Training-only segmentation-reconstruction augmentation
- Validation-based early stopping
- Dropout regularization
- Reduced training epochs to control overfitting and improve runtime efficiency

The purpose of this training design is to avoid over-optimistic performance estimation and reduce small-sample overfitting.

---

## 4. Evaluation

Leave-One-Subject-Out cross-subject evaluation was implemented on a 30-subject setup.

In each LOSO fold:

```text
One subject is used as the unseen test subject.
The remaining subjects are used for training and validation.
```

This setting evaluates whether the model can generalize to unseen subjects, which is a key challenge in EEG-based BCI.

Evaluation metrics include:

- Accuracy
- Balanced accuracy
- Macro-F1
- Confusion matrix

Pilot LOSO experiments can be run with a limited number of folds:

```python
RUN_FOLDS = 3
```

A larger LOSO experiment can be run by increasing the number of folds:

```python
RUN_FOLDS = 10
```

Full 30-subject LOSO evaluation can be run by setting:

```python
RUN_FOLDS = None
```

---

## 5. Insight

Training and validation curves showed that the model can rapidly fit training trials while validation loss remains unstable.

This suggests that Transformer-based EEG decoding can be prone to overfitting under small-sample EEG settings.

To mitigate this issue, the pipeline includes:

- Validation-based early stopping
- Stronger dropout regularization
- Reduced training epochs
- Cross-subject evaluation
- Standardized metrics beyond simple accuracy

This project does not claim that Transformer-based EEG decoding is always superior. Instead, it uses EEG-Conformer as a case study to analyze the limitations and challenges of Transformer-based EEG decoding in small-sample BCI settings.

---

## Model

The model is based on a convolutional Transformer architecture.

It contains:

### Convolutional Front-end

The convolutional front-end extracts local temporal and spatial EEG features.

### Transformer Encoder

The Transformer encoder models temporal-token relationships through self-attention.

### Classification Head

The classification head predicts one of the four motor imagery classes.

---

## Current Status

This repository currently provides the core notebook implementation for:

- Pure motor imagery data loading
- Correct four-class trial assignment
- Leakage-controlled preprocessing
- Training-only augmentation
- EEG-Conformer training
- Leave-One-Subject-Out evaluation
- Training/validation curve analysis
- LOSO result summary
- LOSO visualization

Current experiments are conducted on a 30-subject pure motor imagery setup.

Pilot LOSO evaluation can be completed first to reduce runtime:

```python
RUN_FOLDS = 3
```

The number of folds can be increased after confirming runtime stability.

---

## Project Structure

```text
.
├── README.md
├── EEG_BCI_Conformer_Pipeline_LOSO.ipynb
├── requirements.txt
│
├── results/
│   ├── README.md
│   └── loso_partial_results.csv
│
├── figures/
│   ├── README.md
│   ├── training_validation_loss.png
│   ├── loso_accuracy.png
│   └── loso_macro_f1.png
│
└── docs/
    ├── README.md
    └── original_reproduction_report.pdf
```

---

## How to Run

### 1. Open the notebook

Open the following notebook in Google Colab:

```text
EEG_BCI_Conformer_Pipeline_LOSO.ipynb
```

### 2. Set GPU runtime

In Google Colab:

```text
Runtime → Change runtime type → Hardware accelerator → GPU
```

A T4 GPU is sufficient for pilot experiments.

### 3. Run the setup cells

Run the cells for:

- Package installation
- Imports
- Configuration
- Random seed setup

### 4. Load the data

The notebook will automatically load pure motor imagery runs:

```text
4, 6, 8, 10, 12, 14
```

For the 30-subject setup, the subject range is:

```python
subjects = tuple(range(1, 31))
```

### 5. Run LOSO evaluation

For quick testing:

```python
RUN_FOLDS = 3
```

For a larger pilot run:

```python
RUN_FOLDS = 10
```

For full 30-subject LOSO:

```python
RUN_FOLDS = None
```

---

## Dependencies

```text
Python
PyTorch
MNE-Python
NumPy
Pandas
Scikit-learn
Matplotlib
Einops
Tqdm
Google Colab GPU
```

To install dependencies:

```bash
pip install -r requirements.txt
```

---

## Results

Result files will be stored in:

```text
results/
```

Planned result files include:

```text
loso_partial_results.csv
loso_final_results.csv
loso_summary.csv
```

Figures will be stored in:

```text
figures/
```

Planned figures include:

```text
training_validation_loss.png
loso_accuracy.png
loso_macro_f1.png
loso_accuracy_distribution.png
```

At the current stage, pilot LOSO results can be used to verify the pipeline before running the full 30-subject evaluation.

---

## Notes on Interpretation

The main focus of this project is not only accuracy.

The project also emphasizes:

- Whether the evaluation protocol is clean
- Whether data leakage is avoided
- Whether the model generalizes to unseen subjects
- Whether the training curve shows overfitting
- Whether Transformer-based EEG decoding is stable under small-sample settings

Low or unstable LOSO accuracy is still informative because cross-subject EEG generalization is a difficult and important BCI problem.

---

## Future Work

Future improvements include:

- Run full 30-subject LOSO evaluation
- Add CNN baselines such as EEGNet and ShallowConvNet
- Conduct Transformer-depth ablation
- Compare Transformer depth and attention heads
- Add subject-adaptive fine-tuning
- Explore domain adaptation for cross-subject EEG decoding
- Extend the pipeline toward pseudo-online BCI command feedback

---

## Keywords

```text
EEG
BCI
Motor Imagery
EEG-Conformer
Transformer
PyTorch
MNE-Python
Deep Learning
Cross-subject Evaluation
Leave-One-Subject-Out
LOSO
Small-sample Overfitting
```
