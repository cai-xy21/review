# 014. Multiplexed droplet single-cell RNA-sequencing using natural genetic variation

## 基本信息
- 年份：2018
- 期刊：Nature Biotechnology
- DOI：https://doi.org/10.1038/nbt.4042
- PMID/PMCID：`29227470` / `PMC5784859`
- 主题：demuxlet；样本混池；单细胞人群研究方法
- 可信度：高

## 文章定位
这篇文章是做人群单细胞研究时非常关键的方法论文。它解决的不是生物学问题本身，而是一个决定大规模队列研究能否成立的核心技术问题：如何在一个 pooled 实验里识别细胞属于哪个 donor。

## 亮点
1. 提出 demuxlet，使多 donor 混池单细胞实验成为现实。
2. 在节约成本的同时减少批次效应。
3. 还能同时识别 doublets，显著改善队列级实验设计。

## 核心贡献
- 把人群单细胞研究从小规模、单样本实验推进到 cohort 级设计。
- 为后续 PBMC、疫苗、感染、人群免疫差异研究奠定实验与分析基础。
- 使 donor-aware 设计在技术上可实施。

## 与 T 细胞—人群免疫力的关系
很多 T 细胞相关的人群研究必须比较多个 donor。没有 donor demultiplexing，就难以高效、低批次地构建大样本队列。这篇文章因此是人群免疫力研究的底层关键方法。

## 算法/分析贡献
- 核心算法是基于自然遗传变异的**概率型 donor demultiplexing**。
- 该方法将 scRNA-seq reads 与基因型信息匹配，实现 donor assignment 和 doublet detection。
- 它的意义不只是一个工具，而是定义了“cohort-scale single-cell”这一实验与计算范式。
- 计算输入是 scRNA-seq 中覆盖 SNP 的 allele reads 加 donor genotype reference；输出是每个 barcode 的 donor posterior/assignment、singlet/doublet/ambiguous 标签与 doublet pair 解释。

## 对新算法开发的启发
1. 可继续发展无基因型或弱基因型条件下的 demultiplexing。
2. 可将 donor identity 与 downstream state model 联合建模。
3. 可把 demultiplexing、doublet、batch 统一纳入层级模型。

## 数据可用性
- GEO：`GSE96583`
- 数据性质：
  - `Homo sapiens` PBMC pooled-donor droplet scRNA-seq
  - 论文覆盖 8 个 pooled lupus donor 的 IFN-beta 反应样例，并扩展到 23 个 pooled samples 的 eQTL 使用场景
  - GEO 摘要给出约 15k PBMC 的人群反应分析数据
- 代码：
  - demuxlet 已纳入 `popscle` 工具族：<https://github.com/statgen/popscle>
  - 数据链接：<https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE96583>
- 方法可复现性：高
- 社区采用度：高

## 影响因子/可信度综合判断
- 期刊层面：Nature Biotechnology
- 实用性与复用率：极高
- 对人群研究的重要性：极高
- 综合判断：**高可信度、高复用度基础方法论文**

## 一句话结论
如果没有 demuxlet 这类方法，很多大规模 T 细胞人群免疫研究根本无法经济且稳健地完成。
