# Innovation Summary

This project compares two EEG/BCI decoding protocols built around the same Conformer-style backbone.

## Protocol Names

| Short Name | Full Name | Meaning |
|---|---|---|
| CSG-LOSO | Cross-Subject Generalization Protocol | Strict LOSO evaluation for unseen-subject generalization |
| RSDP | Repeated Subject-Dependent Protocol | Repeated within-subject evaluation for stronger subject-dependent performance |

## Innovation Comparison

| Component | CSG-LOSO | RSDP | Why it matters |
|---|---|---|---|
| Data protocol | Uses pure motor imagery runs: 4, 6, 8, 10, 12, 14 | Uses the same pure motor imagery runs | Keeps the task focused on imagined movement rather than mixed executed/imagined actions |
| Evaluation goal | Tests generalization to unseen subjects | Tests subject-dependent performance with repeated splits | Separates strict generalization from original-paper-style subject-dependent performance |
| Data split | One subject is held out as test subject | Each subject is split internally into train/validation/test | LOSO is harder; within-subject is closer to the original high-performance setting |
| Repetition | Pilot LOSO folds | 3 random split seeds per subject | Repeated evaluation reduces dependence on a single lucky or unlucky split |
| Standardization | Mean and standard deviation are fitted only on training subjects in each fold | Mean and standard deviation are fitted only on the training split of each subject | Prevents validation/test data from leaking into preprocessing |
| Data augmentation | Segmentation-reconstruction is applied only to training data | Segmentation-reconstruction is applied only to training data, with stronger augmentation | Increases training samples while keeping validation/test data clean |
| Model backbone | Lightweight Conformer-style model for stricter cross-subject testing | Higher-performance Conformer-style setting with deeper Transformer and repeated splits | The model capacity is adjusted according to evaluation purpose |
| Metrics | Accuracy, balanced accuracy, macro-F1, loss, confusion matrix | Accuracy, balanced accuracy, macro-F1, loss, confusion matrix | Gives a more complete evaluation than accuracy alone |
| Main insight | Cross-subject generalization is difficult | Subject-dependent performance can be much higher | Explains why original-paper-style results can be high while LOSO results are lower |

## Main Technical Contributions

### 1. Pure motor imagery protocol refinement

The pipeline uses EEGMMIDB runs 4, 6, 8, 10, 12, and 14. This creates a cleaner motor imagery decoding task and avoids mixing real executed movements with imagined actions.

### 2. Leakage-controlled preprocessing

The standardization parameters are fitted only from the training data in each protocol. Validation and test data are never used to compute normalization statistics.

### 3. Training-only segmentation-reconstruction augmentation

The project uses segmentation-reconstruction augmentation only on the training set. This helps address small EEG sample size without contaminating validation or test sets.

### 4. Two-level evaluation design

The project separates two different questions:

```text
CSG-LOSO: Can the model generalize to unseen subjects?
RSDP: How well can the model perform under subject-dependent evaluation?
```

This makes the results easier to explain and avoids unfair comparison between strict cross-subject evaluation and easier subject-dependent evaluation.

### 5. Expanded evaluation metrics

The project reports accuracy, balanced accuracy, macro-F1, loss, and confusion matrix. This is more informative than reporting only accuracy, especially for four-class EEG classification.

## Short Description for Resume or Interview

This project extends EEG-Conformer reproduction into a structured EEG/BCI decoding pipeline. I refined the dataset protocol to pure motor imagery runs, implemented training-only segmentation-reconstruction augmentation, added leakage-controlled standardization, and compared strict LOSO cross-subject evaluation with a repeated subject-dependent protocol. The results show that subject-dependent performance can be high, while unseen-subject generalization remains substantially more challenging.
