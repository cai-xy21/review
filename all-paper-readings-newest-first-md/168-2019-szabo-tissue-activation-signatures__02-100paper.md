# 《Single-cell transcriptomics of human T cells reveals tissue and activation signatures in health and disease》精读

## 论文信息

- 作者：Peter A. Szabo、Hanna Mendes Levitin、Michelle Miron、Donna L. Farber、Peter A. Sims 等
- 期刊：Nature Communications
- 年份：2019；10: 4706
- DOI：10.1038/s41467-019-12464-3
- 原文：[Nature Communications](https://www.nature.com/articles/s41467-019-12464-3)
- PubMed：[PMID 31624246](https://pubmed.ncbi.nlm.nih.gov/31624246/)
- 全文：[PMC6797728](https://pmc.ncbi.nlm.nih.gov/articles/PMC6797728/)
- 数据：[GEO GSE126030](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE126030)
- HCA 项目页：[Human T cells from blood and tissues](https://explore.data.humancellatlas.org/projects/4a95101c-9ffc-4f30-a809-f04518a23803)

## 一句话结论

作者比较 4 名供者的血液、肺、淋巴结和骨髓 T 细胞在静息及抗 CD3/CD28 激活 16 小时后的 51,876 个单细胞转录组，揭示组织驻留程序与激活响应既有跨组织保守模块，也有 CD4/CD8 和组织特异差异，并将这些模块投射到多种肿瘤 T 细胞数据中。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 供者 | 4 | 2 名器官供者 + 2 名健康血液供者 |
| 组织 | 血、肺、肺引流淋巴结、骨髓 | 肺/LN/BM 来自两名器官供者，血液供者构成不同 |
| 条件 | 静息、抗 CD3/CD28 16 h | 是强体外刺激，不等于体内抗原刺激 |
| 文库 | 16 个 10x 3′ v2 样本 | 2×3×2 个组织条件文库 + 2×2 个血液文库 |
| 细胞 | 51,876 | HCA 页面约写 50k；精确处理后总数为 51,876 |
| 分子层 | scRNA-seq | 无 scTCR、ATAC 或空间组学 |
| GEO 处理后包 | GSE126030_RAW.tar，111.4 MB | 内含 16 个样本的矩阵文本文件 |
| SRA 原始数据 | 16 runs，SRP183443 | 当前归档容器合计约 293 GB |

## 1. 研究要解决的问题

研究聚焦两个状态轴：

1. 组织定位如何塑造健康人 T 细胞的静息状态；
2. 不同组织中的 T 细胞对统一 TCR/CD28 刺激如何响应。

作者进一步问：从健康组织提炼出的状态模块，能否解释癌症样本中的 T 细胞异质性。

## 2. 方法框架

### 2.1 配对组织与激活实验

两名死亡器官供者各提供肺、肺引流淋巴结和骨髓；另两名健康活体供者提供外周血。每份样本均分成：

- ex vivo 静息；
- 抗 CD3/CD28 刺激 16 小时。

由此形成 16 个 10x 3′ v2 文库：

- 器官供者：2 人 × 3 组织 × 2 条件 = 12；
- 血液供者：2 人 × 1 组织 × 2 条件 = 4。

### 2.2 计算分析

作者分别分析 CD4 和 CD8 T 细胞，用 scHPF 等方法识别可重复表达模块，并将健康组织定义的模块投射到非小细胞肺癌、结直肠癌、乳腺癌和黑色素瘤公开单细胞数据中。

肿瘤队列是外部复用数据，不能并入核心实验的 51,876 个细胞或 4 名供者。

## 3. 数据规模与图谱组成

### 3.1 精确细胞与组织构成

处理后数据共 51,876 个细胞。按组织汇总为：

| 组织 | 细胞数 | 占比 |
|---|---:|---:|
| 外周血 | 17,625 | 34.0% |
| 肺引流淋巴结 | 16,527 | 31.9% |
| 肺 | 11,059 | 21.3% |
| 骨髓 | 6,665 | 12.8% |
| 合计 | 51,876 | 100% |

这些数包含静息和激活条件。真正的独立生物重复是 4 名供者；肺、淋巴结和骨髓只有 2 名供者，因此组织差异的外推范围有限。

### 3.2 数据层与注释

核心公开数据包含：

1. 16 个样本的基因表达矩阵；
2. barcode 和 gene/feature 文件；
3. 细胞级组织、刺激条件和 CD4/CD8 注释；
4. UMAP 坐标与论文 source data；
5. 由 scHPF 提取的静息和激活表达模块。

论文没有配对 TCR，所以不能从该数据判断克隆扩增、克隆共享或抗原特异性。也没有 CITE-seq、ATAC、活细胞追踪或空间坐标。

### 3.3 GEO GSE126030 页面

[GSE126030](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE126030) 是最直接的数据入口：

- 16 个 GSM 样本：GSM3589406–GSM3589421；
- BioProject：PRJNA520832；
- SRA study：SRP183443；
- 处理后包：GSE126030_RAW.tar，约 111.4 MB；
- 原始 reads：通过 SRA 下载。

RAW.tar 内为逐样本压缩文本矩阵。下载：

~~~bash
wget -c \
  https://ftp.ncbi.nlm.nih.gov/geo/series/GSE126nnn/GSE126030/suppl/GSE126030_RAW.tar
~~~

解包后应先按 GSM 与 GEO 样本表对应 donor、tissue 和 stimulation，不能仅凭文件排序推断。

### 3.4 HCA 项目页面

[HCA 项目页](https://explore.data.humancellatlas.org/projects/4a95101c-9ffc-4f30-a809-f04518a23803) 当前显示：

- 4 donors；
- 约 50.0k cells；
- 10x 3′ v2；
- 4 anatomical entities；
- 文件计数：16 BAM、32 fastq.gz、20 loom、4 CSV、1 tar、1 xlsx。

HCA 的 50k 是项目页的近似值，报告精确分析规模时使用处理后对象的 51,876。

### 3.5 SRA 原始数据

SRP183443 当前含 16 个 runs，与 16 个文库一一对应。当前 SRA 归档容器合计约 293 GB。原始数据适用于重新比对、环境 RNA/双细胞重评估和统一流程复现。

可生成 run table：

~~~bash
curl -L \
  "https://trace.ncbi.nlm.nih.gov/Traces/sra-db-be/runinfo?acc=SRP183443" \
  -o SRP183443_runinfo.csv
~~~

然后用 SRA Toolkit：

~~~bash
prefetch SRR_ACCESSION
fasterq-dump --split-files --threads 8 SRR_ACCESSION
~~~

fasterq-dump 的解包空间通常明显大于 SRA 容器，下载全部数据前至少预留约 0.5–1 TB 工作空间。

### 3.6 论文 Source Data

Nature 页面提供 Source Data Excel。主要表格包含：

- 细胞 barcode；
- UMAP 坐标；
- tissue 与 stimulation；
- CD4/CD8 和 CCL5 等分层；
- 模块权重及肿瘤映射结果。

Source Data 适合复图和核对分组，但不是完整 counts 的替代品。做差异表达应回到 GEO/HCA 表达矩阵，并以 donor 为重复单位。

### 3.7 下载后重建对象

GEO RAW.tar 多为 Matrix Market 或稀疏文本组成，建议逐 GSM 建 AnnData，再添加样本表：

~~~python
import scanpy as sc

adata = sc.read_10x_mtx(
    "GSM_sample_directory",
    var_names="gene_symbols",
    cache=True
)
adata.obs["sample"] = "GSM..."
adata.obs["donor"] = "..."
adata.obs["tissue"] = "..."
adata.obs["stimulation"] = "resting"
~~~

合并前核对 barcode 是否已带样本前缀；差异分析使用 pseudobulk 或 donor-level 模型，避免把数万细胞误当成数万独立重复。

## 4. 主要状态与模块

### 4.1 静息组织状态

CD4 T 细胞覆盖 naive/central-memory、Treg、组织驻留和效应相关状态；CD8 T 细胞主要包括 TEM/TRM、激活型 TEM/TRM 和 TEMRA 等。肺富集组织驻留与效应模块，淋巴结更偏初始/中央记忆。

### 4.2 激活响应

统一抗 CD3/CD28 刺激揭示跨组织保守的即时激活模块，也显示组织来源和 CD4/CD8 谱系会改变响应强度与组成。16 小时主要捕获早期转录响应，不能代表长期分化、耗竭或记忆形成。

### 4.3 七类保守模块

作者提炼出若干静息、激活和功能模块，并将其用于解释疾病数据。模块比离散标签更适合描述连续 T 细胞状态，但模块分数仍受平台、基因覆盖和归一化影响。

## 5. 肿瘤外部验证应如何理解

作者查询了 NSCLC、结直肠癌、乳腺癌和黑色素瘤单细胞数据，发现健康组织的驻留/效应与激活模块可在肿瘤 T 细胞中复现。

这支持跨场景状态程序的复用，但不表示：

- 这些肿瘤细胞是本研究新测；
- 肿瘤状态与健康组织状态完全相同；
- 模块相似可证明相同谱系来源；
- 没有 TCR 数据也能断言克隆转换。

## 6. 关键图表怎么读

- 组织 UMAP：看相对状态分布，供者仅 2–4 人。
- 静息/激活比较：16 h 强刺激给出响应潜能，不是体内状态转移速率。
- scHPF modules：更接近连续功能轴，但仍是数据驱动分解。
- 肿瘤投射：是参考模块的外部适用性测试，不是联合队列统计。

## 7. 创新点

1. 同时比较组织来源与统一激活扰动。
2. 以表达模块而非只靠 cluster 描述 T 细胞功能。
3. 将健康组织参考投射到多癌种。
4. 数据结构简单、公开程度高，适合作为激活状态基准。

## 8. 局限性

1. 只有 4 名供者，组织样本更只有 2 名器官供者。
2. 血液供者与组织供者不是同一批人。
3. 体外强刺激不能代表生理抗原、共刺激和组织因子的组合。
4. 单一 16 h 时间点无法描述状态转移动态。
5. 无 TCR、蛋白、染色质和空间数据。
6. 组织消化可能影响即时早期与应激基因。

## 9. 对本综述的作用

论文适合放在“quantitatively characterizing cell phenotypes, functions and molecular markers”和“link cell state/function transitions with molecular drivers”之间：

- 它提供可量化的静息与激活表达模块；
- 统一刺激构成一个简单扰动实验，可区分基线状态与响应潜能；
- 它也说明单一终点 scRNA 不足以建立真正的动力学导航系统，后续需要时间序列、活细胞追踪和因果扰动。

## 10. 可直接写进综述的表述

> 对 51,876 个健康人血液和组织 T 细胞的配对静息—激活分析表明，组织驻留程序与 TCR/CD28 响应由跨组织保守模块和局部特异模块共同组成；这些模块在多癌种 T 细胞中仍可识别，可作为量化状态和响应潜能的参考坐标。

## 11. 最容易误读的地方

- 51,876 是核心健康组织实验细胞，不包括外部肿瘤数据。
- 4 名供者中只有 2 名贡献肺、LN 和骨髓。
- 16 个文库不是 16 名供者。
- 抗 CD3/CD28 16 h 是体外强刺激。
- 本数据没有 TCR、ATAC 或空间组学。
