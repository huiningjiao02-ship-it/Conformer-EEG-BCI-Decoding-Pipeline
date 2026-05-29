# Conformer-EEG-BCI-Decoding-Pipeline

## Overview

This repository contains a modular EEG/BCI decoding pipeline based on a Conformer-style convolutional Transformer model for four-class motor imagery classification.

The project is not only a reproduction of EEG-Conformer. It reorganizes the experiment into two complementary protocols:

1. **Cross-Subject Generalization Protocol (CSG-LOSO)**  
   A stricter Leave-One-Subject-Out setting used to test whether the model can generalize to unseen subjects.

2. **Repeated Subject-Dependent Protocol (RSDP)**  
   A repeated within-subject setting designed to provide a higher-performance subject-dependent comparison that is closer to the original EEG-Conformer evaluation style.

## Task

The model predicts four imagined motor actions from EEG trials:

```text
0 = left_fist
1 = right_fist
2 = both_fists
3 = both_feet
```

## Dataset Protocol

The project uses EEGMMIDB pure motor imagery runs:

```text
Runs: 4, 6, 8, 10, 12, 14
```

This avoids mixing executed movement trials with imagined motor imagery trials.

## Repository Files

```text
README.md
INNOVATION.md
RESULTS.md
requirements.txt
.gitignore

loso_results.csv
repeated_subject_dependent_results.csv
loso_summary.csv
repeated_subject_dependent_summary.csv
repeated_subject_dependent_subject_summary.csv

EEG_BCI_LOSO.ipynb
EEG_BCI_Repeated_Subject_Dependent.ipynb
```

The two notebook files should be uploaded separately from Google Colab.

## Protocol 1: Cross-Subject Generalization Protocol

This protocol uses Leave-One-Subject-Out evaluation.

In each fold:

```text
One subject is held out as the unseen test subject.
The remaining subjects are used for training and validation.
```

This protocol is more difficult because EEG signals vary strongly across subjects.

## Protocol 2: Repeated Subject-Dependent Protocol

This protocol uses repeated within-subject random splits.

For each subject and each random seed:

```text
70% training
15% validation
15% testing
```

This protocol is easier than LOSO because the model is trained and tested on trials from the same subject. It is useful for subject-dependent comparison and usually produces higher results.

## Main Results

| Protocol | Evaluation Type | Subjects / Runs | Mean Accuracy | Mean Macro-F1 | Best Accuracy |
|---|---|---:|---:|---:|---:|
| CSG-LOSO | Cross-subject | 3 LOSO folds | 0.3519 | 0.3106 | 0.4444 |
| RSDP | Repeated within-subject | 5 subjects x 3 seeds | 0.5762 | 0.5469 | 0.9286 |

## Key Finding

The repeated subject-dependent protocol performs substantially better than the LOSO protocol. This confirms that EEG-Conformer-style models can achieve strong subject-dependent performance, but cross-subject generalization remains much harder.

## Interpretation

The lower LOSO result should not be interpreted as a failed reproduction. It reflects a stricter unseen-subject evaluation setting. The repeated subject-dependent protocol shows that the same model pipeline can reach much higher accuracy when the evaluation setting is closer to the original subject-dependent style.

## Requirements

Install dependencies with:

```bash
pip install -r requirements.txt
```

For Google Colab, enable GPU before running the notebooks.
