# RESULTS.md

## Conformer-EEG-BCI Decoding Results Summary

This project compares two evaluation protocols for four-class EEG-based motor imagery decoding:  
1. **CSG-LOSO Cross-Subject Generalization Protocol**: evaluates whether the model can generalize to unseen subjects.  
2. **RSDP Repeated Subject-Dependent Protocol**: evaluates decoding performance within the same subject.  

The classification task includes four motor imagery classes: left fist, right fist, both fists, and both feet.

## 1. Main Results

| Protocol | Evaluation Goal | Mean Accuracy | Mean Macro-F1 | Best Accuracy |
|---|---|---:|---:|---:|
| CSG-LOSO | Cross-subject generalization | 35.19% | 31.06% | 44.44% |
| RSDP | Within-subject decoding | 57.62% | 54.69% | 92.86% |

## 2. LOSO Cross-Subject Results

In the LOSO protocol, one subject is held out as the completely unseen test subject, while the remaining subjects are used for training and validation. This is a stricter evaluation protocol designed to test generalization to new subjects.

| Metric | Mean | Standard Deviation |
|---|---:|---:|
| Accuracy | 35.19% | 8.91% |
| Balanced Accuracy | 35.44% | 7.76% |
| Macro-F1 | 31.06% | 10.52% |
| Test Loss | 1.5803 | 0.4440 |

## 3. Repeated Subject-Dependent Results

In the RSDP protocol, each subject's data are split into training, validation, and test sets within the same subject, and the experiment is repeated using multiple random seeds. This protocol is closer to common subject-dependent EEG-BCI evaluation settings.

| Metric | Mean | Standard Deviation |
|---|---:|---:|
| Accuracy | 57.62% | 19.92% |
| Balanced Accuracy | 57.36% | 20.03% |
| Macro-F1 | 54.69% | 20.07% |
| Test Loss | 1.3170 | 0.7785 |

The best single run was achieved on Subject 2 with random seed 2026, reaching an accuracy of **92.86%**, balanced accuracy of **93.75%**, and Macro-F1 of **93.65%**.

## 4. Interpretation

The RSDP results are substantially higher than the LOSO results, suggesting that the model can learn more stable EEG representations when training and testing are performed within the same subject. In contrast, LOSO requires the model to generalize to completely unseen subjects, which is more challenging due to large inter-subject variability in EEG signals.

The main conclusion is that Conformer-style EEG decoding models show promising performance in subject-dependent settings, while cross-subject EEG generalization remains a challenging research problem.

## 5. Resume / Interview Description

Built an EEG-BCI four-class motor imagery decoding pipeline using MNE-Python, NumPy, SciPy, and PyTorch. Implemented a Conformer-style CNN-Transformer model and compared a LOSO cross-subject generalization protocol with a repeated subject-dependent protocol. The repeated subject-dependent protocol achieved a mean accuracy of 57.62% and a best single-run accuracy of 92.86%, while the LOSO protocol achieved a mean accuracy of 35.19%, reflecting the difficulty of cross-subject EEG generalization.
