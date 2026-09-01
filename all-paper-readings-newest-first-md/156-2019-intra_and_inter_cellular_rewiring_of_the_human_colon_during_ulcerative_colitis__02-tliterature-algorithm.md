# Algorithm Report 041

## Paper
Intra- and Inter-cellular Rewiring of the Human Colon during Ulcerative Colitis

## 算法视角定位
这篇文章不是“发明一个通用单细胞算法”的方法论文，而是很强的**疾病组织单细胞系统建模范例**。它把 colon mucosa 中的上皮、基质和免疫细胞放到同一 atlas 中，围绕 UC 的三个层次组织算法问题：
- cell subset taxonomy：疾病组织中细胞类型与状态如何稳定拆分
- disease rewiring：细胞组成、表达程序与细胞互作如何随炎症变化
- genetics-to-cell mapping：UC GWAS risk genes 如何落到具体 cell subset 与 gene module

对 T 细胞和人群免疫算法综述而言，它的重要性不在于专门建模 TCR，而在于说明组织 T-cell signal 常位于一个跨上皮、成纤维、单核和风险基因模块共同改变的系统里。

## 题录与数据
- 年份：2019
- 期刊：Cell
- DOI：https://doi.org/10.1016/j.cell.2019.06.029
- PMID：31348891
- 物种/器官：`Homo sapiens`；colon mucosa / large intestine biopsy
- 核心单细胞规模：18 名 UC patients、12 名 healthy individuals、366,650 colon mucosal cells；Single Cell Portal 当前 study summary 显示 `SCP259` 中 365,492 total cells、21,784 genes
- 主要模态：10x scRNA-seq；组织免疫染色与 bulk RNA-seq 外部验证；GWAS risk-gene/module 解释
- processed portal accession：Broad Single Cell Portal `SCP259`
- 作者代码：https://github.com/cssmillie/ulcerative_colitis

## 数据任务定义
1. 在跨 healthy、UC non-inflamed、UC inflamed colon biopsies 的背景下建统一 cell atlas。
2. 对 epithelial、stromal、immune compartments 分层聚类，得到可在 discovery/validation 样本间复用的 cell subsets。
3. 把疾病变化拆成：
   - subset abundance shift
   - within-subset differential expression
   - cell-cell interaction hub rewiring
   - UC risk-gene module enrichment
4. 对临床上关注的 anti-TNF resistance 提供 multicellular tissue-state 解释。

## 关键算法问题
1. 组织炎症会同时改变细胞比例与细胞内表达，如何避免把 composition effect 和 state effect 混写。
2. donor、biopsy region、inflammation status 和 technical variation 并存时，如何构建稳定 subset taxonomy。
3. 细胞互作推断应如何从 ligand/receptor 或 expression signatures 走向 disease-aware network comparison。
4. 如何把 GWAS risk genes 映射到 cell type、co-regulated module 和候选功能，而不是只做全局 enrichment。
5. T cells 扩张时，如何同时考虑其 stromal/myeloid/epithelial context。

## 详细算法贡献
### 1. 分 compartment 的 atlas 构建
- 论文在全组织 scRNA 数据上先按 major lineage 分层处理，再在 epithelial、stromal、immune compartments 中细化 subset。
- 输出不是粗粒度 immune/non-immune 标签，而是 51 个 subsets，包括 10 个 T-cell subsets、7 个 myeloid subsets、8 个 fibroblast subsets，以及多个 epithelial differentiation states。
- 这一做法的算法意义是：对于大组织 atlas，局部 manifold 往往比一次全局聚类更适合发现 lineage 内状态。

### 2. discovery/validation 与 subset reproducibility
- 文章明确关注 discovery 和 validation cohorts 的一致性，并用 subset classification/replication 检查 atlas 是否稳健。
- 对后续方法论文，这是一个重要边界：高 cell count 不自动等于高 reproducibility，subset 需要跨 donor、跨 biopsy replicate 和跨 disease state 验证。

### 3. disease composition 与 within-subset DE 分拆
- 作者代码仓库把主要可复现分析拆成：
  - clustering single cells into cell subsets
  - detecting ambient RNA contamination
  - significant changes in cell composition with disease
  - significant changes in gene expression with disease
- 这个拆法很适合 method report：UC 的 disease signal 不是单一 DE list，而是 abundance 与 transcriptional rewiring 两类统计对象。

### 4. ambient RNA contamination 显式处理
- 作者仓库中单列 `contamination.r`，说明组织 dissociation/10x 背景下污染信号会影响 subset 与 marker 解释。
- 对 colon 这种 secretory epithelial-rich tissue，污染建模会直接影响 immune/stromal subset 的 marker purity。
- 它不是 SoupX 式通用方法论文，但在真实组织 workflow 中把污染作为一等分析步骤保留下来。

