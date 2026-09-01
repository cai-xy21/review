# 061. Single-cell transcriptomics of human T cells reveals tissue and activation signatures in health and disease

## 基本信息
- 年份：2019
- 期刊：Nature Communications
- DOI：https://doi.org/10.1038/s41467-019-12464-3
- PMID/PMCID：PMID 31619651；PMCID PMC6797728
- 主题：human T-cell reference；tissue signature；activation response；disease projection

## 为什么重要
这篇文章是 T-cell-centered reference map 的早期代表。它不把 T 细胞当作大型 atlas 里的一个小类，而是专门比较人类血液、肺、肺引流淋巴结、骨髓 T 细胞在 resting 与 TCR stimulation 后的单细胞转录状态。对“单细胞组学 × T 细胞 × 人群免疫力”来说，它给出的核心启发是：T 细胞状态不是单一分化轴，至少同时受 lineage、tissue context 和 activation condition 影响。

## 数据与研究设计
- 样本对象：2 名健康器官供者提供 lung、lymph node、bone marrow；2 名健康成人外周血供者提供 blood T cells
- 样本类型：人类 CD3+ T cells，来自 mucosal、lymphoid、bone marrow 与 circulating compartments
- 物种/器官：Homo sapiens；blood、lung、lung-draining lymph node、bone marrow
- 细胞规模：>50,000 resting and activated human T cells
- 实验条件：resting 与 anti-CD3/CD28 stimulation/activation
- 模态：10x Genomics Chromium 3' scRNA-seq；配合流式/marker 验证；外部 tumor T-cell datasets projection
- 研究目标：建立健康人类 T-cell activation reference，并用它解释 disease-associated T cells，尤其肿瘤浸润 T 细胞的状态偏移

## 核心亮点
1. **T-cell 专题化 reference**：比一般 PBMC atlas 更适合作为 T-cell state annotation 和 activation benchmarking 资源。
2. **组织与激活双变量设计**：同一框架内比较 tissue residence 和 TCR activation response，避免把 tissue effect 误写成 activation/exhaustion。
3. **健康到疾病投影**：把多个 tumor-associated T-cell scRNA 数据投影到健康参考空间，形成 early reference mapping 范式。
4. **算法问题定义清晰**：它直接提出 tissue-aware、activation-conditioned、out-of-reference disease-state mapping 的任务。

## 核心贡献
- 识别人类组织 T 细胞的 mucosal/lymphoid tissue signatures，包括 lung TRM-like programs 和 lymph-node Treg enrichment。
- 揭示 activation 后 CD8 T cells 的 cytotoxic/effector programs，以及 CD4 T cells 的 interferon-response intermediate state。
- 证明血液 T 细胞不能简单代表组织 T 细胞；blood-to-tissue inference 必须纳入 tissue prior。
- 用健康 reference 解释 tumor T-cell states，为后续 atlas transfer 和 disease projection 提供可复用思路。

## 与 T 细胞-人群免疫力的关系
人群免疫力差异常被外周血采样间接估计，但这篇文章说明：同一 T 细胞 lineage 在不同组织和激活背景下会有明显不同的 transcriptional state。若要从 blood scRNA 推断 tissue immunity 或 disease immunity，算法必须显式处理 tissue domain shift、activation state 和 reference coverage。

## 文章中的算法/分析流程
### 1. T-cell reference map construction
作者从各组织分选 CD3+ T cells，经过常规 scRNA QC、normalization、highly variable gene selection、clustering、UMAP/community detection 和 marker-based annotation，构建 tissue/activation aware reference。该流程不是新算法，但它把 T-cell annotation 的协变量结构定义得很清楚。

### 2. Activation-state decomposition
文章不是只比较 resting vs activated，而是识别 activation 后多个 transcriptional endpoints：CD8 cytotoxic effector modules、cytokine/chemokine modules、CD4 IFN-response state 等。这对后续方法很关键，因为 activation 应被建模为连续、多端点扰动，而不是二元标签。

### 3. Blood/tissue and disease projection
作者将 blood T cells 以及 tumor T-cell datasets 投影到 tissue T-cell reference，使用 UMAP projection 和 scmap 等 reference mapping 思路，观察 disease T cells 在健康 activation reference 中的位置。这是 early atlas-transfer workflow，强调 query 数据可能落在 reference 边界外。

### 4. 可复用代码组件
原文 Data Availability 给出三个相关代码入口：
- marker selection、clustering、differential expression：<https://github.com/simslab/cluster_diffex2018>
- scHPF：<https://github.com/simslab/scHPF>
- UMAP projection analysis：<https://github.com/simslab/umap_projection>

## 对算法工作的启发
1. **Tissue-aware T-cell reference transfer**：query cell 的 label 应同时输出 tissue/activation/lineage posterior 与不确定性。
2. **Activation-conditioned latent model**：将 stimulation response 作为 continuous perturbation axis，而不是离散处理。
3. **Blood-to-tissue inference**：从血液预测组织 T-cell state 时必须估计 domain shift 与不可观测 tissue states。
4. **Out-of-reference detection**：tumor/exhausted/therapy-induced states 可能不在健康 reference 覆盖内，需要 OOD score。

## 数据可用性
- GEO accession：GSE126030
- GEO 链接：<https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE126030>
- 数据内容：scRNA-seq count matrix、cell barcodes、UMAP coordinates、tissue origin、stimulation condition、CD4/CD8 status、CCL5 expression 等 source data
- 样本性质：>50,000 human T cells；healthy organ donor tissues and healthy blood donors；resting/activated conditions
- 代码链接：`cluster_diffex2018`、`scHPF`、`umap_projection`，见上文
- 代码输入：cell-gene count matrices、cell metadata、reference/query cell embeddings or expression objects
- 代码输出：cluster/differential-expression tables、scHPF factors、UMAP projection results
- 模型结构与意义：主要是 reference-map workflow 和 factor/projection components，不是端到端 T-cell foundation model

## 可信度评估
- 期刊层面：Nature Communications，可信度高
- 可复现性：数据 accession 与关键代码入口明确；完整复现仍需整理 tissue/stimulation metadata 与外部 tumor projection datasets
- 局限：donor 数很小；无 TCR-seq；健康 reference 不一定覆盖 tumor exhaustion、antigen-specific expansion 或治疗诱导状态
- 综合判断：**主题高度相关，数据规模中高，直接算法创新低到中，任务定义价值高**

## 一句话结论
这篇文章应作为 tissue-aware and activation-conditioned T-cell reference 的核心文献：它不是新通用算法论文，但清楚定义了后续 T-cell atlas transfer 和 population immunity modeling 必须解决的协变量问题。
