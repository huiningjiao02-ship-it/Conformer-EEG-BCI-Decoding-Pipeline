# Results

This file summarizes the current experiment outputs for the Conformer-based EEG/BCI decoding pipeline.

## Current Experiment Setting

- Dataset: PhysioNet EEG Motor Movement/Imagery Dataset (EEGMMIDB)
- Task: Four-class motor imagery decoding
- Classes:
  - left fist imagery
  - right fist imagery
  - both fists imagery
  - both feet imagery
- Protocol: Pure motor imagery runs
- Runs: 4, 6, 8, 10, 12, 14
- Subjects: 30-subject setup
- Evaluation: Leave-One-Subject-Out (LOSO) cross-subject evaluation
- Model: EEG-Conformer / convolutional Transformer architecture

## Current Status

The repository currently supports pilot LOSO evaluation.

Recommended quick test:

```python
RUN_FOLDS = 3
```

Larger pilot run:

```python
RUN_FOLDS = 10
```

Full 30-subject LOSO:

```python
RUN_FOLDS = None
```

## Metrics

The notebook reports the following metrics:

- Accuracy
- Balanced accuracy
- Macro-F1
- Confusion matrix
- Training loss
- Validation loss
- Runtime per LOSO fold

## Result Interpretation

This project does not only focus on achieving high accuracy.

The main goals are:

1. To build a cleaner EEG/BCI decoding workflow.
2. To avoid data leakage.
3. To test cross-subject generalization.
4. To analyze overfitting in small-sample EEG decoding.
5. To evaluate whether Transformer-based EEG decoding remains stable under stricter protocols.

## How to Fill in Results Later

After running the notebook, update this section with your actual results.

### Pilot LOSO Result

| Setting | Accuracy | Balanced Accuracy | Macro-F1 |
|---|---:|---:|---:|
| RUN_FOLDS = 3 | TBD | TBD | TBD |

### Full LOSO Result

| Setting | Accuracy | Balanced Accuracy | Macro-F1 |
|---|---:|---:|---:|
| RUN_FOLDS = None | TBD | TBD | TBD |

## Notes

If the LOSO accuracy is low or unstable, this is still informative.

Cross-subject EEG decoding is difficult because EEG patterns vary strongly across subjects. Low LOSO performance can support the project insight that Transformer-based EEG decoding is prone to overfitting and weak cross-subject generalization under small-sample settings.
