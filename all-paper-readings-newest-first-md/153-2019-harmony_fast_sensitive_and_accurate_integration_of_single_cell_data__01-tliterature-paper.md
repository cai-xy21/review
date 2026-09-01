# 017. Harmony: fast, sensitive and accurate integration of single-cell data

## 基本信息
- 年份：2019
- 期刊：Nature Methods
- DOI：https://doi.org/10.1038/s41592-019-0619-0
- PMID/PMCID：`31740819` / `PMC6884693`
- 主题：Harmony；快速整合；embedding-level correction
- 可信度：高

## 文章定位
Harmony 是单细胞整合方法中非常实用的一支路线。相比更重型的方法，它在速度、可扩展性和易用性上优势明显，因此在大规模免疫图谱分析中被频繁使用。

## 亮点
1. 在低维嵌入空间中进行快速迭代整合，适合大数据规模。
2. 能处理多个协变量，实用性强。
3. 在很多免疫数据中较好保留了细粒度状态结构。

## 核心贡献
- 提供了一种与 anchor-based integration 不同的整合范式。
- 让大规模 atlas 分析更可扩展。
- 在保持细胞状态结构方面常比激进校正更稳健。

## 与 T 细胞—人群免疫力的关系
T 细胞状态细微、容易被过度校正抹平。Harmony 之所以重要，是因为它常常能在整合 donor/batch 的同时保留 biologically meaningful T-cell substructure。

## 算法/分析贡献
- 核心是 embedding-level iterative correction。
- 在效率与尺度上很有优势。
- 代表了“轻量但高效”的整合范式。
- 算法在已有 PCA/低维 embedding 上交替做 soft clustering 与 batch-aware correction，避免重写完整 count model；输入是 embedding 与 covariate metadata，输出是 corrected embedding。

## 对新算法开发的启发
1. donor-aware 的 lightweight integration
2. 保持稀有 T-cell states 的鲁棒整合
3. 多协变量联合整合与可解释校正

## 数据可用性
- 代码：<https://github.com/immunogenomics/harmony>
- 论文 benchmark 复用 PBMC、pancreatic islet、mouse embryogenesis 和 spatial transcriptomics 等公开数据；不是单一新 accession 型 immune cohort
- 教程与社区使用广泛
- 复用价值：高

## 影响因子/可信度综合判断
- 期刊层面：Nature Methods
- 社区采用度：高
- 方法实用性：高
- 综合判断：**高可信度、实用性极强的方法论文**

## 一句话结论
Harmony 是大规模免疫图谱工作里最常用、也最接地气的整合工具之一。
