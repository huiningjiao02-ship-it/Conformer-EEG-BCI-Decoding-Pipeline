# Conformer-EEG-BCI-Decoding-Pipeline  
# Conformer-EEG-BCI 解码流水线

## 1. Project Overview  
## 1. 项目概述

This repository contains an EEG/BCI decoding pipeline based on a Conformer-style convolutional Transformer architecture. The task is four-class motor imagery classification from EEG signals.

本仓库包含一个基于 Conformer-style convolutional Transformer 架构的 EEG/BCI 解码流水线，用于 EEG 信号的四分类运动想象任务。

The project is not only a simple reproduction of an existing EEG-Conformer model. It reorganizes the experiment into two complementary evaluation protocols: a strict cross-subject generalization protocol and a repeated subject-dependent protocol.

本项目并不是简单复现一个已有 EEG-Conformer 模型，而是将实验整理为两个互补的评估协议：严格的跨受试者泛化协议，以及重复受试者依赖协议。

## 2. Task Description  
## 2. 任务说明

The model predicts four imagined motor actions from EEG signals:

模型需要根据 EEG 信号预测四类运动想象动作：

| Label | English Class | 中文类别 |
|---|---|---|
| 0 | left_fist | 想象左拳运动 |
| 1 | right_fist | 想象右拳运动 |
| 2 | both_fists | 想象双拳运动 |
| 3 | both_feet | 想象双脚运动 |

## 3. Dataset Protocol  
## 3. 数据协议

This project uses the EEGMMIDB dataset and focuses only on pure motor imagery runs: 4, 6, 8, 10, 12, and 14. This avoids mixing executed movement trials with imagined movement trials.

本项目使用 EEGMMIDB 数据集，并且只选择纯运动想象相关的 runs，即 4、6、8、10、12 和 14。这样可以避免将真实执行运动任务和运动想象任务混在一起，使任务定义更加清晰。

## 4. Evaluation Protocols  
## 4. 评估协议

### 4.1 CSG-LOSO: Cross-Subject Generalization Protocol  
### 4.1 CSG-LOSO：跨受试者泛化协议

CSG-LOSO is a strict Leave-One-Subject-Out protocol. In each fold, one subject is held out as the unseen test subject, while the remaining subjects are used for training and validation.

CSG-LOSO 是严格的 Leave-One-Subject-Out 协议。每一折实验中，都会留下一个受试者作为完全未见过的测试集，其余受试者用于训练和验证。

This protocol is more challenging because EEG signals vary substantially across subjects. It is used to evaluate whether the model can generalize to unseen subjects.

该协议难度更高，因为不同受试者之间的 EEG 信号差异较大。它主要用于评估模型是否能够泛化到未见过的受试者。

### 4.2 RSDP: Repeated Subject-Dependent Protocol  
### 4.2 RSDP：重复受试者依赖协议

RSDP is a repeated subject-dependent protocol. For each subject, the data are split into training, validation, and test sets within the same subject, using multiple random seeds.

RSDP 是重复受试者依赖协议。对于每个受试者，数据会在同一受试者内部划分为训练集、验证集和测试集，并使用多个随机种子重复实验。

This protocol is closer to the subject-dependent evaluation setting commonly used in EEG-BCI studies and usually produces higher performance than strict cross-subject testing.

该协议更接近 EEG-BCI 研究中常见的 subject-dependent 评估方式，通常会比严格跨受试者测试取得更高的性能。

## 5. Main Results  
## 5. 主要结果

| Protocol | Mean Accuracy | Mean Macro-F1 | Best Accuracy |
|---|---:|---:|---:|
| CSG-LOSO | 35.19% | 31.06% | 44.44% |
| RSDP | 57.62% | 54.69% | 92.86% |

| 协议 | 平均准确率 | 平均 Macro-F1 | 最佳准确率 |
|---|---:|---:|---:|
| CSG-LOSO 跨受试者泛化协议 | 35.19% | 31.06% | 44.44% |
| RSDP 重复受试者依赖协议 | 57.62% | 54.69% | 92.86% |

## 6. Interpretation  
## 6. 结果解释

The repeated subject-dependent protocol achieved substantially higher performance than the strict LOSO protocol. This indicates that Conformer-style EEG decoding models can achieve stronger performance when training and testing are conducted within the same subject.

重复受试者依赖协议的结果明显高于严格 LOSO 协议。这说明在同一受试者内部进行训练和测试时，Conformer-style EEG 解码模型能够获得更强表现。

However, the lower LOSO result does not indicate a failed reproduction. Instead, it reflects the difficulty of cross-subject EEG generalization. EEG signals vary greatly across individuals, so generalizing to unseen subjects remains a challenging research problem.

然而，LOSO 结果较低并不代表复现失败，而是反映了跨受试者 EEG 泛化本身的困难。由于不同个体的 EEG 信号差异较大，泛化到未见受试者仍然是一个具有挑战性的研究问题。

## 7. Repository Files  
## 7. 仓库文件说明

| File | Description | 中文说明 |
|---|---|---|
| `README.md` | Project overview, task description, protocols, and main results | 项目总介绍、任务说明、评估协议和主要结果 |
| `RESULTS.md` | Detailed experimental results for LOSO and RSDP | LOSO 和 RSDP 的详细实验结果 |
| `INNOVATION.md` | Technical contributions and innovation points | 技术贡献和创新点 |
| `requirements.txt` | Python dependencies | Python 依赖包 |
| `loso_results.csv` | Per-fold LOSO results | LOSO 每一折的结果 |
| `loso_summary.csv` | Summary of LOSO results | LOSO 结果汇总 |
| `repeated_subject_dependent_results.csv` | Per-run repeated subject-dependent results | 重复受试者依赖协议的每次运行结果 |
| `repeated_subject_dependent_summary.csv` | Overall RSDP summary | RSDP 整体结果汇总 |
| `repeated_subject_dependent_subject_summary.csv` | Per-subject RSDP summary | RSDP 按受试者汇总结果 |
| `Conformer_EEG_BCI_Decoding_Pipeline_LOSO_Version.ipynb` | LOSO notebook | LOSO 跨受试者版本 notebook |
| `Conformer_EEG_BCI_Decoding_Pipeline_Repeated_Subject_Dependent_Protocol_Version.ipynb` | repeated subject-dependent notebook | 重复受试者依赖版本 notebook |

## 8. Resume-Friendly Description  
## 8. 简历可用项目描述

Built an EEG-BCI four-class motor imagery decoding pipeline using MNE-Python, NumPy, SciPy, and PyTorch. Implemented a Conformer-style CNN-Transformer model and compared a strict LOSO cross-subject generalization protocol with a repeated subject-dependent protocol. The repeated subject-dependent protocol achieved a mean accuracy of 57.62% and a best single-run accuracy of 92.86%, while the LOSO protocol achieved a mean accuracy of 35.19%, reflecting the difficulty of cross-subject EEG generalization.

基于 MNE-Python、NumPy、SciPy 和 PyTorch 构建 EEG-BCI 四分类运动想象解码流水线，使用 Conformer-style CNN-Transformer 架构进行模型训练，并比较严格 LOSO 跨受试者泛化协议与重复受试者依赖协议。重复受试者依赖协议平均准确率为 57.62%，最佳单次准确率达到 92.86%；LOSO 协议平均准确率为 35.19%，反映跨受试者 EEG 泛化难度较高。
