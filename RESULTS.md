# Results

This file summarizes the two main experiment protocols.

## Protocol A: Cross-Subject Generalization Protocol

This is the stricter LOSO protocol. It evaluates whether the model can generalize to unseen subjects.

### LOSO Pilot Results

| Test Subject | Accuracy | Balanced Accuracy | Macro-F1 | Test Loss | Time sec |
|---:|---:|---:|---:|---:|---:|
| 1 | 0.3444 | 0.3486 | 0.3005 | 1.4205 | 205.81 |
| 2 | 0.4444 | 0.4347 | 0.4204 | 1.2383 | 259.57 |
| 3 | 0.2667 | 0.2798 | 0.2108 | 2.0821 | 430.53 |

### LOSO Summary

| Metric | Mean | Std |
|---|---:|---:|
| Accuracy | 0.3519 | 0.0891 |
| Balanced Accuracy | 0.3544 | 0.0776 |
| Macro-F1 | 0.3106 | 0.1052 |
| Test Loss | 1.5803 | 0.4440 |
| Time / run | 298.64 sec | 117.34 sec |

## Protocol B: Repeated Subject-Dependent Protocol

This is the repeated within-subject protocol. It is closer to the original subject-dependent evaluation style and is expected to produce higher performance.

Each subject was evaluated with 3 random split seeds:

```text
42, 123, 2026
```

### Overall Repeated Subject-Dependent Summary

| Metric | Mean | Std |
|---|---:|---:|
| Accuracy | 0.5762 | 0.1992 |
| Balanced Accuracy | 0.5736 | 0.2003 |
| Macro-F1 | 0.5469 | 0.2007 |
| Test Loss | 1.3170 | 0.7785 |
| Time / run | 65.99 sec | 21.05 sec |

### Per-subject Mean Results

| Subject | Mean Accuracy | Std Accuracy | Mean Balanced Accuracy | Mean Macro-F1 | Mean Test Loss |
|---:|---:|---:|---:|---:|---:|
| 1 | 0.7143 | 0.1890 | 0.7083 | 0.6548 | 1.0410 |
| 2 | 0.7143 | 0.1890 | 0.7014 | 0.6911 | 0.6572 |
| 3 | 0.5714 | 0.1890 | 0.5903 | 0.5487 | 0.9397 |
| 4 | 0.5000 | 0.1890 | 0.4792 | 0.4860 | 1.6134 |
| 5 | 0.3810 | 0.1091 | 0.3889 | 0.3539 | 2.3338 |

### Best Single Run

| Subject | Split Seed | Accuracy | Balanced Accuracy | Macro-F1 | Test Loss |
|---:|---:|---:|---:|---:|---:|
| 2 | 2026 | 0.9286 | 0.9375 | 0.9365 | 0.2357 |

## Protocol Comparison

| Protocol | Purpose | Mean Accuracy | Mean Macro-F1 | Best Accuracy |
|---|---|---:|---:|---:|
| CSG-LOSO | Unseen-subject generalization | 0.3519 | 0.3106 | 0.4444 |
| RSDP | Subject-dependent performance | 0.5762 | 0.5469 | 0.9286 |

## Interpretation

The repeated subject-dependent protocol performs much better than LOSO. This is expected because the model is trained and tested on data from the same subject, so the EEG distribution is more consistent.

The LOSO result is lower because the test subject is completely unseen during training. This makes the task much harder and exposes the difficulty of cross-subject EEG generalization.

The best repeated subject-dependent run reached 92.86% accuracy, showing that the model pipeline can achieve high performance under favorable subject-dependent conditions. However, the average performance over all five subjects is lower, which indicates strong subject variability in EEG data.

## Recommended Reporting Sentence

The project achieved a mean accuracy of 57.62% under repeated subject-dependent evaluation, with the best single run reaching 92.86%. Under the stricter LOSO cross-subject protocol, the mean accuracy was 35.19%, highlighting the difficulty of unseen-subject EEG generalization.
