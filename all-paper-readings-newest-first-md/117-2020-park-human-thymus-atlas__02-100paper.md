# 《A cell atlas of human thymic development defines T cell repertoire formation》精读

## 论文信息

- 作者：Jong-Eun Park、Rachel A. Botting、Cecilia Domínguez Conde 等
- 期刊：Science
- 年份：2020；367(6480): eaay3224
- DOI：10.1126/science.aay3224
- 原文：[Science](https://www.science.org/doi/10.1126/science.aay3224)
- PubMed：[PMID 32079746](https://pubmed.ncbi.nlm.nih.gov/32079746/)
- 全文：[PMC7611066](https://pmc.ncbi.nlm.nih.gov/articles/PMC7611066/)
- 原始数据：[ArrayExpress E-MTAB-8581](https://www.ebi.ac.uk/biostudies/arrayexpress/studies/E-MTAB-8581)
- HCA 项目页：[Human thymus development](https://explore.data.humancellatlas.org/projects/c1810dbc-16d2-45c3-b45e-3e675f88d87b)
- 论文版代码和矩阵：[Zenodo 3572422](https://doi.org/10.5281/zenodo.3572422)
- 持续更新数据集：[Zenodo concept DOI](https://doi.org/10.5281/zenodo.3572421)

## 一句话结论

作者整合 15 份 7–17 周胎龄胸腺与 9 份出生后胸腺，绘制 255,901 个细胞、超过 50 个状态的人胸腺发育图谱，并把转录状态、TCR 重排和胸腺微环境结合起来，解析常规与非常规 T 细胞谱系形成。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 生物样本/供者 | 24：15 胎儿、9 出生后 | 数据库中的 74 source records 不是 74 名供者 |
| 发育范围 | 胎龄 7–17 周至出生后 | 年龄跨度大，批次与发育阶段需同时建模 |
| 细胞 | 255,901 | 138,397 个发育期细胞 + 117,504 个出生后细胞 |
| 状态 | 超过 50 类 | 含胸腺基质、髓系及多条 T 细胞发育分支 |
| 技术 | 10x 3′/5′、Smart-seq2、TCRαβ、smFISH | smFISH 是靶向空间验证，不是空间转录组 |
| 原始归档 | E-MTAB-8581；103 runs、206 FASTQ | 当前 submitted 数据约 2.62 TiB |
| 论文版 Zenodo | 约 2.43 GiB | 包含注释矩阵、代码和样本表 |
| 当前 concept 版本 | 约 4.35 GiB | 另含 VDJ 和原始 counts h5ad；应记录具体版本 |

## 1. 研究要解决的问题

胸腺是 T 细胞状态导航的生理原型：前体在受控的空间与信号序列中经历增殖、TCR 重排、正负选择、谱系决定和输出。论文要解决：

1. 人类胎儿到出生后胸腺包含哪些细胞状态；
2. 常规 αβ T、γδ T 及 CD8αα 等非常规谱系如何分叉；
3. TCR 基因重排和选择如何与转录状态对应；
4. 胸腺上皮、成纤维、内皮和髓系细胞如何构成支持性微环境。

## 2. 方法框架

### 2.1 多平台单细胞测序

主体采用 10x Genomics 3′ 和 5′ 单细胞 RNA 测序；典型每通道加载约 8,000 个活细胞。5′ 数据与富集 TCRαβ 文库结合。Smart-seq2 用于更深的全长转录本验证，每细胞约 100–200 万 reads。

### 2.2 空间与计算分析

作者使用 smFISH/RNAscope 对选定 marker 的胸腺定位进行验证；通过聚类、差异表达、轨迹/邻接关系、TCR 重排和跨物种比较解析发育。这里没有连续活体追踪，因此“轨迹”是横断面数据推断，不是直接观察的细胞命运。

## 3. 数据规模与图谱组成

### 3.1 样本和细胞构成

| 层级 | 规模/内容 | 含义 |
|---|---:|---|
| 胎儿胸腺 | 15 份，7–17 PCW | 覆盖胸腺定植与早期谱系形成 |
| 出生后胸腺 | 9 份 | 通常来自儿童心胸手术剩余组织 |
| 发育期细胞 | 138,397 | 胎儿图谱主体 |
| 出生后细胞 | 117,504 | 用于发育持续性与年龄比较 |
| 总细胞 | 255,901 | 论文统一分析规模 |
| 细胞状态 | >50 | T 细胞发育状态加微环境细胞 |

主要 T 细胞发育框架包括：

- 早期胸腺前体和双阴性阶段；
- 增殖与双阳性 CD4+CD8+ 阶段；
- TCRαβ entry/选择状态；
- CD4 单阳性和 CD8 单阳性分化；
- 调节性 T 细胞与增殖分支；
- γδ T 细胞；
- GNG4+ CD8αα T(I)、ZNF683+ CD8αα T(II) 等非常规分支；
- EOMES 相关 NKT-like 状态。

非 T 区室包括胸腺上皮细胞、成纤维细胞、内皮、髓系和 B 细胞等，它们用于构建配体—受体和空间微环境模型。

### 3.2 数据层具体有什么

1. 10x 基因表达矩阵：用于状态发现和定量；
2. TCRαβ 序列：用于检查 V/J 使用、重排时序和选择；
3. Smart-seq2 深度表达：对部分细胞提供更完整转录本；
4. 逐细胞元数据：样本、发育阶段、技术、聚类与精细注释；
5. smFISH/RNAscope 图像：选定 marker 的空间验证；
6. 跨组织/跨物种参照：用于判断胸腺状态与外周 T 细胞、鼠胸腺的对应关系。

不是每个 25.59 万细胞都同时具有 10x GEX、成对 TCR 和空间坐标。下载后必须按 assay 与 VDJ 字段重新统计可用于某项分析的细胞数。

### 3.3 HCA 项目页面

[HCA 项目页](https://explore.data.humancellatlas.org/projects/c1810dbc-16d2-45c3-b45e-3e675f88d87b) 当前标注：

- 24 名 donors；
- 约 25 万 cells；
- 10x 3′ v2 为主要平台之一；
- accession：E-MTAB-8581、ERP119282、PRJEB36131；
- 文件计数：206 个 fastq.gz、28 个 BAM、30 个 loom、1 个 h5ad、2 个 zip；
- 许可：CC BY 4.0；
- 项目页面更新于 2025-07-07。

HCA 页面适合按文件类型浏览和程序化下载。文件计数是当前门户的导出组织，不一定等于论文发表时作者包的文件数。

### 3.4 ArrayExpress/ENA 原始数据

[E-MTAB-8581](https://www.ebi.ac.uk/biostudies/arrayexpress/studies/E-MTAB-8581) 是原始测序入口。当前 SDRF/ENA 记录可概括为：

| 元数据层 | 当前数量 |
|---|---:|
| source records | 74 |
| biological individuals | 24 |
| experiments/runs | 103 |
| FASTQ | 206 |
| 10x V2 library runs | 93 |
| 10x V3 library runs | 10 |

当前 submitted FASTQ/原始文件约 2,684 GiB，即约 2.62 TiB。74 个 source records 是样本/技术记录，不是供者数；103 个 runs 也不是 103 个生物重复。

下载元数据：

~~~bash
wget -O E-MTAB-8581.sdrf.txt \
  "https://www.ebi.ac.uk/biostudies/files/E-MTAB-8581/E-MTAB-8581.sdrf.txt"
~~~

由 SDRF 中的 run accession 在 ENA 下载 FASTQ。重跑 Cell Ranger 时应按库版本和 GEX/VDJ 文库关系组织 manifest。

### 3.5 论文指定的 Zenodo 版本

[Zenodo 3572422](https://doi.org/10.5281/zenodo.3572422) 是论文 Data availability 指向的固定版本，当前文件为：

| 文件 | 字节数 | 约合 | 内容 |
|---|---:|---:|---|
| thymus_annotated_matrix_files.zip | 2,471,545,253 | 2,357.1 MB | 注释后的表达矩阵 |
| thymus_code_package.zip | 137,053,938 | 130.7 MB | 分析代码 |
| sample_metadata.xlsx | 19,631 | 19 KB | 样本元数据 |

总计约 2,487.8 MB，即约 2.43 GiB。这是严格复现论文时应优先保存的版本。

### 3.6 当前持续更新版本

[concept DOI 3572421](https://doi.org/10.5281/zenodo.3572421) 当前解析到更新记录，并在上述文件基础上增加：

| 文件 | 字节数 | 约合 |
|---|---:|---:|
| thymus_vdj.zip | 493,293,847 | 470.4 MB |
| HTA07.A01.v02.entire_data_raw_count.h5ad | 1,571,941,873 | 1,499.1 MB |
| sample_metadata_fix.xlsx | 23,987 | 23 KB |

加上注释矩阵与代码后，当前总量约 4,457.3 MB，即约 4.35 GiB。持续更新版本更方便复用 VDJ 和 raw counts，但不能在复现记录中只写 concept DOI；应同时记录它实际解析到的 record ID、文件名、字节数和日期。

### 3.7 三种下载路线

路线 A，快速复用图谱：

- 下载 thymus_annotated_matrix_files.zip；
- 下载 sample_metadata；
- 先查看现成注释和状态构成。

路线 B，研究 TCR 重排：

- 在当前 concept 版本下载 thymus_vdj.zip；
- 同时下载注释矩阵，以细胞 barcode 和 sample ID 对齐；
- 核验双链、productive、multiple chains 和克隆定义。

路线 C，完全重处理：

- 从 E-MTAB-8581/ENA 下载 206 个 FASTQ；
- 按 10x chemistry、样本和 GEX/VDJ 配对重跑；
- 预计需数 TiB 存储和较大计算资源。

### 3.8 下载后首先核查什么

~~~python
import scanpy as sc

adata = sc.read_h5ad("HTA07.A01.v02.entire_data_raw_count.h5ad", backed="r")
print(adata)
print(adata.obs.columns.tolist())
print(adata.obs["cell_type"].value_counts() if "cell_type" in adata.obs else "")
~~~

重点核查：

- 该 h5ad 是全 255,901 细胞还是某一整合对象；
- raw counts 位于 X、raw 还是 layers；
- PCW、postnatal age、donor、sample 和 chemistry 字段；
- VDJ zip 的 barcode 是否需要加 sample 前缀；
- 胎儿与出生后标签是否在相同层级。

## 4. 主要生物学发现

### 4.1 常规 αβ T 细胞发育

图谱细化了从早期前体、双阴性、双阳性、TCR entry 到 CD4/CD8 单阳性的连续状态，并显示正选择前后细胞周期、代谢和 TCR 信号模块的切换。

### 4.2 非常规 T 细胞

论文定义 GNG4+ CD8αα T(I) 与 ZNF683+ CD8αα T(II) 等非常规状态，提示它们并非简单的常规 CD8 T 末端亚群，而具有不同发育窗口和分子程序。

### 4.3 TCR repertoire formation

TCR 序列使作者能够把转录状态与 V(D)J 重排、克隆选择和谱系分化对应。这里讨论的是 repertoire 形成规律，不等于确定每条 TCR 的抗原。

### 4.4 胸腺微环境

上皮、成纤维、内皮和髓系细胞表达不同趋化因子、细胞因子和选择相关配体，构成阶段特异的发育生态位。

## 5. 关键图表怎么读

- 全图谱 UMAP：看状态范围，不能将二维距离直接解释为发育时间。
- 发育轨迹：支持最可能的状态邻接，但不能替代条形码谱系追踪。
- TCR V/J 图：反映重排和选择偏好，不直接给出抗原特异性。
- smFISH 图：验证少数状态在胸腺区域的定位，不是全图谱空间坐标。
- 配体—受体网络：是表达兼容性假设，需功能实验验证。

## 6. 创新点

1. 把胎儿和出生后人胸腺置于统一单细胞框架。
2. 同时覆盖淋巴发育与基质微环境。
3. 将 TCR repertoire 形成与转录状态连接。
4. 提供代码、注释矩阵、原始数据和后续 VDJ 包，多层次复用性强。

## 7. 局限性

1. 横断面样本不能直接观察单个细胞的发育命运。
2. 胎儿与出生后样本来源、处理和平台可能混杂。
3. 稀有状态的供者覆盖有限。
4. TCR 克隆/序列不能直接说明抗原和选择强度。
5. 原位实验是靶向验证，不是全转录组空间组学。
6. 当前 concept DOI 文件集已变化，复现需锁定版本。

## 8. 对本综述的作用

这是“导航 T 细胞状态”的生理范本：胸腺通过按顺序提供 TCR 信号、细胞因子、趋化和空间生态位，把前体导航为多条成熟谱系。它可用于提出细胞治疗设计问题：

- 是否能在体外复现阶段化而非一次性刺激；
- 是否能以胸腺状态图谱定义更精确的产品质量属性；
- 是否能通过人工生态位控制谱系承诺和成熟度。

## 9. 可直接写进综述的表述

> 人胸腺单细胞图谱以 24 份胎儿和出生后样本、255,901 个细胞及配对 TCR 为基础，展示了由前体到常规和非常规 T 细胞的分阶段状态转换，并揭示胸腺基质生态位可能提供的顺序化导航信号。

## 10. 最容易误读的地方

- 24 是生物供者/样本数，74 是数据库 source records。
- 25.59 万是全部胸腺细胞，不是全部为 T 细胞。
- 并非每个细胞都有配对 TCR。
- 轨迹不是活细胞谱系追踪。
- smFISH 不等于空间转录组。
- 论文固定 Zenodo 版本与当前 concept 版本的文件组成不同。
