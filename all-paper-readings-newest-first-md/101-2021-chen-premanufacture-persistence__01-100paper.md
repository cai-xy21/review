# 《Integrative Bulk and Single-Cell Profiling of Premanufacture T-cell Populations Reveals Factors Mediating Long-Term Persistence of CAR T-cell Therapy》精读

## 论文信息

- 作者：Grace M. Chen、Chao Chen、Ranjan K. Das 等
- 期刊：*Cancer Discovery*
- 年份：2021；11: 2178–2195
- DOI：10.1158/2159-8290.CD-20-1677
- 原文：[Cancer Discovery](https://doi.org/10.1158/2159-8290.CD-20-1677)
- PubMed：[PMID 33820778](https://pubmed.ncbi.nlm.nih.gov/33820778/)
- 全文：[PMCID PMC8419030](https://pmc.ncbi.nlm.nih.gov/articles/PMC8419030/)
- 数据：[dbGaP phs002323.v1.p1 / current v2](https://www.ncbi.nlm.nih.gov/projects/gap/cgi-bin/study.cgi?study_id=phs002323.v2.p1)
- 交互图谱：[T-cell Atlas Shiny](https://tanlab4generegulation.shinyapps.io/Tcell_Atlas/)

## 一句话结论

对 71 名患儿/年轻患者的制造前 T 细胞分群 bulk RNA 图谱及 6 人 CITE-seq/scATAC 数据表明，持续 IFN/IRF7 程序与较短 CAR-T 持久性相关，而 TCF7 regulon 在 naive 及部分 effector T cells 中维持，与长期 B-cell aplasia 相关。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 队列 | 71 名儿童/年轻患者 | 70 B-ALL，1 Hodgkin lymphoma；65 例制造成功 |
| bulk atlas | 355 profiles | 71 人 × 5 个 FACS T-cell subsets |
| subsets | TN、TSCM、TCM、TEM、TEFF | 先分选再测序，降低组成混杂 |
| CITE-seq | 17,750 cells（QC 后） | 6 人；同细胞 RNA + surface ADT |
| scATAC-seq | 19,673 nuclei/cells（QC 后） | 同 6 人的平行 aliquots；不是同一个细胞的 joint assay |
| CITE 状态 | 11 个最终 T-cell subsets | 初始 21 clusters 后按 RNA/protein 合并 |
| 临床 endpoint | B-cell aplasia duration | ≥6 月 long persistence；<6 月 short/failed |
| 访问 | dbGaP 受控 | bulk 和 single-cell raw/individual-level data 需申请 |

## 1. 研究要解决的问题

制造前 leukapheresis T-cell 的哪些内在程序决定 CAR-T 在体内长期功能持久？早期记忆比例之外，是否存在跨 T-cell subset 的转录调控网络可作为制造前筛选或重编程靶点？

## 2. 方法框架

- 71 人制造前 T cells 分选为 TN/TSCM/TCM/TEM/TEFF；
- SMART-Seq v4 bulk RNA-seq 建立 355-profile atlas；
- 以 B-cell aplasia 持续时间作为功能 CAR-T persistence readout；
- 6 人做 10x 3′ CITE-seq 和独立 aliquot 的 10x scATAC-seq；
- regulon/TF network 分析聚焦 IRF7、TCF7/LEF1；
- 将表型组成、基因表达和染色质 motif/accessibility 与结局整合。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

核心不是对成品 CAR-T 测序，而是对**制造前起始材料**建图。bulk 层用五类 FACS subset 扩大到 71 人；single-cell 层在 6 人中用 CITE-seq 和 scATAC 解开同一 subset 内部的状态/调控异质性。

### 3.2 临床与 bulk 图谱规模

| 层级 | 规模/内容 | 应如何理解 |
|---|---:|---|
| 患者 | 71；平均年龄 11.7 岁 | 主要为 pediatric B-ALL |
| 制造成功 | 65/71 | intention-to-treat 分析需区分失败制造 |
| bulk profiles | 355 | 理论上 71×5；每个 profile 是分选群体平均表达 |
| 5 subsets | TN、TSCM、TCM、TEM、TEFF | marker 基于 CCR7/CD62L/CD45RO/CD95 |
| persistence | cohort median 约 11 月 | BCA 是 anti-CD19 CAR 功能 proxy，不等于直接数 CAR-T |

预先分选的优势是可在同一 T-cell type 内找结局相关程序；否则 naive 比例变化会伪装成 TCF7/IFN 差异。

### 3.3 单细胞多组图谱

| modality | 患者 | QC 后规模 | 图谱组成 |
|---|---:|---:|---|
| CITE-seq | 6 | 17,750 cells | 3′ GEX + TotalSeq-A ADT；21 transcript clusters 合并为 11 类 |
| scATAC-seq | 6 | 19,673 nuclei | 独立 aliquot；chromatin peaks、motifs、gene activity |

11 个 CITE 状态覆盖 CD4/CD8 naive、memory、effector，以及 resting/activated Treg。六人的临床持久性从约 2 月失败到 >22 月，属于“极端/范围覆盖”的机制性子队列，不应代表 71 人的精确总体比例。

### 3.4 图谱的调控组成

慢性 IFN response 在多种 subset 中与较差持久性相关，IRF7 是关键调控节点。TCF7 regulon 与 naive/early-memory 相关，但在长期持久患者的一部分 effector cells 中仍可保持；因此 TCF7 与 effector marker 并非严格互斥。

### 3.5 数据访问与下载

`phs002323` 是受控 dbGaP study：

1. 登录 dbGaP，查看 study version 与 Data Use Certification；
2. 由机构 PI/Authorized Organizational Official 提交申请；
3. 获批后通过 dbGaP download tools 获取 bulk RNA、CITE-seq、scATAC 及 phenotype；
4. 报告中记录具体 version（论文 v1，后续资源常列 v2），避免版本歧义。

未获批前可使用论文 Supplementary Tables、figure source 与 Shiny atlas 浏览基因/regulon，但交互网页不能替代原始矩阵。不要声称 GEO 匿名开放。

### 3.6 下载后先做什么

先建立 `patient–subset–assay–persistence` 映射并检查 355 profiles 是否有缺失/低细胞数；CITE 与 ATAC 来自平行 aliquots，只能做跨模态锚定，不能声称单细胞一一配对。结局模型应以患者留出验证，避免将 5 个 subset 当作 5 个独立患者。

## 4. 主要发现

早期/naive-memory 组成与长期 persistence 相关；但在校正组成后，持续 IFN/IRF7 仍是跨 subset 的不利程序。TCF7 regulon 则是更有利的干性/记忆调控轴，并可在部分 effector cells 中保留。

## 5. 对状态导航的含义

制造优化不应只按表面 marker 追求 TN/TSCM 百分比，还应减少 chronic IFN imprint，并保留 TCF7/LEF1 network。这里提出的可导航变量是“起始细胞组成 + 细胞内调控状态”双层结构。

## 6. 推荐图版

- Fig. 1：71 人、5 subsets、355 profiles 与临床 persistence；
- IFN/IRF7 图：不利调控网络；
- TCF7 regulon 图：长期 persistence；
- Fig. 6：17,750 CITE cells + 19,673 ATAC cells 的多组整合。

## 7. 创新价值

1. 大型临床制造前 T-cell subtype-specific transcriptomic atlas。
2. 先分选再比较，分离组成与细胞内程序。
3. bulk 发现由 CITE/scATAC 解析到单细胞调控网络。

## 8. 局限性

1. 主队列以儿童 B-ALL 为主，跨年龄/疾病外推有限。
2. single-cell 子队列仅 6 人。
3. BCA 是功能持久性的 surrogate endpoint。
4. CITE 与 ATAC 非同细胞测量。
5. 受控访问提高复现门槛。

## 9. 对本章节的作用

适合“optimize conditions for navigating T-cell states”：把起始材料的 TCF7 与 IFN/IRF7 network 转化为制造前选择和培养条件优化的候选目标。

## 10. 可直接用于综述的观点

> 制造前 T-cell 质量不只是 naive/memory 比例问题；在同一分选亚群内，IRF7 主导的持续 IFN imprint 与较短持久性相关，而 TCF7 regulon 可在有利的 effector cells 中继续保留，为制造阶段的状态重编程提供调控靶点（Cancer Discovery 2021, Chen）。

## 11. 避免误读

- 不要把 355 bulk profiles 写成 355 位患者。
- 不要把 6 人单细胞子队列外推为全队列比例。
- 不要把 CITE 与 scATAC 写成同一细胞 joint multiome。
- 不要把 B-cell aplasia 等同于直接 CAR-T 细胞计数。

