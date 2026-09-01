# 《c-Jun overexpression in CAR T cells induces exhaustion resistance》精读

## 论文信息

- 作者：Lynn RC, Weber EW, Sotillo E, Gennert D, Xu P, Good Z, Anbunathan H, Lattin J, Jones R, Tieu V, Nagaraja S, Granja J, de Bourcy CFA, Majzner R, Satpathy AT, Quake SR, Monje M, Chang HY, Mackall CL
- 期刊：*Nature* 576:293–300；在线发表于 2019 年 12 月 4 日
- DOI：[10.1038/s41586-019-1805-z](https://doi.org/10.1038/s41586-019-1805-z)
- PubMed：[PMID 31802004](https://pubmed.ncbi.nlm.nih.gov/31802004/)
- 开放全文：[PMC6944329](https://pmc.ncbi.nlm.nih.gov/articles/PMC6944329/)
- 数据项目：[NCBI BioProject PRJNA563926](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA563926)
- 五个 GEO 入口：[bulk RNA GSE136891](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE136891)、[in-vitro scRNA GSE136874](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE136874)、[in-vivo scRNA GSE136805](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE136805)、[ATAC GSE136796](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE136796)、[ChIP GSE136853](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE136853)

## 一句话结论

耗竭型 CAR-T 中 AP-1 失衡表现为 BATF/IRF4 占位而可用 c-Jun 不足；工程化过表达 c-Jun 可提高扩增、细胞因子和肿瘤控制，并在体内肿瘤浸润细胞的单细胞转录组中降低功能障碍程序，但其主要体现为转录调控重编程而非大规模逆转 ATAC 可及性。

## 数据护照（先看这一表）

| 数据层 | 公共入口与规模 | 关键内容 |
|---|---:|---|
| bulk RNA-seq | GSE136891；46 个样本 | naive/CM 来源，GD2/CD19/HA CAR，D7/D10/D14，JUN 对照 |
| in-vitro scRNA-seq | GSE136874；2 个 10x 样本、1,530 个细胞 | 804 CD19-28ζ 与 726 GD2-28ζ；慢性 tonic signaling 比较 |
| in-vivo scRNA-seq | GSE136805；2 个 10x 样本、17,931 个细胞 | 6,946 Control-Her2-BBζ 与 10,985 JUN-Her2-BBζ TIL |
| ATAC-seq | GSE136796；20 个样本 | CD19-28ζ/HA-28ζ，CD4/CD8 和 naive/CM 来源，重复 |
| ChIP-seq | GSE136853；11 个样本 | c-Jun、IRF4 与 input；Control/JUN-HA CAR，两次 transduction |
| 总 GEO 记录 | 81 个 GSM | 样本数不能相加解释为供者数；包含技术/生物重复与不同 assay |
| 体内模型 | 多种异种移植实体瘤/血液瘤 | 原始纵向数据主要在图源/补充材料，不在 GEO 中统一打包 |

## 1. 研究要解决的问题

高 tonic-signaling CAR 可快速出现耗竭，但“缺少哪个可工程化的转录因子”并不清楚。作者比较非耗竭 CD19 CAR 与易耗竭 GD2/HA CAR，提出 AP-1 复合物组成失衡是关键，并检验 c-Jun 增补能否让 CAR-T 对耗竭具有抵抗力。

## 2. 工程和分析框架

### 2.1 细胞工程

研究使用多种 CD28ζ 或 4-1BBζ CAR（CD19、GD2、HER2、CD22 等）。JUN 通过双顺反子载体与 CAR 共表达，也设计了 c-Jun 突变体和可药物调控版本；同时用 CRISPR 敲除 JUNB、BATF、BATF3、IRF4 等节点测试机制。

### 2.2 多组学证据

- bulk RNA-seq 定义 chronic stimulation 与 JUN rescue 的转录变化；
- 10x scRNA-seq 分析细胞间异质性和体内 TIL 状态；
- ATAC-seq 检验染色质开放是否整体改变；
- c-Jun/IRF4 ChIP-seq 检验 AP-1–IRF4 占位重分配；
- 流式、长期扩增、细胞因子、连续杀伤和多种异种移植模型验证功能。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

这是一套围绕“**CAR 构建 × T 细胞来源 × 培养时间 × JUN 工程 × assay**”组织的多组学数据。它不是一个统一 AnnData，而是五个 GEO Series：bulk RNA、两套 scRNA、ATAC、ChIP。复用时应先建立跨 Series 的实验字典。

单细胞数据分为两个完全不同场景：

1. **体外形成耗竭**：D10 CD19-28ζ 与 GD2-28ζ CAR-T；
2. **体内工程干预**：143B osteosarcoma 小鼠肿瘤中的 Her2-BBζ 对照与 JUN-Her2-BBζ 人 TIL。

二者不能直接合并为一个“JUN 前后”单细胞比较，因为第一套没有 JUN 条件，第二套改变了场景、CAR 和测序深度。

### 3.2 多大规模、覆盖哪些条件

| GEO | 样本/细胞 | 设计细节 | 数据深度 |
|---|---:|---|---|
| GSE136891 | 46 bulk 样本 | naive/central-memory 来源；GD2、CD19、HA CAR；D7/10/14；含 JUN 条件 | BGISEQ-500，SE50，约 3,000 万 reads/样本 |
| GSE136874 | 2 library，1,530 cells | 804 CD19-28ζ；726 GD2-28ζ | >100,000 reads/cell；中位 12,675/15,708 UMI，2,990/3,446 genes |
| GSE136805 | 2 library，17,931 cells | 肿瘤浸润 6,946 control 与 10,985 JUN | 平均 49,542 normalized reads/cell；中位 4,281 UMI、1,599 genes |
| GSE136796 | 20 ATAC 样本 | CD19-28ζ/HA-28ζ；CD4/CD8、naive/CM；重复 | 原始 FASTQ 在 SRA/GEO RAW tar |
| GSE136853 | 11 ChIP/input 样本 | c-Jun、IRF4、input；Control/JUN-HA；两次 transduction | NextSeq/HiSeq，2×75 bp |

GSE136805 的细胞先按 ≥500 genes、≤20,000 UMI、≤10% mitochondrial reads 过滤；TIL 来自 6 只 NSG 小鼠、于输注后 14 天分选并合并，因此 17,931 个细胞不是 17,931 个独立动物重复。

### 3.3 GEO 中具体有什么文件

| 入口 | 处理数据 | 原始数据 |
|---|---|---|
| GSE136891 | `Processed_TPMs.csv.gz`、`Processed_TPMs_additional.csv.gz` | 46 个 SRA/BioSample FASTQ |
| GSE136874 | `GSE136874_RAW.tar`，按 GSM 打包 10x matrix | SRA 可下载 10x 原始 reads |
| GSE136805 | filtered feature-barcode H5、CD4/CD8/T/normalized Seurat RDS、Cell Ranger summary | SRA raw reads |
| GSE136796 | `GSE136796_RAW.tar`，含每样本处理文件 | SRA raw ATAC reads |
| GSE136853 | `GSE136853_RAW.tar` + readme | SRA raw ChIP/input reads |

BioProject 页面把这些实验连接在 PRJNA563926 下；其中 bulk RNA 项目显示 46 个 BioSamples/experiments，SRA 总量约 121 Gbases。下载量远大于 GEO supplementary matrices，普通分析优先取处理文件。

### 3.4 如何获取：按目的选择

#### 路线 A：复现 AP-1/JUN 转录结论

下载 GSE136891 的 TPM 表和样本元数据。先比较 HA/GD2 与 CD19 CAR，再在相同 CAR、时间点和来源亚群内比较 JUN；不要把不同时间或 naive/CM 来源混为重复。

#### 路线 B：研究体外耗竭异质性

下载 GSE136874 RAW tar。它只有两个 library，建议保留原始 condition 作为唯一批次变量，并明确细胞数小、测序深度高。可重算 exhaustion TF correlation，但不要用 donor-level 统计解释，因为 GEO 设计未提供多供者单细胞重复。

#### 路线 C：研究 JUN 体内状态

GSE136805 已提供 filtered H5 和多个 Seurat RDS，可直接使用。若做发表级差异表达，应考虑 6 只鼠在测序前被 pooled，cell-level P 值会夸大独立重复数；可将其作为描述性单细胞证据，再用独立功能实验验证。

#### 路线 D：机制重分析

从 GSE136796/136853 下载 FASTQ，统一比对并对 c-Jun/IRF4 ChIP 做 spike-in-aware normalization。GEO 的处理文件适合图级复现，但不同抗体/input 的样本不可直接用普通 library-size normalization 混合。

### 3.5 下载后先做什么

对每个 Series 导出 SOFT/MINiML 元数据，构造以下主键：

```text
assay | GSM | donor_or_transduction | CAR | costim | subset | day | JUN | in_vitro_or_TIL
```

GSE136805 的 filtered H5 可用 Scanpy 读取：

```python
import scanpy as sc
adata = sc.read_10x_h5(
    "GSE136805_JUN-Her2-BBz-and-Control-Her2-BBz-filtered_feature_bc_matrix.h5"
)
print(adata)
```

条件标签可能在 barcode 前缀或作者 RDS metadata 中；读取后必须核对，不要只凭文件名分组。

## 4. 主要机制发现

易耗竭 CAR-T 中多种 AP-1/bZIP 因子上调，但 c-Jun 相对不足，导致 BATF/IRF4 主导的复合物和耗竭相关转录。c-Jun 过表达：

- 提高长期扩增和低抗原条件下功能；
- 降低 terminal differentiation 与 inhibitory receptor 程序；
- 增强多种实体瘤模型中的肿瘤控制；
- 在体内 Her2-BBζ TIL 中维持增殖/效应并降低 dysfunction signature；
- 改变 c-Jun 在 IRF4-bound loci 的结合，包括 TCF7、HAVCR2、HIF1A 等位点。

## 5. 转录重编程与表观重编程不是一回事

Extended Data 显示 JUN 对转录状态影响显著，但 ATAC 的全局改变较有限。因此本研究更支持：外源 c-Jun 通过改变现有开放区域中的转录因子竞争/复合物组成来重编程表达，而非像“休息”那样大范围重置可及性。

这对状态导航很重要：不同操作可抵达相似功能终点，却作用于不同层级。JUN 是**转录因子剂量校准**，rest 更接近**信号历史与表观状态重置**。

## 6. 功能 readout

论文用长期扩增、连续肿瘤共培养、IL-2/IFNγ/TNFα、CD107a、体内肿瘤生物发光和生存验证组学结论。scRNA 不是疗效的唯一证据，而是解释 JUN 细胞为何在肿瘤中保持更好的细胞状态。

## 7. 推荐图版

- **Fig. 1–2**：不同 CAR 的耗竭与 AP-1 不平衡，适合提出问题。
- **Fig. 3**：JUN 提升扩增与功能，是工程主图。
- **Fig. 5**：多种体内肿瘤模型，适合疗效论证。
- **Fig. 6 / Extended Data Fig. 6**：转录与表观层级的差异，适合“分子驱动”章节。

若只能选一组，选 Fig. 3 + Fig. 6。

## 8. 创新价值

1. 从耗竭状态描述推进到可工程化转录因子节点。
2. 用五类测序数据分解细胞状态、染色质和 TF 占位。
3. 证明 c-Jun 工程可跨多个 CAR 与肿瘤模型改善功能。
4. 揭示 AP-1 不是简单“高或低”，而是复合物组成和因子竞争问题。

## 9. 局限性

1. 主要是健康供者细胞和小鼠异种移植，患者制造物的泛化需验证。
2. JUN 是原癌基因相关转录因子，长期安全性、插入与表达剂量需严格控制。
3. 两套 scRNA 的实验场景不同，不能作直接纵向轨迹。
4. 体内 scRNA 在分选前 pooled，缺乏小鼠层面的单细胞独立重复。
5. 过表达可能引起参考外的长期状态；论文随访时间不足以覆盖所有风险。
6. ATAC 改变有限，不能把转录恢复写成全面表观遗传逆转。

## 10. 对本章节的作用

该文适合作为“**识别分子驱动后，以转录因子工程导航状态**”的代表。它与 Weber 2021 的 rest 形成互补：一个补充 AP-1 组分，一个改变信号历史；二者共同说明状态优化可在不同控制层级实现。

## 11. 可直接用于综述的观点

> 在多种 tonic-signaling CAR 模型中，c-Jun 过表达通过重平衡 AP-1/IRF4 转录网络提高扩增、效应和体内抗肿瘤活性；单细胞 TIL 数据显示 JUN 工程维持更低的 dysfunction 程序，但 ATAC 结果提示其主要是转录调控重编程而非全局染色质重置（Nature 2019, Lynn）。

## 12. 避免误读

- 不要把 JUN 工程概括为“关闭耗竭基因”。
- 不要把 17,931 个 TIL 当作 17,931 个生物重复。
- 不要把 GSE136874 与 GSE136805 直接拼成纵向数据。
- 不要声称 c-Jun 已证明临床安全或有效。
- 不要把 GEO 的 81 个 GSM 写成 81 位供者。

## 13. 数据复用优先级

最适合立即复用的是 GSE136805 的 filtered H5/RDS 和 GSE136891 TPM 表；若研究 TF 占位机制，再下载 GSE136853。五套数据都值得保留，但必须用实验字典分层分析，不能把它们粗略合并成一个 batch-corrected atlas。
