# Algorithm Report 081

## Paper
Scirpy: a Scanpy extension for analyzing single-cell T-cell receptor-sequencing data

## 题录与实现
- DOI：https://doi.org/10.1093/bioinformatics/btaa611
- PMID：32761198
- Code：<https://github.com/scverse/scirpy>
- Docs：<https://scirpy.scverse.org>

## 算法视角定位
Scirpy 是 receptor-analysis infrastructure，而不是 cohort atlas。它解决的问题是：如何把单细胞 V(D)J receptor calls 结构化并接入 Scanpy/AnnData，使 clonotype、clonal expansion、chain pairing、CDR3 similarity 与 RNA-derived cell state 可在同一对象中分析。

## 输入与输出
- 输入：10x Cell Ranger V(D)J contig annotations、TraCeR/AIRR 格式 receptor calls、AnnData scRNA object。
- 输出：带 immune receptor metadata 的 AnnData；clonotype id；clone size；chain QC；V/J usage；receptor similarity network；repertoire diversity/overlap summaries。

## 核心算法贡献
### 1. AnnData-native receptor data model
Scirpy 将 receptor chain 信息与 cell barcode 对齐到 AnnData，形成可和 Scanpy 聚类、UMAP、cell type annotation、DE analysis 直接联动的数据结构。这是后续 clone-state coupling 的前置条件。

### 2. clonotype definition layer
支持按 CDR3 nucleotide/amino acid、V/J gene、paired alpha-beta 或 heavy-light chain 定义 clonotype，并把 clone size 回填到 cell metadata。算法意义是把序列层信息变成 cell-level 和 donor-level 可统计变量。

### 3. receptor similarity graph
除 exact clonotype 外，Scirpy 可以基于 CDR3 sequence distance 构建 similarity network，帮助发现可能具有相近 specificity 的 receptor groups。它仍属于无监督 similarity hypothesis，不等于已知抗原特异性预测。

### 4. repertoire statistics and visualization
提供 clonal expansion、V/J usage、spectratype、clone overlap、diversity 等统计，并与细胞状态图联合可视化。

## 新算法贡献程度
- 直接新模型：中等偏低。
- 工具/数据结构贡献：高。
- 对 T 细胞人群免疫算法的支撑：高。

## 局限与机会
- clonotype 通常仍是后处理 metadata，尚未进入 transcriptome/protein latent model。
- CDR3 similarity graph 没有显式整合 HLA、pMHC binding、donor covariates 或 disease outcome。
- 适合扩展为 clonotype graph + expression graph + donor hierarchy 的联合模型。

## 数据可用性评估
- 新数据 accession：不适用；软件论文无新大型实验队列。
- 代码开放：高，GitHub + 文档齐全。
- 适合复用程度：高，尤其适合作为 Python 端 TCR/BCR preprocessing layer。
