# 023. Analyzing the Mycobacterium tuberculosis immune response by T-cell receptor clustering with GLIPH2 and genome-wide antigen screening

## 基本信息
- 年份：2020
- 期刊：Nature Biotechnology
- DOI：https://doi.org/10.1038/s41587-020-0505-4
- PMID：32341563
- 主题：TCR specificity grouping；CDR3 motif；repertoire statistics；antigen discovery

## 为什么重要
GLIPH2 处理的是 T 细胞算法里最关键但最难直接观测的问题之一：当我们只有大量 TCR 序列时，哪些受体可能有相近 antigen specificity。该论文把 TCR beta repertoire clustering、统计 enrichment 与 genome-wide Mtb antigen screening 接在一起，使“sequence-to-specificity hypothesis”成为可操作计算任务。

## 数据与研究设计
- TCR cohort：58 名 latent `Mycobacterium tuberculosis` infected individuals
- repertoire 输入：`19,044` unique TCR beta sequences
- antigen validation：用 artificial antigen-presenting cells 与 reporter T cells 筛选 `3,724` 个 Mtb proteins，覆盖约 `95%` 的 Mtb protein-coding genes
- 物种/样本：人类 latent Mtb infection cohort；PBMC-derived T-cell receptor repertoires
- 任务边界：论文核心不是 scRNA-seq state atlas，而是 `TCR sequence -> specificity group -> antigen hypothesis/validation`

## 核心贡献
1. **GLIPH2 的可扩展性**：论文将算法描述为可处理 millions of TCR sequences 的升级版 grouping method。
2. **把相似性变成 specificity hypothesis**：不是泛泛做字符串聚类，而是寻找与共享抗原识别相关的 sequence motifs 与 group evidence。
3. **群体层证据**：同组 TCR 的跨个体 convergence、V gene/HLA 等信息为 group plausibility 提供统计支持。
4. **抗原筛选闭环**：Mtb ORF-scale screen 为算法输出提供功能验证场景。

## 与 T 细胞-人群免疫力的关系
- 人群免疫力差异不只在“有多少 T 细胞”，还在“受体谱能识别什么”。
- GLIPH2 让共享 specificity、public-like motifs 与 cohort enrichment 可以被系统挖掘，适合感染、疫苗、肿瘤 neoantigen 与 autoimmune specificity hypotheses。
- 它对单细胞 TCR 很有价值，但它本身不使用 transcriptome/protein state；将 specificity group 与 exhausted/cytotoxic/memory state 联合起来，仍需下游分析或新模型。

## 文章中的算法贡献
### 1. specificity group 的输入对象
- 以 TCR beta CDR3 sequence 为中心，结合 V gene、subject、count/repertoire metadata 等信息组织 repertoire。
- 输出对象不是单细胞 cluster，而是候选 specificity groups 与 group-level statistics。

### 2. global/local sequence evidence
- GLIPH 系列的关键思想是把共享局部 motif 与整体相似 CDR3 模式视为可能的共同 paratope hotspots。
- GLIPH2 升级了大 repertoire 分析的工程效率，使 cohort-level screening 不必停留在小数据手工比对。

### 3. statistical evidence layer
- group 的可解释读数包括 motif、聚合序列、跨个体共享、V gene/HLA enrichment 等。
- 这一步很重要：同一 edit distance 下的序列相近不自动等于同 antigen specificity，群体统计证据用于减少纯字符串相似造成的误判。

### 4. antigen-screen validation
- 算法先形成候选 specificity groups，再用覆盖 Mtb proteome 的 antigen screen 寻找 groups 对应的 targeted proteins/epitopes。
- 因而这篇方法论文的 contribution 不只是 cluster algorithm，也包含“computational candidate -> functional antigen testing”的设计范式。

## 相比已有方法的算法增量
- 相比只跟踪 clonotype expansion：GLIPH2 进一步猜测不同 clonotypes 是否收敛到相近 specificity。
- 相比 transcriptome-only T-cell state discovery：GLIPH2 触及 antigen-facing receptor grammar。
- 相比无统计约束的 TCR clustering：它强调 group enrichment 与 biological plausibility。

## 局限与新算法空间
1. **以 TCR beta 为主**：alpha-beta pairing、paired-chain information 与 chain multiplicity 的利用受限。
2. **状态信息缺失**：specificity group 与 T-cell phenotype 仍是后接，而非 joint model。
3. **不确定性与 negatives**：sequence convergence 只能给 specificity hypothesis，跨 disease/ancestry/HLA transfer 的校准仍是问题。
4. **新算法机会**：
   - paired TCR encoder + GLIPH-like statistical evidence
   - TCR group + scRNA/ADT state joint latent model
   - donor/HLA-aware specificity uncertainty
   - antigen-specific population immunity score

## 数据可用性
- 文章提供的数据层：
  - Supplementary Table 1：Mtb-specific TCR sequences and summary
  - Supplementary Table 2：VDJdb TCR sequences
  - Supplementary Table 3：GLIPH2 specificity groups
  - Supplementary Table 4：whole Mtb ORF clone set gene list
- 数据性质：58 名 latent Mtb cohort 的人 TCR beta repertoire；功能筛选覆盖 3,724 个 Mtb proteins
- 独立 accession：论文页面本轮未给出类似 GEO/SRA 的主要 repertoire accession；可复用数据主要随 supplementary tables 发布
- 文章提供的代码：Nature Supplementary Code，包含 two compiled standalone versions of GLIPH2
- 代码输入：TCR repertoire table，核心字段围绕 CDR3 beta sequence 与 repertoire/subject/V-gene/HLA 等 metadata
- 代码输出：candidate specificity groups、motif/group enrichment statistics、group annotations，可接 antigen hypotheses
- 模型结构与意义：motif/global-similarity grouping + repertoire/statistical enrichment evidence；把 TCR sequence similarity 提升成可检验的 specificity hypothesis
- 复用判断：**中高**。论文数据表和 compiled code 可定位，但现代 pipeline 集成、源码可审计性、paired single-cell RNA/TCR joint modeling 仍需额外工作。

## 可放入 method report 的表述
GLIPH2 是 repertoire sequence algorithm 的代表：它把 TCR specificity 从“按 clonotype 数频次”推进到“跨 clonotype sequence convergence 的候选 specificity groups”，但尚未与单细胞状态、donor hierarchy 和抗原暴露结局形成统一模型。

## 一句话结论
GLIPH2 是本批论文里最直接触及 T 细胞识别特异性的算法工作，适合用来支撑后续 `TCR sequence + cell state + donor` 新模型空间。
