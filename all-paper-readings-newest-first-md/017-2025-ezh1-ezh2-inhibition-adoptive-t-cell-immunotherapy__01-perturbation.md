# 《EZH1/EZH2 inhibition enhances adoptive T cell immunotherapy against multiple cancer models》精读

## 论文信息

- 作者：Patrizia Porazzi、Siena Nason、Ziqi Yang 等
- 期刊：*Cancer Cell*
- 年份：2025；43(3): 537–551.e7；在线发表于 2025 年 2 月 20 日
- DOI：[10.1016/j.ccell.2025.01.013](https://doi.org/10.1016/j.ccell.2025.01.013)
- PubMed：[PMID 39983725](https://pubmed.ncbi.nlm.nih.gov/39983725/)
- 全文：[PubMed Central](https://pmc.ncbi.nlm.nih.gov/articles/PMC13312562/)
- 公共数据：NCBI GEO [GSE265799](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE265799)、[GSE267074](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE267074)、[GSE266249](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE266249)、[GSE285897](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE285897)
- 原始文献文件：`perturbation_33references.zip` 中编号 1 的 PDF

## 一句话结论

在多种人源血液瘤和实体瘤模型中，先用 tazemetostat 抑制肿瘤细胞 EZH2，或用 valemetostat 同时抑制 EZH1/EZH2，可通过增强肿瘤免疫原性、黏附和炎症反应，提高 CAR-T/TCR-T 的识别、浸润、扩增与持续杀伤；现有证据主要来自体外实验和免疫缺陷小鼠异种移植模型，尚不能直接等同于患者疗效。

## 数据护照（先看这一表）

| GEO | 数据层 | 生物材料与比较 | 公共处理后数据 | 关键规模 | 最直接用途 |
|---|---|---|---|---:|---|
| `GSE265799` | 肿瘤内 CAR-T scRNA-seq | OCI-Ly18 异种移植瘤；vehicle + CART19 对比 tazemetostat + CART19；CAR-T 输注后第 11 天 | 2 个 10x `.h5` 表达矩阵 | 36,601 features；827 vs 775 cells，共 1,602 cells | 分析肿瘤浸润 CAR-T 的细胞毒、激活和耗竭程序 |
| `GSE266249` | 外周血 CAR-T scRNA-seq | 同类异种移植模型；CAR-T 输注后第 20 天 | 2 套 10x MTX/TSV 矩阵 | 36,604 features；693 vs 1,197 cells，共 1,890 cells | 分析循环 CAR-T 的初始/早期记忆、效应与终末分化状态 |
| `GSE267074` | 分选肿瘤细胞 bulk RNA-seq | OCI-Ly18-GFP⁺ 肿瘤细胞；CART19 对比 CART19 + tazemetostat | 1 个基因计数表 | 58,735 rows；18 count columns；每组 3 只鼠 × 每鼠 3 个重复 | 分析肿瘤细胞免疫原性、黏附、IFN 和凋亡通路 |
| `GSE285897` | 肿瘤细胞 ATAC-seq | SU-DHL-4-GFP⁺；DMSO/TAZ × UTD/CART19，四组各 3 个重复 | 12 个 `.bedgraph.gz` | 12 libraries；处理文件总归档约 18.4 MB | 分析 EZH2 抑制和 CAR-T 接触后的染色质开放变化 |

**最重要的数据提醒：**两个 scRNA-seq GEO 系列都写明实验来源为 3 对 3 只小鼠，但公共处理后文件只有每个处理组一个表达矩阵，没有逐鼠矩阵或可将每个细胞回溯到单只小鼠的公开字段。因此不能把 1,602 或 1,890 个细胞当作独立生物学重复直接做组间显著性检验；公开版本更适合描述状态构成和生成假设。

## 1. 研究要解决的问题

CAR-T 和其他过继性细胞治疗在血液肿瘤中已取得成功，但抗原逃逸、肿瘤细胞免疫原性不足、T 细胞浸润差及持续性不足仍导致大量治疗失败；在实体瘤中这些障碍更加突出。

EZH2 是 PRC2 的催化亚基，通过 H3K27me3 维持转录抑制。多种肿瘤依赖 EZH2，且 EZH2 活性可压低 MHC、趋化因子、共刺激和炎症相关基因。作者因此提出：

1. 与其只改造 T 细胞，是否可以先对肿瘤进行表观遗传重编程；
2. EZH2 抑制是否能使肿瘤更容易被 CAR-T/TCR-T 识别和杀伤；
3. 同时抑制可代偿 EZH2 的 EZH1，能否进一步增强效果；
4. 该策略是否可跨越不同靶点、不同 ACT 产品及血液瘤/实体瘤模型。

## 2. 实验与机制框架

### 2.1 干预对象和时序

论文主要使用两种已进入临床应用的表观遗传药物：

- **tazemetostat**：选择性 EZH2 抑制剂；
- **valemetostat**：EZH1/EZH2 双重抑制剂。

效果最稳定的给药顺序是先让肿瘤细胞暴露于抑制剂约 3 天，再加入 CAR-T/TCR-T，并在共培养期间维持药物。这个时序说明研究的主要逻辑是“先改变靶细胞，再改善 T 细胞反应”，不是简单把 EZH2 当作 CAR-T 内源性耗竭靶点。

### 2.2 肿瘤、ACT 与模型覆盖

| 肿瘤类型 | 代表细胞/模型 | ACT 靶点 |
|---|---|---|
| 生发中心型 DLBCL | OCI-Ly18、Toledo、SU-DHL-4、Karpas-422 | CD19、CD22、CD79b CAR-T |
| 多发性骨髓瘤 | RPMI-8226；体外与骨内异种移植 | BCMA CAR-T |
| 急性髓系白血病 | KG-1、THP-1 | CD33 CAR-T |
| 卵巢癌、前列腺癌 | SKOV-3、PC3 | HER2 CAR-T |
| Ewing 肉瘤 | A673 | HLA-A*02:01 限制的 LOXHD1 TCR-T |

DLBCL 部分同时覆盖野生型和突变型 EZH2，提示效应并不只限于携带 EZH2 激活突变的肿瘤。

### 2.3 证据链

研究用多层实验把“药物—肿瘤重编程—T 细胞反应—肿瘤控制”串起来：

1. H3K27me3、RNA-seq 和 ATAC-seq：验证表观遗传与转录重编程；
2. 流式、ELISA、长时程杀伤及重复挑战：评估 T 细胞激活、细胞因子、扩增和连续杀伤；
3. z-Movi：测量 CAR-T 与肿瘤细胞的整体黏附/结合 avidity；
4. 肿瘤内和外周血 scRNA-seq：观察 CAR-T 状态；
5. NSG 异种移植：验证肿瘤控制、T 细胞扩增和生存获益。

## 3. 公共数据到底有什么

以下内容依据论文 Data and code availability、GEO Series/Sample 页面以及实际下载后的文件结构核对；页面状态核对时间为 2026 年 8 月 22 日。

### 3.1 `GSE265799`：肿瘤浸润 CAR-T 单细胞转录组

实验为 OCI-Ly18 人源 DLBCL 在 NSG 小鼠中的皮下异种移植。肿瘤接受 vehicle 或 tazemetostat 预处理，输注 CART19 后第 11 天，从肿瘤单细胞悬液中分选人源 CAR-T，使用 10x Chromium Next GEM Single Cell 5′ 建库和 Illumina NovaSeq 6000 测序。

- GEO 总体设计写明 **3 vs 3 animals**；
- GEO 有 4 个 library 条目：每组各 1 个 GEX 与 1 个 Feature Barcode library；
- 公共处理后文件只有两个 GEX `.h5`：
  - vehicle：`GSM8229410_PP_CART19_TILs_filtered_feature_bc_matrix.h5`，36,601 × 827；
  - tazemetostat：`GSM8229412_PP_CART19_TAZ_TILs_filtered_feature_bc_matrix.h5`，36,601 × 775；
- `GSE265799_RAW.tar` 实际约 11.26 MB（10.74 MiB）；
- 原始测序记录可由 SRA/BioProject `PRJNA1104228` 获取。

论文对该数据过滤时使用：`200 < nFeature_RNA < 5500`、线粒体比例 `<10%`，并以 `MS4A1 < 1`、`CD2 > 1` 去除残余肿瘤细胞；聚类使用 Seurat，并受预定义经典 T 细胞基因列表驱动。

**可回答：**药物组肿瘤内 CAR-T 是否增强 `GZMB`、`PRF1`、`IFNG`、`CD69` 等激活/细胞毒程序，是否降低部分耗竭相关信号，以及七个转录簇的相对组成。

**不能稳健回答：**小鼠间变异、个体层面的差异表达、TCR 克隆扩增和谱系关系。GEO 未提供逐鼠处理后矩阵，也未在公开文件清单中提供 V(D)J/TCR 数据。

### 3.2 `GSE266249`：外周血 CAR-T 单细胞转录组

该系列取 CAR-T 输注后第 20 天外周血，分选人源 T 细胞，比较 CART19 + vehicle 与 CART19 + tazemetostat；同样使用 10x 5′ 和 NovaSeq 6000。

- GEO 总体设计同样写明 **3 vs 3 animals**；
- 4 个 library 条目由每组 GEX 和 Feature Barcode 各一个组成；
- 公共处理后 GEX 数据为两套 10x Matrix Market 文件：
  - vehicle：36,604 features × 693 cells，2,109,560 个非零计数；
  - tazemetostat：36,604 features × 1,197 cells，3,456,286 个非零计数；
- `GSE266249_RAW.tar` 约 21.02 MB（20.05 MiB）；
- 原始测序记录对应 BioProject `PRJNA1106444`。

论文报告两组均形成七个簇；tazemetostat 组更富集激活的初始/早期记忆、效应、抗凋亡和糖酵解相关程序，vehicle 组部分簇更偏终末效应和 TGF-β/Smad3 程序。

**解读提醒：**处理后矩阵中药物组细胞更多（1,197 vs 693）与论文所述体内扩增一致，但捕获细胞数还受分选、上机和质控影响，不能单凭矩阵细胞数计算真实体内扩增倍数；真实扩增证据应结合流式计数。

### 3.3 `GSE267074`：分选肿瘤细胞 bulk RNA-seq

这是机制链中最直接反映“肿瘤被重编程”的数据。OCI-Ly18-GFP⁺ 细胞皮下接种后接受 tazemetostat 或 vehicle，随后输注 CART19；第 11 天切取肿瘤并分选 GFP⁺ 肿瘤细胞做 bulk RNA-seq。

- 比较：CART19 + vehicle 对 CART19 + tazemetostat；
- 3 只小鼠/组，每只小鼠 3 个测序/样本重复，共 18 个 GSM；
- 处理后文件 `GSE267074_Bulk_Dissected_Tumors_gene_count_excel.txt.gz` 约 2.7 MB；
- 表中有 58,735 个基因行、18 个原始 count 列和 gene name/染色体/位置/biotype 等 9 个注释列；
- vehicle：M_766、M_769、M_770，各 A/B/C；
- tazemetostat：M_767、M_768、M_774，各 A/B/C；
- 原始 reads 对应 BioProject `PRJNA1109533`。

方法为 NovaSeq 6000、100 bp single-end；Trimmomatic 0.36 去接头和低质量碱基，STAR 2.6.0c 比对到 hg19，featureCounts 1.6.1 计数，DESeq2 1.22.2 做差异表达，GSEA 4.1.0 + MSigDB Hallmark 做通路分析。

论文据此报告 tazemetostat 组合组上调 MHC-I/II、趋化/共刺激、黏附、B 细胞激活、IFN 应答和 p53/凋亡相关基因，例如 `HLA-A`、`HLA-DOA/B`、`CXCL9/10`、`CD40`、`ICAM1`、`FOS/JUN`、`STAT1-3`、`IRF2/7/9`、`FAS` 和多个 caspase。

**统计提醒：**A/B/C 是否构成真正独立的生物样本，应以 GSM 元数据和实验记录为准；严谨复分析宜把 mouse ID 作为生物学重复层级，避免把同一只鼠的三个重复当成 3 只独立动物。

### 3.4 `GSE285897`：肿瘤细胞 ATAC-seq

SU-DHL-4-GFP⁺ DLBCL 细胞先用 tazemetostat 或 DMSO 处理，再与 UTD 或 CART19 共培养，之后分选肿瘤细胞做 ATAC-seq。形成 2 × 2 设计：

1. DMSO + UTD；
2. tazemetostat + UTD；
3. DMSO + CART19；
4. tazemetostat + CART19。

每组 3 个重复，共 12 个样本。每个文库从 50,000 个分选细胞核开始，使用 Active Motif ATAC-Seq kit 和 Tn5 转座；NextSeq 2000 产生 75 bp paired-end reads。

- 处理后归档 `GSE285897_RAW.tar` 为 12 个 `.bedgraph.gz`，约 18.38 MB；
- 每个 bedGraph 对应一个 GSM/重复，单文件约 1.31–1.73 MB；
- 原始 FASTQ 可由 SRA/BioProject `PRJNA1206723` 获取。

bedGraph 是连续基因组信号，不是原始 fragments 或 peak-by-cell 矩阵。它适合浏览或重新汇总区域信号，但若要严格重做质控、去重复、峰调用、差异可及性和 footprint，应该下载 SRA 原始 reads。

### 3.5 什么没有公开

GEO 覆盖的是四套测序实验，不等于论文全部原始数据：

- 体外荧光素酶/阻抗/Incucyte 杀伤曲线；
- 流式细胞术 FCS 原始文件及 gating workspace；
- ELISA/Luminex 原始读数；
- 活体生物发光、肿瘤体积、个体生存和体重的完整机器可读表；
- z-Movi avidity 的逐细胞原始数据；
- IHC 全切片图像；
- scRNA-seq 的逐鼠处理后对象、Seurat 对象、簇标签/UMAP 坐标和明确的细胞级 mouse ID；
- 可直接运行的公开分析代码仓库。

论文 Data and code availability 只给出四个 GEO accession，并未声明公共 GitHub/Zenodo 代码。新生成的细胞系需联系通讯作者并完成 MTA；这与自由下载数据不同。

## 4. 如何下载数据

### 4.1 路线 A：只下载处理后数据，最快复分析

在 GEO 页面底部点击 Supplementary file，或在 PowerShell 中执行：

```powershell
Invoke-WebRequest `
  -Uri "https://ftp.ncbi.nlm.nih.gov/geo/series/GSE265nnn/GSE265799/suppl/GSE265799_RAW.tar" `
  -OutFile "GSE265799_RAW.tar"

Invoke-WebRequest `
  -Uri "https://ftp.ncbi.nlm.nih.gov/geo/series/GSE266nnn/GSE266249/suppl/GSE266249_RAW.tar" `
  -OutFile "GSE266249_RAW.tar"

Invoke-WebRequest `
  -Uri "https://ftp.ncbi.nlm.nih.gov/geo/series/GSE267nnn/GSE267074/suppl/GSE267074_Bulk_Dissected_Tumors_gene_count_excel.txt.gz" `
  -OutFile "GSE267074_Bulk_Dissected_Tumors_gene_count_excel.txt.gz"

Invoke-WebRequest `
  -Uri "https://ftp.ncbi.nlm.nih.gov/geo/series/GSE285nnn/GSE285897/suppl/GSE285897_RAW.tar" `
  -OutFile "GSE285897_RAW.tar"
```

解包 TAR：

```powershell
tar -xf GSE265799_RAW.tar
tar -xf GSE266249_RAW.tar
tar -xf GSE285897_RAW.tar
```

### 4.2 路线 B：读取处理后表达矩阵

肿瘤内 scRNA-seq 的 10x HDF5：

```python
import scanpy as sc

til_vehicle = sc.read_10x_h5(
    "GSM8229410_PP_CART19_TILs_filtered_feature_bc_matrix.h5"
)
til_taz = sc.read_10x_h5(
    "GSM8229412_PP_CART19_TAZ_TILs_filtered_feature_bc_matrix.h5"
)
print(til_vehicle.shape, til_taz.shape)
```

外周血 scRNA-seq 的 10x MTX/TSV：

```python
import scanpy as sc

pb_vehicle = sc.read_10x_mtx(
    ".", prefix="GSM8243328_car19_", var_names="gene_symbols"
)
pb_taz = sc.read_10x_mtx(
    ".", prefix="GSM8243330_tazcar19_", var_names="gene_symbols"
)
print(pb_vehicle.shape, pb_taz.shape)
```

bulk RNA-seq 计数表：

```python
import pandas as pd

counts = pd.read_csv(
    "GSE267074_Bulk_Dissected_Tumors_gene_count_excel.txt.gz",
    sep="\t",
    compression="gzip"
)
print(counts.shape)
print(counts.columns.tolist())
```

ATAC bedGraph：

```python
import pandas as pd

signal = pd.read_csv(
    "GSM8712290_1ATAC.bedgraph.gz",
    sep="\t",
    header=None,
    names=["chrom", "start", "end", "signal"]
)
```

### 4.3 路线 C：下载原始 FASTQ

每个 GEO 页面点击 **SRA Run Selector**，或通过以下 BioProject 进入：

- `GSE265799` → [PRJNA1104228](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA1104228)
- `GSE266249` → [PRJNA1106444](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA1106444)
- `GSE267074` → [PRJNA1109533](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA1109533)
- `GSE285897` → [PRJNA1206723](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA1206723)

在 Run Selector 中导出 `SraAccList.txt`，安装 NCBI SRA Toolkit 后：

```powershell
prefetch --option-file SraAccList.txt
Get-Content SraAccList.txt | ForEach-Object {
  fasterq-dump $_ --split-files --threads 8 --outdir fastq
}
```

原始数据适合严格重跑 Cell Ranger/STAR/ATAC 流程。`fasterq-dump` 的临时和 FASTQ 空间通常显著大于 SRA 下载体积，开始前应预留数倍磁盘空间。

### 4.4 推荐下载组合

- **只看 T 细胞状态：**先下 `GSE265799_RAW.tar` 和 `GSE266249_RAW.tar`，总处理后下载量约 32 MB；
- **只验证肿瘤重编程：**下 `GSE267074` 计数表，约 2.7 MB；
- **研究染色质机制：**先用 `GSE285897` bedGraph 浏览，再按需下 SRA reads；
- **严格复现论文：**四个系列全部下载，并在统计模型中保留 mouse/replicate 层级；同时需要向作者索取未公开功能实验原始数据与代码。

## 5. 核心生物学结果

### 5.1 肿瘤预处理是主要作用环节

tazemetostat 预处理多种 DLBCL 细胞后，CART19 杀伤增强；EZH2 shRNA 敲低得到相似结果。相反，只预处理 CAR-T、在制造期加入 tazemetostat，或直接在 CART19 中敲除 EZH2，并未复现同等增益。由此支持药物的首要作用对象是肿瘤细胞。

### 5.2 肿瘤被改造成更易被 T 细胞识别的状态

bulk RNA-seq、ATAC-seq、流式和 avidity 实验共同显示：EZH2 抑制增加抗原呈递、炎症/IFN、趋化、共刺激和黏附相关程序，同时提高 CAR-T 与肿瘤的整体结合强度。CD19 表达本身没有明显增加，因此疗效增强不是简单来自靶抗原上调。

### 5.3 CAR-T 状态随空间位置而不同

肿瘤内 CAR-T 在 tazemetostat 组偏细胞毒和激活；外周血则更富集初始/早期记忆、抗凋亡和代谢适应程序。论文据此提出，药物一方面促进肿瘤局部效应功能，另一方面有利于循环区室中较少分化状态的维持。

这里应避免把两个取样时间和区室直接拼成单向分化轨迹：肿瘤内为第 11 天，外周血为第 20 天，而且公共数据无法进行逐鼠或克隆追踪。

### 5.4 效应跨 ACT 产品与肿瘤类型

tazemetostat 增强了 CD19/CD22/CD79b CAR-T 对 DLBCL、BCMA CAR-T 对骨髓瘤、CD33 CAR-T 对 AML、HER2 CAR-T 对卵巢/前列腺癌，以及 LOXHD1 TCR-T 对 A673 肉瘤细胞的杀伤。证据说明策略具有跨模型潜力，但并不意味着所有患者肿瘤都会同样敏感。

### 5.5 双 EZH1/EZH2 抑制更强

valemetostat 在多种模型中降低 H3K27me3，并在部分直接比较中比 tazemetostat 更强地促进 CART19 或 BCMA CAR-T 的持续杀伤和扩增。在 RPMI-8226 骨内异种移植模型中，组合改善肿瘤控制、外周 T 细胞扩增和血清 IFN-γ。

## 6. 该文与 T 细胞状态扰动的关系

这篇文章不是典型的 T 细胞内在 CRISPR perturbation，而是**肿瘤侧表观遗传扰动**：先改变肿瘤细胞的可及性和转录程序，再间接推动 CAR-T 的状态变化。

可用以下因果链理解：

> EZH1/EZH2 抑制 → H3K27me3 降低与染色质开放变化 → 肿瘤抗原呈递/黏附/趋化/IFN 程序增强 → CAR-T 结合、浸润和激活增强 → 肿瘤内细胞毒状态与外周较少分化状态增加 → ACT 控瘤改善。

但这仍是多通路共同作用的模型。作者没有逐一敲除 `CXCL9/10`、`ICAM1`、MHC 或 OX40L 来证明哪个节点是必要且充分条件，不能把任一单基因上调写成唯一机制。

## 7. 数据复用建议

1. **scRNA-seq 重新注释：**统一处理两个系列，检查 CD4/CD8 比例、效应/细胞毒、记忆、增殖和应激程序；不要直接依赖原文七簇标签。
2. **pseudo-bulk 限制：**如果无法从原始 reads 或作者处恢复 mouse ID，就不要声称做了有生物学重复的 pseudo-bulk 差异表达。
3. **bulk RNA-seq 分层：**先按 mouse 汇总或在设计矩阵中嵌套 replicate，再比较治疗效应；检查同鼠 A/B/C 的相关性。
4. **RNA–ATAC 联合：**把 `GSE267074` 的差异基因与 `GSE285897` 的差异开放区域联系起来，但注意它们来自不同 DLBCL 细胞系和一个体内/一个体外系统。
5. **与扰动图谱对接：**把 EZH1/EZH2 抑制看作肿瘤侧 perturbation，分析其是否把肿瘤推向高 IFN、高抗原呈递、高黏附生态位，并与 T 细胞内在 perturbation 的效应区分。
6. **完整验证：**若目标是定量复现 Figure 1–4，需向作者索取未公开的流式、杀伤、成像、IHC、avidity 和动物级源数据。

## 8. 推荐图版

- **Figure 1**：tazemetostat 与 CART19 的给药顺序、DLBCL 杀伤和体内控瘤。适合讲研究逻辑。
- **Figure 2**：肿瘤转录重编程、IHC 浸润和 avidity。最适合讲“肿瘤侧机制”。
- **Figure 3**：肿瘤内/外周血 CAR-T scRNA-seq 与直接干预 CAR-T 的对照。最适合 T 细胞状态章节。
- **Figure 4**：跨肿瘤、跨 CAR/TCR 靶点以及 valemetostat 的广谱验证。适合讲可推广性。

如果只能选一张用于本章，选 **Figure 3**；如果强调数据与因果链，选 **Figure 2 + Figure 3**。

## 9. 创新价值

1. 将干预焦点从 T 细胞本身扩展到肿瘤侧表观遗传可塑性。
2. 用转录组、染色质、细胞结合、单细胞状态和体内疗效构成较完整证据链。
3. 在多个 CAR/TCR 靶点、血液瘤与实体瘤模型中验证方向一致性。
4. 比较选择性 EZH2 与双 EZH1/EZH2 抑制，提出补偿机制和增强策略。
5. 使用临床已有药物，为组合治疗转化提供较短路径。

## 10. 局限性

1. 主要体内证据来自 NSG 免疫缺陷小鼠，缺乏完整髓系、Treg 和细胞因子网络，不能代表真实人类 TME。
2. 论文没有患者随机对照疗效数据；提到的临床试验不能替代本研究的临床验证。
3. 多个实体瘤扩展主要是体外细胞系杀伤，证据强度低于 DLBCL 部分。
4. EZH1/EZH2 抑制同时改变大量通路，关键必要机制尚未逐个验证。
5. 两个单细胞数据的公共处理后文件按处理组汇总，缺乏逐鼠细胞标签，限制统计推断。
6. 单细胞样本总量较小：肿瘤内 1,602 个、外周血 1,890 个处理后细胞。
7. 公共数据没有 TCR/V(D)J，无法判断状态差异是否由少数克隆扩增驱动。
8. GEO 未覆盖多数功能实验的机器可读源数据，也没有公开分析代码，完整复现性有限。
9. 增强 CAR-T 扩增和细胞因子也可能增加 CRS 等毒性；该研究没有能力充分评价组合安全性。
10. RNA 和 ATAC 数据来自不同细胞系、不同场景，跨组学整合需要避免把相关性误作同一系统内直接因果。

## 11. 对本章节的作用

该文适合作为“**通过肿瘤侧扰动导航 T 细胞状态**”的代表：它表明 CAR-T 状态不只由细胞内在工程决定，也会被靶细胞的表观遗传和免疫原性强烈塑造。

在综述结构中，它可连接三部分：

1. 肿瘤细胞表观遗传状态如何制造免疫冷环境；
2. 药物 perturbation 如何改变 T 细胞局部效应与外周记忆状态；
3. 肿瘤侧药物与 T 细胞侧基因工程如何组合。

它不能承担“临床有效性已经确立”或“EZH2 是普适 CAR-T 耗竭靶点”的证据角色。

## 12. 可直接用于综述的观点

> Porazzi 等通过肿瘤侧表观遗传扰动证明，EZH2 或 EZH1/EZH2 抑制可解除肿瘤细胞的转录抑制，增强抗原呈递、黏附、趋化和炎症程序，从而促进 CAR-T/TCR-T 的识别、浸润与持续杀伤；这一结果提示 T 细胞状态导航不仅可通过细胞内在工程实现，也可通过重塑其靶细胞生态位完成（Cancer Cell 2025）。

## 13. 避免误读

- 不要把“CAR-T 与肿瘤 avidity 增强”写成 CAR 本身亲和力改变；它是细胞整体结合强度，受黏附和共刺激等多因素影响。
- 不要把 tazemetostat 的效果归因于 CD19 上调；论文未观察到 CD19 增加。
- 不要把 scRNA-seq 细胞数差异直接当作体内扩增倍数。
- 不要把处理组汇总矩阵中的单细胞当成独立动物重复。
- 不要把肿瘤内细胞毒与外周早期记忆两个截面写成已证实的分化轨迹。
- 不要把 EZH2 抑制等同于直接解除 CAR-T 耗竭；直接预处理或敲除 CAR-T 内 EZH2 并未产生同等获益。
- 不要把多个细胞系和异种移植模型中的一致方向写成已证明的患者疗效。
- 不要声称论文公开了完整分析代码或全部原始实验数据；公开部分主要是四套测序数据。
