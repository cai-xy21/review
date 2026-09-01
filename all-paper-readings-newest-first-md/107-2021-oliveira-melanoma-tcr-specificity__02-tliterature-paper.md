# 058. Phenotype, specificity and avidity of antitumour CD8 T cells in melanoma

## 基本信息
- 年份：2021
- 期刊：Nature
- DOI：https://doi.org/10.1038/s41586-021-03704-y
- PMID：34290406
- 主题：melanoma CD8 TIL；antigen specificity；TCR avidity；phenotype/function linkage

## 为什么重要
这篇和 `053` 一起，是 specificity-aware tumour T-cell modeling 的核心文献。它把 melanoma CD8 TIL 的 single-cell phenotype、exact TCR clonotype、实验验证的 antigen specificity、TCR avidity、CITE-seq surface phenotype 和 serial blood TCR dynamics 连接起来。它直接回答一个算法痛点：仅凭 exhausted-like transcriptome 能否代表 tumour-reactivity？答案是有强相关但不充分，必须把 TCR function label、antigen context 和 clone dynamics 纳入模型。

## 数据与研究设计
- 物种/组织：Homo sapiens；melanoma CD8 TIL 与 serial peripheral blood
- 样本结构：文章主图围绕多个 melanoma patients 的 tumour/blood TCR tracking；其中 TCR reconstruction 和 specificity screening 重点展示四名患者的深入分析
- 模态：scRNA-seq、scTCR-seq、CITE-seq、TCR specificity/deorphanization、TCR reconstruction、avidity assays、HLA/peptide context、blood TCR dynamics
- 关键功能标签：tumour-specific TCRs、viral/bystander TCRs、MAA-specific、NeoAg-specific、EBV/viral-specific 等
- 研究目标：在单细胞分辨率上连接 CD8 T-cell phenotype、TCR specificity、TCR avidity、tumour antigen abundance 和 peripheral persistence

## 算法贡献
- 把 phenotype、exact TCR clonotype、specificity 和 avidity 连到 single-cell resolution。
- 证明 exhausted-like state 与 tumour-reactivity 强相关，但 bystander/viral cells 会占据不同 memory-like states。
- 把 tumour antigen quality/quantity 也纳入解释：TCR avidity 与 target abundance、peptide-HLA binding affinity/stability 相关。
- 给 specificity-aware representation learning 提供功能标签和跨 compartment dynamics 问题。
- 提供“部分 clonotype 有功能真值、更多 clonotype 未实验验证”的典型 semi-supervised learning 场景。

## 文章中的算法/分析流程
### 1. Single-cell phenotype and clonotype map
- 输入是 melanoma CD8 TIL 的 scRNA-seq、scTCR-seq 和部分 CITE-seq 数据。
- 通过 Seurat/Harmony/SingleR/Scanpy 等工具进行单细胞归一化、整合、注释和可视化。
- TCR clonotype 被映射回单细胞 phenotype clusters，用于比较 exhausted、non-exhausted memory-like 和其他 CD8 states。

### 2. TCR reconstruction and specificity screening
- 从 TIL clonotypes 中选择候选 TCR，进行 TCR cloning/reconstruction。
- 将重构 TCR 转入健康供者 T cells，测试其对 autologous melanoma cells、control cells、peptide-pulsed targets、MAA/NeoAg/viral peptide 的反应。
- 输出是 clonotype-level antigen specificity label：tumour-specific、viral-specific、non-reactive/unknown 等。

### 3. Avidity and antigen context
- 对 tumour-specific TCRs 测量 avidity，并与 melanoma cell 中 cognate target abundance、peptide-HLA binding affinity/stability 关联。
- 算法意义：label 不只是 binary specificity，还可以是 specificity category + functional strength/avidity。

### 4. Tumour-blood clone dynamics
- 在 serial peripheral blood 中追踪 TIL-derived TCR clonotypes。
- 分析 intratumoural exhaustion level 与 blood persistence、checkpoint response context 的关系。
- 这为 longitudinal clone monitoring 提供任务定义：用 tumour phenotype 和 blood repertoire 共同预测 persistence/response。

## 数据可用性
- 文章 DOI：https://doi.org/10.1038/s41586-021-03704-y
- dbGaP：`phs001451.v3.p1`
- dbGaP study ID：26121
- 数据性质：human melanoma CD8 TIL and peripheral blood；scRNA-seq、scTCR-seq、CITE-seq；TCR specificity/deorphanization；avidity and antigen-context assays
- 数据访问边界：scRNA-seq、scTCR-seq 和 CITE-seq 经 dbGaP controlled access；其他数据可向通讯作者合理请求
- 论文网页：<https://www.nature.com/articles/s41586-021-03704-y>
- PubMed：<https://pubmed.ncbi.nlm.nih.gov/34290406/>
- code：<https://github.com/kstromhaug/oliveira-stromhaug-melanoma-tcrs-phenotypes>

## 代码可用性
- 代码内容：10x TCR processing、per-patient clonotype grouping、combined TIL phenotype analyses、reference comparisons、manuscript statistics。
- 文章 Code availability 列出的公共工具包括：
  - Broad Institute Picard Pipeline
  - GATK4/Mutect2
  - NetMHCpan 4.0
  - ContEst、ABSOLUTE
  - STAR、RSEM
  - Seurat v3.2.0、Harmony v1.0、SingleR、Scanpy
- 代码输入：
  - 10x scRNA/scTCR/CITE-seq objects
  - per-patient TCR clonotype tables
  - reconstructed/screened TCR specificity labels
  - antigen annotations：MAA、NeoAg、viral peptide、pHLA binding/target abundance
  - blood TCR tracking tables
- 代码输出：
  - clonotype group assignments
  - phenotype cluster/state comparisons
  - tumour-specific vs viral/bystander T-cell state analyses
  - avidity and antigen-context associations
  - tumour-blood persistence summaries
- 模型结构与意义：
  - 公开代码是 study-specific analysis scripts，不是通用 antigen-prediction package。
  - 可抽象为 `single-cell phenotype + TCR clonotype + functional specificity/avidity label + blood dynamics -> phenotype-function model`。

## 对新算法开发的启发
1. sequence + phenotype + antigen context 的 specificity/avidity prediction
2. partially labelled clonotype learning
3. tumour-blood clone persistence model
4. bystander-vs-tumour-reactive classifier with calibrated uncertainty
5. 使用 pHLA binding、tumour expression、TCR sequence embedding 和 single-cell phenotype 的 multimodal model

## 对新算法贡献程度
- 直接算法创新：**中等偏低**。主要是高质量整合分析和代码脚本，而非新通用模型。
- 数据/功能标签贡献：**极高**。specificity、avidity、phenotype 和 blood dynamics 同时存在，极适合新算法开发。
- 公开代码贡献：**高**。有作者 GitHub 分析代码；数据本身为 dbGaP controlled access。
- 新算法空间：**极高**。尤其适合 semi-supervised specificity prediction、avidity regression 和 cross-compartment clone persistence modeling。

## 可作为我们 method 报告里的位置
建议与 `053` 并列放在“specificity-aware and function-aware T-cell modeling”。`053` 更偏 NSCLC anti-PD-1 MANA-specific TIL atlas，`058` 更偏 melanoma TCR specificity/avidity/function linkage；两者共同说明新算法不应只预测 T-cell state，还应预测 antigen function。

## 一句话结论
`058` 把抗原功能真值接到 single-cell phenotype，是 tumour-reactive T-cell algorithm 从 state inference 走向 function inference 的关键条目。
