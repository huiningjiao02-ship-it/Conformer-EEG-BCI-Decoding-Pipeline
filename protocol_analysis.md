# Protocol Analysis

## Purpose

This document explains the protocol changes in the Conformer-based EEG/BCI decoding pipeline.

The goal is to move from a simple EEG-Conformer reproduction toward a cleaner and more reliable motor imagery EEG decoding workflow.

## 1. From Mixed Motor Tasks to Pure Motor Imagery

The original prototype used runs 11, 12, 13, and 14.

However, these runs mix executed movement and imagined movement:

| Run | Task Type | Content |
|---|---|---|
| 11 | Executed movement | left/right fist |
| 12 | Motor imagery | imagined left/right fist |
| 13 | Executed movement | both fists/both feet |
| 14 | Motor imagery | imagined both fists/both feet |

For a project focused on motor imagery decoding, mixing executed and imagined movements makes the task definition less clean.

Therefore, the upgraded version uses pure motor imagery runs:

```text
4, 6, 8, 10, 12, 14
```

These runs correspond to imagined motor actions.

## 2. Trial Label Assignment

In EEGMMIDB, the event labels T1 and T2 do not always mean the same thing.

Their meaning depends on the run type.

For left/right fist imagery runs:

```text
T1 = left fist
T2 = right fist
```

For both fists/both feet imagery runs:

```text
T1 = both fists
T2 = both feet
```

Therefore, the code applies run-dependent label mapping to assign each EEG trial to the correct four-class label.

This is not an algorithmic innovation, but it is important for protocol correctness.

## 3. Leakage-controlled Preprocessing

Data leakage happens when information from validation or test data enters the training process.

To avoid this, the pipeline follows this order:

1. Split data into training, validation, and test sets.
2. Compute channel-wise mean and standard deviation using the training set only.
3. Apply the training-set normalization statistics to validation and test sets.

This prevents validation/test information from being used during training.

## 4. Training-only Augmentation

The pipeline uses segmentation-reconstruction augmentation.

This method creates augmented EEG trials by stitching temporal segments from trials of the same class.

Important rule:

```text
Only training trials are used for augmentation.
```

Validation and test trials are never used for augmentation.

This avoids test-set contamination and makes evaluation more credible.

## 5. Early Stopping

The model may quickly fit the training set while validation loss remains unstable.

To reduce overfitting, the pipeline uses validation-based early stopping.

If validation performance does not improve for several epochs, training stops early.

This avoids unnecessary training and reduces the risk of memorizing training trials.

## 6. LOSO Cross-subject Evaluation

LOSO means Leave-One-Subject-Out.

For a 30-subject setup:

- One subject is held out as the unseen test subject.
- The remaining 29 subjects are used for training and validation.
- This process can be repeated for multiple held-out subjects.

LOSO is harder than within-subject evaluation because the model must generalize to unseen subjects.

This is closer to real BCI scenarios.

## 7. Overfitting Insight

Training and validation curves are used to inspect model behavior.

A typical overfitting pattern is:

```text
Training loss decreases rapidly.
Validation loss remains high or unstable.
```

This means the model fits training trials but does not generalize well.

In this project, this observation is used as an important insight:

```text
Transformer-based EEG decoding can be prone to overfitting under small-sample EEG settings.
```

## Summary

The upgraded protocol improves the project in five aspects:

| Component | Improvement |
|---|---|
| Framework | modular EEG/BCI decoding pipeline |
| Data | pure motor imagery runs |
| Training | leakage control, train-only augmentation, early stopping |
| Evaluation | LOSO cross-subject validation |
| Insight | overfitting analysis in small-sample EEG decoding |
