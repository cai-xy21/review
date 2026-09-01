# 074. Multi-omics resolves a sharp disease-state shift between mild and moderate COVID-19

## 基本信息
- 年份：2020
- 期刊：Cell
- DOI：https://doi.org/10.1016/j.cell.2020.10.037
- 主题：COVID-19 multi-omics；severity transition；longitudinal blood cohort；T-cell states

## 为什么重要
这篇文章把 COVID-19 免疫差异从简单 mild/severe 二分类推进到 multi-omics disease-state shift，尤其强调 mild 到 moderate 之间存在明显转折。它的数据包含 clinical labs、plasma proteomics/metabolomics、single-cell RNA/CITE-seq、TCR-seq、single-cell secretome 等，是开发 donor-level multi-omics severity model 的高价值任务场景。

## 数据与研究设计
- 样本对象：139 名 COVID-19 patients，覆盖 mild、moderate、severe；另有 258 healthy controls 用于不同模态匹配
- 采样设计：许多患者在诊断后一周内 serial blood draws，支持 T1/T2 trajectory 分析
- 物种/器官：Homo sapiens；peripheral blood / PBMC / plasma
- 模态：clinical labs/EHR、plasma proteomics、plasma metabolomics、PBMC scRNA-seq/CITE-seq、single-cell TCR-seq、single-cell secretome
- severity 定义：WHO Ordinal Scale (WOS)
- 研究目标：识别 COVID-19 severity progression 中不同模态的 disease-state transition，尤其 mild-to-moderate shift

## 核心亮点
1. **multi-omics longitudinal cohort**：同一 disease axis 上整合临床、血浆分子和单细胞免疫状态。
2. **非线性 severity transition**：mild-to-moderate 的变化比 moderate-to-severe 更明显，提示 phase transition/change point。
3. **T-cell 与 TCR 层信息**：涉及 proliferative/exhausted CD4 T cells、cytotoxic CD4 T cells 和 TCR sharing patterns。
4. **临床可转化任务**：moderate transition 可能是治疗窗口，适合 early-warning severity classifier。

## 核心贡献
- 发现 plasma proteome/metabolome 在 healthy-to-mild、mild-to-moderate、moderate-to-severe 阶段呈不同转折模式。
- 将 lipid、amino acid、xenobiotic metabolism 等代谢变化与 inflammatory protein/cytokine signals 关联。
- 在 PBMC single-cell 数据中解析 disease-severity associated immune cell states，包括 T-cell activation/exhaustion/cytotoxic programs。
- 通过 cross-omic correlation/network 和 surprisal/module analysis 组织多模态 disease modules。

## 与 T 细胞-人群免疫力的关系
该文提供了一个 T-cell state 如何嵌入全身代谢、炎症和临床严重程度的例子。T 细胞不是孤立变化：其增殖、耗竭、细胞毒性和克隆共享状态，需要与 plasma proteomics/metabolomics 和病程阶段联合建模。

## 文章中的算法/分析流程
### 1. WOS-aligned severity modeling
作者用 WOS 表示每次 blood draw 的 severity，并支持两种视角：按当前样本 severity 分组，或按患者 T1/T2 轨迹分析。算法上可抽象为 ordinal disease-state modeling。

### 2. Stepwise differential multi-omics
分别比较 healthy-to-mild、mild-to-moderate、moderate-to-severe 的蛋白、代谢物、细胞状态和 clinical variables，识别转折阶段。

### 3. Cross-omic network/correlation
使用 Spearman correlation、covariate adjustment、GEE、probabilistic PCA、surprisal analysis 和 module scoring 关联 plasma molecules、clinical labs 与 single-cell modules。

### 4. T-cell/TCR interpretation
在 single-cell 层解释 severity-associated T-cell states 和 TCR sharing，但尚未形成端到端 TCR-state-severity joint model。

## 对算法工作的启发
1. **Multi-omic change-point detection**：把 disease shift 明确建模为非线性 phase transition。
2. **Longitudinal donor hidden-state model**：区分病程、治疗、基础免疫差异和采样时间。
3. **Missing-modality robust learning**：真实临床多组学常有模态缺失和不均匀采样。
4. **TCR-state-severity integration**：将 clone expansion/sharing 与 cell phenotype、plasma context 和 outcome 联合。

## 数据可用性
- scRNA-seq ArrayExpress：E-MTAB-9357
- Plasma proteomics/metabolomics supplemental：Mendeley Data <https://doi.org/10.17632/tzydswhhb5.5>
- 数据性质：human COVID-19 blood/PBMC/plasma；139 patients with serial draws；healthy controls across modalities
- 代码：未定位独立完整作者 GitHub；数据与 supplement 公开，但完整 pipeline 需按 Methods 重建
- 代码输入/输出（按论文流程抽象）：
  - 输入：WOS labels、clinical labs、protein/metabolite abundance、single-cell RNA/ADT/TCR/secretome profiles
  - 输出：severity-associated omics features、transition modules、cross-omic networks、single-cell immune-state summaries
- 模型结构与意义：multi-omics statistical integration workflow；对新算法最重要的是 disease transition benchmark

## 可信度评估
- 期刊层面：Cell，高可信度
- 可复现性：ArrayExpress 和 Mendeley Data 明确；完整整合分析较依赖 metadata 对齐
- 局限：多模态缺失、治疗/时间/基础疾病 confounding、TCR 与其他模态未端到端联合
- 综合判断：**数据和任务价值很高，直接算法创新中等，对 disease-state modeling 启发很强**

## 一句话结论
这篇文章应作为 multi-omics severity transition 的核心文献：它提示人群免疫算法要能在 donor-level 多模态数据中识别非线性病程转折，而不只是做 cell-type annotation。
