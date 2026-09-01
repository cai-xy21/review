# 043. Single-cell RNA-seq reveals new types of human blood dendritic cells, monocytes, and progenitors

## 基本信息
- 年份：2017
- 期刊：Science
- DOI：https://doi.org/10.1126/science.aah4573
- PMID：28428369
- PMCID：PMC5775029
- 主题：human blood immune atlas；DC/monocyte taxonomy；rare immune subsets；reference building

## 为什么重要
这篇是人类血液免疫单细胞图谱的早期经典。它不以 T cells 为主角，但它证明了全转录组单细胞发现可以重写免疫细胞分类，并直接影响 antigen presentation 和 T-cell activation reference context。

## 数据与研究设计
- 物种/来源：健康人外周血，PBMC 中富集 DC、monocyte 和 progenitor-like populations
- 主技术：Smart-seq2 full-length scRNA-seq
- 总体：约 2,400 个单细胞 profiles
- GEO 分期：
  - exploratory phase：1,140 single cells + 12 population samples
  - deep characterization phase：额外 1,261 single cells + 9 population samples
- 采样策略：先用抗体 cocktails 富集目标 immune fractions，再做 single-cell transcriptomics 与 follow-up phenotype/function validation

## 核心贡献
1. 发现并验证新的 blood DC and monocyte subtypes，包括 AXL/SIGLEC family characterized AS DC。
2. 提出 circulating cDC progenitor 的存在与表型线索。
3. 显示传统 marker taxonomy 会混入功能上不同的 cell states。
4. 为后续 immune monitoring 提供更细的 gene signatures 和 reference structure。

## 与 T 细胞和人群免疫力的关系
- DC/monocyte 是 T-cell priming、antigen presentation 与 inflammatory cue 的上游。
- 这篇文章说明 donor immune baseline 不能只看 T cells，还需要 reference-quality APC state annotation。
- AS DC 等亚群对 T-cell activation 解释尤其相关。

## 算法与分析视角
- 以 unsupervised single-cell clustering 和 transcriptome-wide signatures 推进 taxonomy。
- 采用 discovery 后再 deep characterization 的设计，降低稀有 state 只在单批数据中“被发现”的风险。
- 最终输出 cluster、signature 与可 prospectively isolate 的 marker hypothesis，而不是一个可安装的软件模型。

## 数据可用性
- GEO：https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE94820
- GEO accession：`GSE94820`
- raw controlled access：dbGaP `phs001294.v1.p1`
- BioProject：`PRJNA374527`
- processed matrices：GEO supplementary discovery/deep-characterization expression matrices
- Single Cell Portal：https://singlecell.broadinstitute.org/single_cell/study/SCP43/atlas-of-human-blood-dendritic-cells-and-monocytes
- Portal accession：`SCP43`
- 作者独立代码：本轮未定位到专用代码仓库

## 对新算法开发的启发
1. reference annotation 应支持 open-world novelty detection。
2. 稀有亚群发现应有 discovery/validation protocol。
3. donor-level T-cell model 可加入 APC composition/state context。
4. transcriptome-to-marker-panel 的反向设计仍有方法空间。

## 可信度与边界
- 可信度：高
- 强项：Science 经典、两阶段 single-cell design、后续表型和功能验证
- 边界：主要研究 APC lineage；raw data controlled；不是 TCR 或多模态算法论文

## 一句话结论
`043` 说明单细胞算法首先改变了 immune taxonomy，本身就是后续 T-cell population modeling 的 reference 基础。
