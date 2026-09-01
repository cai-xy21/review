# 《Common Trajectories of Highly Effective CD19-Specific CAR T Cells Identified by Endogenous T-cell Receptor Lineages》精读

## 论文信息

- 作者：Taylor L. Wilson、Hyunjin Kim、Ching-Heng Chou 等
- 期刊：*Cancer Discovery*
- 年份：2022；12: 2098–2119
- DOI：10.1158/2159-8290.CD-21-1508
- 原文：[Cancer Discovery](https://doi.org/10.1158/2159-8290.CD-21-1508)
- PubMed：[PMID 35792801](https://pubmed.ncbi.nlm.nih.gov/35792801/)
- 全文：[PMCID PMC9437573](https://pmc.ncbi.nlm.nih.gov/articles/PMC9437573/)
- processed data：[Dryad 10.5061/dryad.1rn8pk0x4](https://datadryad.org/dataset/doi:10.5061/dryad.1rn8pk0x4)
- raw data：[dbGaP phs002966.v1.p1](https://www.ncbi.nlm.nih.gov/projects/gap/cgi-bin/study.cgi?study_id=phs002966.v1.p1)

## 一句话结论

对 15 名实际输注患者的 184,791 个 CD19 CAR-T 单细胞和内源 αβ TCR 谱系进行纵向分析，识别出输注前可产生高效体内 cytotoxic effectors 的 precursor program，并训练出约 82.8% 准确率的产品细胞 fate classifier。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 入组/输注 | 16 人入组，15 人实际输注 | 12/15 达 CR；11/12 CR 在 4 周 MRD− |
| 单细胞 | 184,791 CAR-T | 118,749 GMP product + 66,042 post-infusion |
| 时间点 | GMP、week 1–4、week 8、month 3、month 6 | month 6 仅 7 cells，来自 1 人 |
| GEX clusters | 21 | 再归纳为 precursor/effector/dysfunctional 等功能群 |
| αβ TCR pairs | 153,853 unique pairs | 跨时间出现者定义 lineage；采样导致明显漏检 |
| processed data | Dryad，RDS/Seurat objects | 公共 domain；可直接 readRDS |
| raw GEX/scTCR/bulk TCR | dbGaP phs002966.v1.p1 | 受控访问 |
| classifier | 约 82.8% accuracy | 需患者留出/外部验证；不能按 cell-level random split 高估 |

## 1. 研究要解决的问题

输注前 CAR-T 产品中哪些细胞会在体内成为强效 cytotoxic effectors，哪些会走向 dysfunction？能否利用内源 TCR 作为自然谱系 barcode，把产品中少数 precursor 与其后代直接连接，并据此预测产品命运？

## 2. 方法框架

- SJCAR19（CD19.4-1BBζ）phase I/II pediatric B-ALL trial；
- GMP product、外周血和骨髓多个时间点分选 CAR-T；
- 10x 5′ single-cell GEX + αβ TCR enrichment；
- bulk TCR sequencing 补充克隆频率；
- 同一 αβTCR 跨时间定义 lineage，连接 pre/post states；
- clustering、pseudotime 与 machine learning 识别 effector precursor signature。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

这是一套**制造前—输注后、表达—克隆谱系、血液—骨髓**联合图谱。CAR construct 把分析限制到 CAR-expressing cells，内源 αβ TCR 则标记原始 T-cell clone，使同一克隆在多个时间点的状态可连接。

### 3.2 临床与单细胞规模

| 层级 | 规模/组成 | 应如何理解 |
|---|---:|---|
| enrolled | 16 pediatric/AYA B-ALL | 1 人未输注；主要纵向结论基于 15 人 |
| outcomes | 12 CR；11 MRD− CR at week 4 | 结局组较小 |
| GMP cells | 118,749 | 占全部 CAR-T 约 64%；平均每 cell 3,320 genes |
| post-infusion cells | 66,042 | 平均每 cell 2,299 genes |
| total | 184,791 | 平均每 patient 11,549，患者间差异大 |
| timepoints | GMP, W1, W2, W3, W4, W8, M3, M6 | M6 极度稀疏，不能做群体比例结论 |

作者仅保留至少一个 CD19-CAR UMI 的细胞，因此图谱聚焦“转录上检测到 CAR”的 cells；这会排除 CAR RNA dropout 的真实 CAR+ cells，但提高分析特异性。

### 3.3 状态图谱组成

全部 184,791 cells 分成 21 个转录 clusters。GMP 产品富集 proliferation/cell-cycle programs；post-infusion 沿两条主要方向展开：

1. cytotoxic effector trajectory：EOMES、GNLY、GZMH、GZMK、KLRD1、IFNG 等；
2. dysfunctional/terminal trajectory：抑制、应激和终末分化程序。

有效 effector 的输注前 precursor 不是最“naive”的单一群，而是一套已具 early effector readiness 又未终末失能的状态。signature 中还有 CD52、CD74、CD86、LAG3 等可测表面候选。

### 3.4 TCR 谱系图谱

靶向 CDR3 enrichment 得到 153,853 个 unique αβ pairs。同一患者中、至少一个 α链和一个 β链匹配的细胞被视为同一 clone；同一 αβ pair 在多个时间点出现则定义 lineage。

这套定义很保守但仍受采样限制：数百万输注细胞只抽样数千至数万，许多真实 persistent clones 可能只在一个时点被看到。因而“nonpersistent”更准确地说是“未在后续样本检出”。

### 3.5 Dryad 公共数据包有什么

[Dryad dataset](https://datadryad.org/dataset/doi:10.5061/dryad.1rn8pk0x4) 提供 processed single-cell expression 与 TCR 数据，使用 RDS/Seurat objects；许可为 public domain。可用：

```r
obj <- readRDS("file.rds")
obj
colnames(obj@meta.data)
Assays(obj)
```

下载前在 Dryad 文件列表逐个核对文件名、大小与 checksum；不要只引用 landing page 而不记录 dataset DOI/version。

### 3.6 raw data 如何获取

dbGaP `phs002966.v1.p1` 托管 raw single-cell GEX、single-cell TCR 和 bulk TCR sequences。申请流程包括 dbGaP 登录、机构授权、Data Use Certification 与获批下载。若只做状态/lineage 下游分析，Dryad processed objects 已足够；若要重比对、重新定义 clonotype 或审计 CAR reads，则需 dbGaP raw data。

### 3.7 下载后先做什么

1. 检查 patient、tissue（blood/marrow）、timepoint、GMP/post、cluster、CD4/CD8、TCRαβ 与 outcome 字段；
2. 对每患者单独定义 clonotype，防止 public/convergent TCR 跨患者错误合并；
3. 标记 chain completeness 和多链细胞；
4. 对 month 6 加入最低细胞阈值；
5. fate classifier 必须 patient-level leave-one-out 或外部队列验证。

## 4. 主要发现

高效 post-infusion cytotoxic cells 可由 shared endogenous TCR 追溯到 GMP 产品中的特定 precursors；这些 precursor 已表达 early effector/cytotoxic readiness，但仍具扩增能力。输注后 dysfunctional cells 也可在早期出现，说明失败不是单一晚期终点。

## 5. 预测模型

作者基于 pre-infusion transcriptome 训练分类器，报告约 82.8% accuracy 预测细胞是否成为有利 effector。该模型是 proof-of-concept；若 cell-level 随机划分会泄漏患者/克隆信息，临床转化需严格 patient-level 验证、概率校准与未知状态拒绝。

## 6. 推荐图版

- Fig. 1：184,791-cell、21-cluster atlas；
- pseudotime 图：cytotoxic 与 dysfunctional 分支；
- TCR alluvial/lineage 图：从 GMP 到 post-infusion；
- precursor classifier 图：最适合“real-time optimization”章节。

## 7. 创新价值

1. 用内源 αβ TCR 作为自然谱系 barcode。
2. 在大规模临床纵向 CAR-T 单细胞数据中连接 precursor 与后代功能。
3. 将 lineage evidence 转化为可预测、可富集的产品 signature。

## 8. 局限性

1. 仅 15 名输注患者，来自单一产品/临床试验。
2. blood/marrow 抽样导致 lineage 严重漏检。
3. 仅保留有 CAR UMI 的细胞造成选择偏差。
4. month 6 数据几乎不可用于群体比较。
5. classifier 尚未成为验证过的临床放行工具。

## 9. 对本章节的作用

这是“link transitions with molecular drivers”与“build real-time optimization systems”的代表文献：TCR 先提供真实克隆连续性，再把成功 precursor program 转化为产品预测器和候选表面富集标志。

## 10. 可直接用于综述的观点

> 内源 αβ TCR lineage tracing 可把输注前 CAR-T 状态与体内后代功能直接连接；高效 cytotoxic effectors 来源于一类具有 early effector readiness 但未终末失能的产品 precursor，提示未来制造闭环可用 lineage-validated signature 实时富集有利状态（Cancer Discovery 2022, Wilson）。

## 11. 避免误读

- 不要把 16 名入组写成 16 名都接受输注。
- 不要把“后续未检出”写成克隆确定消失。
- 不要把 153,853 αβ pairs 当成 153,853 个独立患者 clone families 跨人比较。
- 不要把 82.8% accuracy 当作已验证临床性能。

