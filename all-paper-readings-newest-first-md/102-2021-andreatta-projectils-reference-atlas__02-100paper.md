# 《Interpretation of T cell states from single-cell transcriptomics data using reference atlases》精读

## 论文信息

- 作者：Massimo Andreatta、Jesús Corria-Osorio、Sandra Müller 等
- 期刊：Nature Communications
- 年份：2021；12: 2965
- DOI：10.1038/s41467-021-23324-4
- 原文：[Nature Communications](https://www.nature.com/articles/s41467-021-23324-4)
- PubMed：[PMID 34017005](https://pubmed.ncbi.nlm.nih.gov/34017005/)
- 全文：[PMC8137700](https://pmc.ncbi.nlm.nih.gov/articles/PMC8137700/)
- 软件：[ProjecTILs GitHub](https://github.com/carmonalab/ProjecTILs)
- 论文版鼠 TIL 参考：[Figshare 12478571](https://doi.org/10.6084/m9.figshare.12478571)
- 病毒特异 CD8 参考：[Figshare 12489518](https://doi.org/10.6084/m9.figshare.12489518)
- 论文代码快照：[Zenodo 4601466](https://doi.org/10.5281/zenodo.4601466)

## 一句话结论

作者构建由 6 项研究、25 个样本和 16,803 个小鼠肿瘤 αβ T 细胞组成的统一参考图谱，并以 ProjecTILs 将查询细胞投射到固定参考空间，从而标准化 T 细胞状态注释、量化基因扰动和跨队列比较。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 论文核心参考 | 小鼠 TIL atlas | 不是统一的人 pan-cancer TIL atlas |
| 细胞 | 16,803 个 αβ T cells | 25 个样本、6 项研究 |
| 样本 | 21 个肿瘤 + 4 个新测引流淋巴结 | 肿瘤以 B16、MC38 模型为主 |
| 参考状态 | 9 个主要状态 | 标签由参考定义，query 中的新状态可能被强制吸附 |
| 病毒 CD8 参考 | 约 7,000 cells、12 spleen samples | LCMV Arm/Cl13 多时间点 |
| 人数据验证 | 30 名患者等多个公开队列 | 是跨物种投射/外部 meta-analysis，不是核心参考的组成 |
| 主参考文件 | ref_TILAtlas_mouse_v1.rds | 当前 Figshare 直链文件，R/Seurat 对象 |
| 代码快照 | ProjecTILs v0.6.1，约 17.2 MB | 软件持续更新，复现应锁定版本 |

## 1. 研究要解决的问题

单细胞研究常独立聚类和命名，导致相似 T 细胞状态被不同命名、不同状态被同一粗标签合并。论文希望：

1. 建立稳定、可解释的 TIL 参考状态空间；
2. 将新数据映射到参考，而不是每个队列从头命名；
3. 定量描述 query 偏离参考的程度；
4. 用统一坐标比较基因敲除、治疗、组织和疾病。

## 2. ProjecTILs 方法框架

### 2.1 参考图谱构建

作者用 STACAS/Seurat 对多个小鼠 TIL 单细胞数据集进行整合，得到固定 PCA、UMAP 和细胞状态。主要状态覆盖：

- naive-like；
- central-memory-like；
- effector-memory-like；
- precursor exhausted/Tpex；
- exhausted/Tex；
- resident-memory-like；
- regulatory T；
- follicular-helper-like；
- proliferating。

具体命名会随软件对象版本略有差别，正式引用以参考对象中的 functional.cluster 字段为准。

### 2.2 Query-to-reference 投射

查询数据先与参考寻找 anchors，再投射到固定 PCA/UMAP，以近邻转移标签。作者还用独立成分和 deviation score 找出 query 中相对参考发生偏移的基因程序。

这种方法的优势是坐标可比；代价是参考定义了可见状态集合，超出参考的新状态可能被映射到最近的旧状态。

## 3. 数据规模与图谱组成

### 3.1 核心鼠 TIL 参考图谱

论文核心参考并不是“5 个公开人 TIL 队列”。实际为：

| 层级 | 内容 |
|---|---|
| 物种 | mouse |
| 细胞 | 16,803 个高质量 αβ T cells |
| 样本 | 25 |
| 来源研究 | 6 |
| 肿瘤样本 | 21，主要为 B16 与 MC38 |
| 新测样本 | 4 个 MC38 tumor-draining lymph nodes |
| 统一状态 | 9 个主要 T 细胞状态 |

核心来源 accession 包括：

- GSE124691；
- GSE116390；
- GSE121478；
- GSE86028；
- E-MTAB-7919；
- 本文新生成的 E-MTAB-9274。

原始数据仍分散在各研究归档。Figshare 的 RDS 是整合后参考对象，不替代每个 accession 的 FASTQ/counts。

### 3.2 核心参考对象

[Figshare 12478571](https://doi.org/10.6084/m9.figshare.12478571) 是论文鼠 TIL 参考入口。当前 ProjecTILs 配置使用：

- 文件名：ref_TILAtlas_mouse_v1.rds；
- 直链：https://ndownloader.figshare.com/files/41398167
- MD5：679c7fe3cb1737e43cc2f84350331253。

下载：

~~~bash
wget -c \
  https://ndownloader.figshare.com/files/41398167 \
  -O ref_TILAtlas_mouse_v1.rds
~~~

读取：

~~~r
library(Seurat)
ref <- readRDS("ref_TILAtlas_mouse_v1.rds")
ref
table(ref$functional.cluster)
table(ref$sample)
~~~

Figshare 可以替换版本或改变 file ID；长期复现应同时记录 DOI、file ID、文件名、MD5、下载日期和 ProjecTILs 版本。

### 3.3 病毒特异 CD8 参考

[Figshare 12489518](https://doi.org/10.6084/m9.figshare.12489518) 提供第二个参考：

- 约 7,000 个病毒特异 CD8 T cells；
- 12 个脾脏样本；
- LCMV Armstrong 急性感染与 Clone 13 慢性感染；
- 时间点约为 4.5、7/8 和 30 天；
- 来源包括 GSE131535、GSE134139、GSE119943 的 WT 数据。

它用于比较急性感染的 effector/memory 与慢性感染的 progenitor/exhausted 状态，比肿瘤参考更适合病毒模型 query。

### 3.4 新生成数据 E-MTAB-9274

论文新生成 4 个 MC38 引流淋巴结样本，归档在 [E-MTAB-9274](https://www.ebi.ac.uk/biostudies/arrayexpress/studies/E-MTAB-9274)。这些数据补足肿瘤局部之外的淋巴状态，使参考含 naive/central memory-like 区域。

需要原始 counts/FASTQ 时，应：

1. 从 E-MTAB-9274 下载 SDRF；
2. 取得 run accession；
3. 在 ENA 下载 FASTQ；
4. 以论文/软件相符的 Seurat 流程重建。

### 3.5 论文中的 perturbation/query 数据

ProjecTILs 的核心价值通过多套独立 query 展示：

| 场景 | accession | 目的 |
|---|---|---|
| OVA 四聚体肿瘤特异 T | GSE122713 | 比较抗原特异 TIL 状态 |
| miR-155 KO | GSE121478 | 测试转录调控扰动 |
| Regnase-1 KO | GSE137015 | 识别效应/记忆偏移 |
| Ptpn2 KO | GSE134139 | 量化信号负调控缺失 |
| Tox KO | GSE119943 | 映射耗竭调控 |
| CD4 depletion | GSE137007 | 分析微环境干预 |
| 多组织 T cells | PRJEB36998 | 测试参考跨组织可用性 |

这些 query 不应加进 16,803 个参考细胞的统计中。投射结果可比较“状态偏移”，但基因敲除可能创造参考中不存在的新状态，因此需结合 deviation 分析。

### 3.6 人类数据在论文中扮演什么角色

论文将 30 名患者的黑色素瘤和基底细胞癌 T 细胞映射到鼠参考：

- GSE123139；
- GSE123813。

随后对 132 份癌症患者活检进行更广的 meta-analysis，另使用 GSE120575、GSE115978、GSE114727 以及肝癌、肺癌、结直肠癌等数据；部分整理对象归档在 [Zenodo 4263972](https://doi.org/10.5281/zenodo.4263972)。

这部分是人鼠 ortholog 投射与外部验证，不等于论文构建了 132 人的统一人参考图谱。

### 3.7 代码和版本

[GitHub](https://github.com/carmonalab/ProjecTILs) 是当前软件源；[Zenodo 4601466](https://doi.org/10.5281/zenodo.4601466) 保存论文时期 v0.6.1 快照，压缩包约 18,006,794 bytes，即 17.2 MB。

复现论文：

- 使用 v0.6.1；
- 使用论文期鼠参考对象；
- 记录 Seurat/R 依赖。

现在应用：

- 可使用当前 ProjecTILs；
- 但需执行软件自带参考下载函数并保存 sessionInfo；
- 不要把后续增加的人 CD8/CD4 atlas 当作 2021 论文原始数据。

当前软件生态另提供后发表的人 CD8 TIL atlas（约 11,021 细胞、20 样本、7 肿瘤类型）和人 CD4 atlas（约 12,631 细胞、20 样本、9 肿瘤类型）。这些是后续资源，若使用必须独立引用，不能写成本文的核心规模。

### 3.8 最小复用流程

~~~r
library(ProjecTILs)
library(Seurat)

query <- readRDS("query_seurat.rds")
ref <- readRDS("ref_TILAtlas_mouse_v1.rds")
query.projected <- make.projection(query, ref=ref)
table(query.projected$functional.cluster)
~~~

运行前要统一物种、基因符号、assay、counts 和归一化方式；跨物种映射还需明确 ortholog 策略。

## 4. 主要方法学发现

### 4.1 固定参考提升可比性

不同 query 被放到同一坐标系，状态比例和基因程序可直接比较，避免每个研究独立聚类后再人工对标签。

### 4.2 参考标签与 deviation 互补

标签回答“最像哪个已知状态”，deviation 回答“相对该状态发生了什么变化”。对基因敲除和药物扰动，后者尤其重要。

### 4.3 TCR 的作用

部分人队列具有 TCR，可检验同一克隆在不同映射状态的分布。TCR 不是核心参考每个细胞的必备输入，ProjecTILs 本身主要根据 GEX 投射。

## 5. 关键图表怎么读

- 参考 UMAP：固定坐标利于比较，但二维邻近仍不是谱系方向。
- Query projection：近邻标签是参考条件下的最佳匹配，不是绝对真值。
- KO deviation：比只看状态比例更能发现扰动特异程序。
- 人鼠投射：支持保守性，但 ortholog 损失和物种差异不可忽略。

## 6. 创新点

1. 把 T 细胞注释从每队列独立聚类转为参考映射。
2. 同时提供标签、坐标和偏离程度。
3. 系统演示多种遗传与治疗扰动。
4. 提供可直接读取的参考对象和 R 软件。

## 7. 局限性

1. 论文核心参考为小鼠、肿瘤模型有限。
2. Query 中全新状态可能被强制映射到最近参考。
3. 参考标签依赖训练集和人工注释。
4. 跨物种只保留 ortholog，可能丢失关键人特异程序。
5. UMAP 投射不是动态轨迹或因果证据。
6. 软件和参考对象持续更新，版本漂移影响复现。

## 8. 对本综述的作用

该论文特别适合“quantitatively characterizing cell phenotypes”和“build real-time optimization systems”：

- 参考坐标可作为细胞状态传感器的离线版本；
- deviation score 可作为优化目标，而不只是离散标签；
- 未来可将快速转录/蛋白读出映射到固定参考，实时调整培养或刺激条件。

## 9. 可直接写进综述的表述

> ProjecTILs 以 16,803 个小鼠肿瘤 αβ T 细胞参考图谱为固定状态空间，使独立队列和遗传扰动能够获得可比较的状态标签与偏离分数；这一“参考映射—偏差量化”框架可为细胞状态闭环优化提供计算接口。

## 10. 最容易误读的地方

- 核心参考是 mouse TIL，不是 5 个或更多人类 TIL 队列。
- 16,803 是参考细胞数，不含所有 query。
- 132 份人癌活检是外部 meta-analysis。
- 后续人 CD8/CD4 atlas 不是 2021 论文原始资源。
- 投射标签说明相似性，不证明谱系、方向或因果。