### 5. intercellular network rewiring
- 文章把 inflammatory fibroblasts、inflammatory monocytes、microfold-like cells 和 CD8/IL-17 co-expressing T cells 放入 disease interaction hub 的解释框架。
- 算法上，这代表从 `cell subset annotation -> disease-associated subset/module -> interaction graph comparison` 的组织免疫分析链。
- 局限也清楚：这类 interaction inference 主要依赖表达共现与先验互作知识，通常不是干预因果图。

### 6. GWAS risk gene module mapping
- 论文把 UC risk genes 的 cell-type specificity 与 co-regulated modules 结合，尝试从 loci 到 cell/pathway 做归因。
- 对新算法开发，这比“把 risk genes 画在 UMAP 上”更进一步：它提出 `variant/gene set -> cell subset -> module -> tissue phenotype` 的任务结构。
- 仍有空间把 non-coding variants、allele-specific expression、eQTL 和 single-cell state uncertainty 联合起来。

## 代码专项
### 作者仓库输入
- Single Cell Portal `SCP259` 下载的 metadata：`all.meta2.txt`
- epithelial sparse matrix files：`Epi.genes.tsv`、`Epi.barcodes2.tsv`、`gene_sorted-Epi.matrix.mtx`
- stromal sparse matrix files：`Fib.genes.tsv`、`Fib.barcodes2.tsv`、`gene_sorted-Fib.matrix.mtx`
- immune sparse matrix files：`Imm.genes.tsv`、`Imm.barcodes2.tsv`、`gene_sorted-Imm.matrix.mtx`
- discovery-cohort Seurat objects：`train.Epi.seur.rds`、`train.Fib.seur.rds`、`train.Imm.seur.rds`

### 作者仓库输出
- compartment-level clustering/subset assignments
- ambient contamination diagnostics
- disease-associated cell-frequency changes
- disease-associated differential-expression tests
- manuscript-oriented markers, scores and plots

### 代码结构与意义
- `run.r` 是复现入口；`analysis.r`、`markers.r`、`scores.r`、`contamination.r`、`downsample.r` 和 `run_phenograph.py` 支撑具体步骤。
- 这是分析脚本仓库，不是可直接 pip/R package 安装的通用模型。
- 对方法综述可把它写成“组织疾病 atlas 的公开 analysis workflow”，其主要输出是 disease rewiring 证据而非新的 latent representation。

## 对 T 细胞算法的连接
- 文章指出 UC 中 CD8 与 IL-17 共表达的 T cells 随 disease expansion，并进入 interaction hubs。
- 它提醒我们：T-cell state 在 mucosal disease 中常同时受 epithelial barrier、fibroblast inflammatory program 和 monocyte recruitment 影响。
- 因为没有 TCR/V(D)J，本文不能回答 clone expansion、antigen specificity 或同克隆跨状态迁移。

## 对新算法贡献程度
- 直接新算法：**中等偏低**
- 组织疾病任务定义：**很高**
- 数据资源价值：**高**
- 对 donor-aware/multicellular modeling 启发：**很高**
- 综合判断：**P1 级 tissue immune rewiring benchmark**

## 数据可用性评估
- DOI：https://doi.org/10.1016/j.cell.2019.06.029
- 单细胞数据入口：https://singlecell.broadinstitute.org/single_cell/study/SCP259/intra-and-inter-cellular-rewiring-
- Portal accession：`SCP259`
- Portal 数据性质：human colon mucosa atlas；epithelial、stromal、immune expression matrices 与 metadata 可下载
- 原文规模：18 UC + 12 healthy donors；366,650 cells；51 subsets
- 作者代码：https://github.com/cssmillie/ulcerative_colitis
- 当前核验到的 accession 级结论：本轮定位到 `SCP259` 作为文章提供的单细胞数据入口；未在原文公开页、SCP 摘要和作者仓库入口定位到额外 GEO/SRA/EGA raw read accession
- 可复用性：processed atlas 与 analysis scripts 充足；raw-read 级复现门槛高于有 GEO/SRA/EGA raw accession 的条目

## 新算法空间
1. **Multicellular disease module model**
   - 联合 T cell、fibroblast、myeloid、epithelial programs，输出 patient-level inflammatory module 和 uncertainty。
2. **Composition-state disentanglement**
   - 把 subset abundance shift 与 within-subset transcription shift 放进 donor-aware hierarchical model。
3. **Genetics-aware tissue atlas**
   - 把 GWAS/eQTL/allelic effects 与 cell-state-specific response 联合建模。
4. **Interaction graph with causal guardrails**
   - 对 cell-cell interaction graph 加入 spatial、perturbation 或 longitudinal evidence，降低表达共现误判。

## 最终判断
`041` 最适合在 method report 中承担“组织疾病场景为什么不能只做 T-cell 单体模型”的角色。它给出公开 atlas、公开 analysis scripts 和明确 disease rewiring 问题，但 TCR、多模态蛋白层和 donor-level predictive modeling 仍是后续算法机会。
