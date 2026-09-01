# Algorithm Report 077

## Paper
COVID-19 and the human innate immune system

## 算法视角定位
这篇文章是 Cell 综述，不是原始单细胞数据论文，也不提出新算法。它适合放在 method report 的 **biological framing / innate-immunity prior** 位置，用来总结 COVID-19 中 interferon、myeloid inflammation、neutrophil、monocyte/macrophage、complement/coagulation 和 tissue damage 等先验知识。对 T 细胞-人群免疫力算法而言，它的价值是帮助定义哪些 innate programs 应作为 covariates or interacting modules 纳入 T-cell modeling。

## 数据模态与样本设计
- 原始数据：无。综述整合多篇 COVID-19 human immunology studies。
- 物种/器官：以 human COVID-19 studies 为主，覆盖 blood, lung, airway and systemic innate immune response。
- 关联单细胞主题：scRNA-seq/CITE-seq/CyTOF 等研究在综述中作为证据来源，而非本文生成数据。

## 关键算法问题
1. T-cell population immunity 模型中是否需要显式加入 innate immune context。
2. severe COVID-19 的 T-cell dysfunction 是 T-cell-intrinsic 还是受 interferon/myeloid/cytokine environment 驱动。
3. 如何把 innate inflammation、antiviral defense 和 immunopathology 表达为可计算的 donor-level covariates。
4. 如何从综述中提炼可用于模型约束的 biological priors。

## 论文中的方法学贡献
### 1) Innate immune axes as modeling priors
综述强调 COVID-19 的关键 innate axes 包括：
- early/late type I interferon response
- inflammatory monocyte/macrophage programs
- neutrophil activation and NET-related pathology
- complement/coagulation coupling
- cytokine and chemokine circuits
- tissue epithelial-immune interaction

**算法意义**：这些 axes 可以转化为 module scores、latent factors、confounders 或 graph nodes，用于解释 T-cell state variation。

### 2) Timing and severity framing
综述区分 antiviral protection 与 delayed/excessive inflammation。对算法来说，time since infection/onset 不能只是 metadata，应作为动态变量建模。

**方法学价值**：同样的 IFN/T-cell state 在不同病程时间点可能含义相反。因此 severity model 需要 time-aware or stage-aware design。

### 3) Multicellular interaction framing
文章把 innate sensing、myeloid recruitment、cytokine storm、adaptive immune dysregulation 放入一个跨细胞系统中。

**算法意义**：支持开发 `innate-adaptive interaction model`，而非只在 T-cell subset 内做 clustering。

## 不是它解决得很好的问题
1. 无原始数据、无代码、无 accession。
2. 不提供可直接复现的 computational pipeline。
3. 由于是综述，结论依赖纳入文献质量和当时数据。
4. 对 TCR/BCR、single-cell multimodal joint modeling 的算法细节不是重点。

## 数据可用性评估
- DOI：https://doi.org/10.1016/j.cell.2021.02.029
- PubMed：PMID 33743212
- 数据：无新数据；引用已有公开/受控数据研究。
- 代码：无新代码。
- 复用性：作为 biological prior 高；作为 algorithm benchmark 低。

## 代码/模型结构
- 输入：不适用。
- 输出：不适用。
- 可转化为算法输入的内容：innate immune gene modules、severity-stage priors、innate-adaptive interaction graph、time-dependent interpretation rules。

## 对新算法贡献程度评估
- 定义任务价值：中高
- 数据资源价值：低
- 直接算法创新：低
- 对后续方法启发：中高

综合评估：**综述型生物学先验文献；不应被写成数据或算法贡献，但适合支持模型设计中的 innate context。**

## 可发展的新算法空间
### A. Innate-adaptive joint immune model
联合建模 myeloid/IFN/neutrophil/cytokine modules 与 T-cell activation/exhaustion/clonal expansion。

### B. Time-aware severity modeling
把 symptom-onset time、sampling time 和 treatment time 纳入 disease-state inference。

### C. Biological-prior constrained factor model
用综述总结的 innate axes 作为先验 factor groups，提升模型解释性。

## 适合纳入 method report 的表述
这篇综述不提供算法或数据，但它明确了 T-cell state 不能脱离 innate immune context 解释。若我们开发 T-cell population immunity 算法，应至少纳入 IFN/myeloid/neutrophil/cytokine 等先验模块作为协变量或互作节点。
