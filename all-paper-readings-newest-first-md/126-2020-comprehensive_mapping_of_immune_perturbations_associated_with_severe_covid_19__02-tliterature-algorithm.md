# Algorithm Report 075

## Paper
Comprehensive mapping of immune perturbations associated with severe COVID-19

## 算法视角定位
这篇文章是 COVID-19 high-dimensional immune profiling 的早期研究，重点是用流式/高维免疫表型、serology、cytokines 和临床指标刻画 severe COVID-19 的免疫扰动。它不是单细胞组学算法论文，也不是 scRNA-seq atlas；在本综述中更适合作为 **人群免疫 phenotype and severity module** 参考。

## 数据模态与样本设计
- 模态：高维 flow cytometry/immunophenotyping、plasma cytokines/chemokines、SARS-CoV-2 antibody responses、clinical laboratory metadata。
- 物种/器官：`Homo sapiens` peripheral blood/PBMC/plasma。
- 队列：35 名 active hospitalized COVID-19 patients，其中 7 名 moderate、28 名 severe；7 名 recovered donors；12 名 healthy donors。
- 细胞重点：T cells, B cells, NK cells, monocytes, granulocyte-related features, plasmablasts and activation/exhaustion markers。

## 关键算法问题
1. 如何从高维免疫表型数据中识别 severe COVID-19 相关的细胞组成和 marker 状态。
2. 如何把 T-cell activation/exhaustion、B-cell/plasmablast response、myeloid activation 和 cytokine storm 放在 donor-level severity 背景下解释。
3. 如何从 flow-level features 构造可泛化的 severity-associated immune modules。
4. 如何与 scRNA/CITE-seq COVID 数据互相校验。

## 论文中的算法贡献
### 1) High-dimensional immune phenotype mapping
文章系统比较 COVID-19 patients 与 controls 的 immune-cell frequencies and phenotypes，刻画 lymphopenia、T-cell activation、B-cell/plasmablast expansion、monocyte and myeloid perturbations 等 severe-associated features。

**算法意义**：它提供了 single-cell transcriptomics 外的 protein/phenotype validation layer。对我们方法论文来说，可作为 transcriptome-derived states 是否有 flow-level counterpart 的参考。

### 2) Severity-associated immune perturbation modules
文章把多个免疫分支的改变合成 severe COVID-19 的 immune perturbation picture，而不是只关注某一个 marker。

**方法学价值**：这对应 donor-level feature engineering：从 cell population frequencies、activation markers、cytokines 和 serology 构建 severity-associated vector。

### 3) Cross-study reusable phenotype anchor
后续机器学习研究常把此类 flow cytometry dataset 与其他 COVID flow datasets 合并或并列，用 PhenoGraph/neural network/LRP 等方法重分析。说明该文数据适合作为 severity phenotype benchmark。

## 不是它解决得很好的问题
1. 不是 scRNA/CITE-seq 数据，无法解析 transcriptomic state 和 receptor clonotype。
2. 没有提出新的 clustering/classification algorithm。
3. flow panels 的 marker choice 限制了状态发现空间。
4. cohort/treatment/timing confounding 仍需要层级统计模型处理。

## 数据可用性评估
- DOI：https://doi.org/10.1126/sciimmunol.abd7114
- PubMed：PMID 32669287
- IgH sequencing BioProject/SRA：`PRJNA630455`，原文说明以 AIRR-compliant manner 提供。
- Flow cytometry files：原文说明 compensated flow cytometry files 可通过 <https://hpap.pmacs.upenn.edu> 获取，并联系 MRB 获取下载说明。
- Data Availability 原文边界：主要数据在 paper 或 Supplementary Materials；IgH data 上传 SRA；flow files 通过 HPAP/PMACS 平台。
- 可确认数据性质：human peripheral blood immune phenotyping from 35 active hospitalized COVID-19 patients, 7 recovered donors and 12 healthy donors, severity-stratified。
- 代码：未定位到作者独立代码仓库；使用高维免疫表型统计分析、gating、t-SNE visualization 与相关分析。
- 复用性：中等到中高。结论、marker modules、IgH SRA 和 flow portal 可复用；但 flow 文件下载不是简单 GEO/SRA 矩阵式入口，完整复现仍需要平台访问和 gating 细节。

## 代码/模型结构
- 输入：flow cytometry FCS/derived frequency table、marker intensity features、CBC/clinical labs、cytokine/serology values、IgH sequencing summaries、clinical severity metadata。
- 核心流程：`manual/algorithmic gating -> t-SNE visualization -> frequency/marker comparison -> severity association -> correlation with cytokine/serology/clinical labs -> NLR/NTR biomarker analysis -> immune perturbation summary`
- 输出：severity-associated immune-cell frequency changes、activation/exhaustion phenotype features、plasmablast/humoral response summaries、NLR/NTR severity associations、candidate immune perturbation modules。
- 模型意义：提供 protein/phenotype anchor，可用于验证 scRNA-derived immune states。

## 对新算法贡献程度评估
- 定义任务价值：中高
- 数据资源价值：中
- 直接算法创新：低
- 对后续方法启发：中高

综合评估：**重要人群免疫 phenotype paper；不是单细胞组学算法核心，但可作为 severity-associated immune modules 的外部验证参考。**

## 可发展的新算法空间
### A. Cross-platform phenotype alignment
把 flow/CyTOF marker space 与 scRNA/CITE-seq cell-state space 对齐，形成可解释的 severity state translation。

### B. Donor-level immune perturbation score
从 flow frequencies、marker intensity、cytokines 和 clinical labs 训练 interpretable severity score。

### C. Panel-aware state inference
针对不同 flow panels 的 marker 缺失，开发可迁移 immune phenotype model。

## 适合纳入 method report 的表述
这篇文章提醒我们，T-cell and population-immunity algorithms 不能只在 RNA space 内闭环；severe disease states 最终需要能映射到 flow/CyTOF 等临床可部署表型空间。
