# INNOVATION.md 中文版  
# Conformer-EEG-BCI 解码流水线：创新点中文版

## 1. 创新总结

本项目的创新点在于：在同一个 EEG-BCI 解码任务中，基于 Conformer-style backbone 对比了两个不同评估协议，即 CSG-LOSO 跨受试者泛化协议和 RSDP 重复受试者依赖协议。

这一设计使项目不仅能够展示模型在 subject-dependent 设置下的较高准确率，也能够反映模型在 unseen-subject 跨受试者泛化任务中的实际困难。

## 2. 协议对比

| 方面 | CSG-LOSO | RSDP |
|---|---|---|
| 数据协议 | 使用纯运动想象 runs 4、6、8、10、12、14 | 同样使用纯运动想象 runs |
| 评估目标 | 测试模型是否能泛化到未见受试者 | 测试同一受试者内部的 subject-dependent 表现 |
| 数据划分 | 留出一个受试者作为测试集 | 每个受试者内部划分训练/验证/测试 |
| 难度 | 更难 | 相对更容易 |
| 重复性 | Pilot LOSO folds | 每个受试者使用 3 个随机种子重复划分 |
| 标准化 | 只用训练数据拟合标准化参数，避免数据泄漏 | 同样只用训练集拟合标准化参数 |
| 数据增强 | 控制在训练数据内 | 训练集使用 segmentation-reconstruction 增强 |
| 模型结构 | 轻量化 Conformer-style 模型 | 更强调 subject-dependent 性能 |
| 评价指标 | Accuracy、Balanced Accuracy、Macro-F1、Loss、混淆矩阵 | 同样使用多指标评价 |
| 主要结论 | 跨受试者泛化困难 | Subject-dependent 设置下表现更高 |

## 3. 主要技术贡献

### 3.1 纯运动想象任务协议优化

项目只使用 EEGMMIDB 数据集中与纯运动想象相关的 runs，即 4、6、8、10、12、14。这样可以避免将真实执行运动和运动想象混在一起，使任务定义更加清晰。

### 3.2 避免数据泄漏的预处理流程

项目中的标准化过程只使用训练数据拟合参数，验证集和测试集不参与标准化统计量计算。这可以避免数据泄漏，使评估结果更加可靠。

### 3.3 只在训练集使用数据增强

项目使用 segmentation-reconstruction augmentation 来增加训练样本数量，但该增强仅应用于训练集，不会污染验证集和测试集。

### 3.4 双层评估设计

项目同时比较两类问题：

第一，模型是否能够泛化到未见过的受试者，即 CSG-LOSO；

第二，模型在同一受试者内部的解码能力如何，即 RSDP。

这种设计比单一 subject-dependent 实验更完整，能够更清楚地区分模型的被试内表现和跨被试泛化能力。

### 3.5 多指标评价

项目不只报告 Accuracy，还报告 Balanced Accuracy、Macro-F1、Loss 和混淆矩阵。这样可以更全面地评价模型表现，尤其适合不同类别表现可能不均衡的 EEG 分类任务。

## 4. 主要结论

项目结果显示，subject-dependent 设置下模型能够达到较高表现，最佳单次准确率为 92.86%。然而，在严格 LOSO 跨受试者泛化设置下，平均准确率下降到 35.19%，说明跨受试者 EEG 泛化仍然具有挑战。

这一结果有助于解释为什么一些 EEG-Conformer 类模型在 subject-dependent 设置下可以获得较高准确率，但在真实应用中泛化到新受试者时仍然困难。

## 5. 简历/面试推荐表述

我将 EEG-Conformer 复现扩展为结构化 EEG-BCI 解码流水线，针对纯运动想象任务优化数据协议，实现了只在训练集内进行的 segmentation-reconstruction 数据增强和避免数据泄漏的标准化流程，并比较了严格 LOSO 跨受试者协议与重复受试者依赖协议。结果显示，subject-dependent 设置下模型表现较高，而 unseen-subject 泛化仍然具有明显挑战。
