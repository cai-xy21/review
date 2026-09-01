# 《Single-cell CRISPR screens in vivo map T cell fate regulomes in cancer》精读

## 论文信息

- 作者：Peipei Zhou、Hao Shi、Hongling Huang 等
- 期刊：*Nature*
- 年份：2023；624: 154–163；在线发表于 2023 年 11 月 15 日
- DOI：10.1038/s41586-023-06733-x
- 原文：[Nature](https://www.nature.com/articles/s41586-023-06733-x)
- PubMed：[PMID 37968405](https://pubmed.ncbi.nlm.nih.gov/37968405/)
- 免费全文：[PMC10700132](https://pmc.ncbi.nlm.nih.gov/articles/PMC10700132/)
- 全部新数据：[GEO GSE216800](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE216800)

## 一句话结论

作者用 720 条 sgRNA/360 个 dual-guide vector 在 B16-OVA 肿瘤内扰动 180 个转录因子，对 42,209 个高质量 OT-I TIL 同时读取 guide 与转录组，构建 Tpex→Tex 命运 regulome；IKZF1、ETS1 和 RBPJ 分别控制不同分化分支，其中 RBPJ 缺失可重激活 terminally exhausted-like T 细胞的细胞毒程序。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| scCRISPR targets | 180 TFs | 由既有 RNA/ATAC/TF motif 三类证据筛选，不是全基因组 |
| library | 720 sgRNA；360 dual-guide vectors | 每基因 4 guides，两个 dual-guide constructs；NTC 另设 |
| scCRISPR samples | 16 个 10x reactions | GEO GSE216909；B16-OVA day 7 TIL |
| guide capture | 82% cells 检到 ≥1/720 guide | 双 guide 同 vector 一致检出率 81%；多 target cells 被剔除 |
| 最终 scCRISPR | 42,209 QC OT-I cells | median 185 cells/target；5,371 NTC cells |
| regulome targets | 172 TFs | 8 个低于 48 cells 的 perturbation 不进入 network analysis |
| 状态 | 6 clusters；Tox⁺ 细分 4 states | Tpex1、Tpex2、Tex1、Tex2；另有 Teff/perturbation-specific 等 |
| 单基因 scRNA validation | 69,079 cells | Ikzf1 28,945；Ets1 20,618；Rbpj 19,516 |
| GSE216800 | 89 sample records，8 子系列 | microarray、scCRISPR、3 套 scRNA、3 套 ATAC |
| 代码 | upon request | GEO/Source data 开放，但分析代码并未公开仓库化 |

## 1. 研究要解决的问题

肿瘤内 cytotoxic T lymphocyte（CTL）可处在 stem/progenitor exhausted、terminal exhausted、cycling 和 effector 等状态，但普通 scRNA 只能给相关网络。作者要建立 causal regulome，回答：

1. 哪些 TF 控制 Tpex→Tex 分化的各个分支；
2. 每个 perturbation 如何改变整个转录组和状态比例；
3. terminal Tex 是否存在可功能再激活的调控节点；
4. 单细胞 screen 命中能否通过独立 scRNA、ATAC、遗传互作和治疗实验验证。

## 2. 体内 scCRISPR 框架

### 2.1 dual-guide direct capture

作者改造 retroviral vector，使其同时表达 Ametrine 和两个可直接捕获的 sgRNA。每个 TF 使用 4 条 guides，组成两个 dual-guide vectors，提高 knockout 强度并可用同一 vector 的双 guide 共检出做质量检查。

### 2.2 TF 候选选择

180 个 TF 来自四个既有 CD8⁺ T cell RNA/ATAC 数据集，对 early/late exhausted 或 Tpex/Tex 做：

- differential expression；
- differential accessibility；
- TF motif enrichment。

至少在三类分析中的两类命中才进入文库。这提高命中密度，但意味着图谱被既有 marker/数据先验限制，不是无偏 genome-wide regulome。

### 2.3 体内读取

Cas9-OT-I cells 转入 B16-OVA-bearing mice，第 7 天回收 TIL，使用 16 个 10x 3′ v3.1 + Feature Barcode reactions。同时测 gene expression 和 CRISPR guide identity，再用状态比例、差异表达和 perturbation-to-target effect 构建网络。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

GSE216800 是本批文献中最复杂的 GEO 包之一，包含：

1. 主 180-TF in vivo scCRISPR；
2. Ikzf1、Ets1、Rbpj 三个单基因独立 scRNA validation；
3. Rbpj、Ets1、Ikzf1 三套 ATAC-seq；
4. genetic interaction/机制 microarray。

作者还再分析大量外部人/鼠 scRNA、bulk RNA 和 ATAC 数据。外部 accessions 在论文 Data availability 中列出，但不属于 GSE216800 的 89 个新样本。

### 3.2 多大规模、覆盖哪些生物情境

| 子系列 | 样本数 | 数据/比较 |
|---|---:|---|
| [GSE216909](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE216909) | 16 | 主 180-TF scCRISPR，OT-I TIL |
| [GSE216796](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE216796) | 4 | Ikzf1 sgRNA vs NTC scRNA |
| [GSE218372](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE218372) | 4 | Ets1 sgRNA vs NTC scRNA |
| [GSE216798](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE216798) | 2 | Rbpj sgRNA vs NTC scRNA |
| [GSE216797](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE216797) | 12 | Rbpj ATAC-seq |
| [GSE239801](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE239801) | 16 | Ets1 ATAC-seq |
| [GSE239802](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE239802) | 20 | Ikzf1 ATAC-seq |
| [GSE216749](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE216749) | 15 | microarray，包含遗传互作/机制比较 |
| GSE216800 | 89 | 上述八个子系列合计 |

主 scCRISPR 的数量层次：

- 平均每细胞 26 guide UMI；
- 82% 细胞检到文库 guide；
- 同一 dual-guide vector 的两个 guides 在双 guide 细胞中共同出现率 81%；
- 去除多 target、低质量和 doublet 后 42,209 cells；
- median 185 cells/TF、median 35 guide UMI/singlet；
- NTC 细胞 5,371；
- 8 个 perturbation 少于 48 cells，不进入 network；最终 172 TF regulome。

单基因 scRNA 验证总计 69,079 cells：

| 实验 | 总 cells | 组别 |
|---|---:|---|
| Ikzf1 | 28,945 | NTC 14,044；sgIkzf1 14,901 |
| Ets1 | 20,618 | NTC 10,562；sgEts1 10,056 |
| Rbpj | 19,516 | NTC 10,918；sgRbpj 8,598 |

### 3.3 公共数据包有什么

| 文件/资源 | 体积 | 内容与用途 |
|---|---:|---|
| `GSE216800_RAW.tar` | 2.0 GB | SuperSeries 聚合 CEL、CSV、TAR；可能与子系列重复 |
| `GSE216909_RAW.tar` | 1.3 GB | 主 scCRISPR 的 CSV/TAR 处理数据；raw reads 在 SRA |
| `GSE216796_RAW.tar` | 350.0 MB | Ikzf1 scRNA 处理 TAR |
| `GSE218372_RAW.tar` | 258.2 MB | Ets1 scRNA 处理 TAR |
| `GSE216798_RAW.tar` | 120.5 MB | Rbpj scRNA 处理 TAR |
| `GSE216797_Rbpj_ATAC_summarizedCounts.xlsx` | 6.3 MB | Rbpj ATAC peak counts |
| `GSE239801_Ets1_ATAC_summarizedCounts.csv.gz` | 2.2 MB | Ets1 ATAC peak counts |
| `GSE239802_Ikzf1_ATAC_summarizedCounts.csv.gz` | 2.7 MB | Ikzf1 ATAC peak counts |
| `GSE216749_RAW.tar` | 18.4 MB | 15 个 microarray CEL；sample table 另有 processed data |
| Supplementary Tables 1–13 | 394.4 KB zip | 180 TF candidate、720 guides、screen/network、验证与 guide sequences |

处理 TAR 中可能包括 Cell Ranger matrices 和 guide matrices；实际文件名应下载后列目录确认。论文没有提供统一 `.h5ad`/Seurat reference object，也没有公开分析代码仓库。

### 3.4 如何获取：按目的选择

#### 路线 A：只复核主 regulome

下载 `GSE216909_RAW.tar` 和 Supplementary Tables。构建 cell × gene 与 cell × guide matrices，按作者 QC 重现 42,209 cells，并从四条 sgRNA 聚合到 TF target。

#### 路线 B：复核三个关键 TF

分别下载 GSE216796、GSE218372、GSE216798 的处理数据及对应 ATAC count matrix。单独分析可避免主 screen 中每 target 只有约百级 cells 的稀疏问题。

#### 路线 C：从 raw 10x reads 重建

从子系列 SRA Run Selector 导出 runs，分清 GEX 与 guide Feature Barcode。使用与论文一致或可比的 Cell Ranger pipeline，并实现 dual-guide pattern `GGG(BC)GTTT` 的捕获逻辑。

#### 路线 D：完整获取但避免重复

不要同时下载 2.0 GB SuperSeries TAR 和所有子系列 TAR，除非需要核对。先下载各子系列更容易知道 assay；总包主要是便利聚合。

#### 路线 E：外部验证数据

论文再分析 GSE156728、GSE99254、GSE108989、GSE122713、GSE123813、GSE120575、GSE86042、GSE161983、GSE164177、GSE72056、GSE123139、GSE98638、E-MTAB-8832 等 scRNA，以及 GSE160160/GSE89307 bulk RNA、GSE160341 ATAC。只在重做跨数据验证时下载，不能算入新数据规模。

### 3.5 下载后先做什么

1. 列出 TAR 内容并建立 sample/library manifest；
2. 重现 mt% <10%、features <6,000、UMI <60,000 等 QC；
3. 复核 ≥1 guide UMI、dual-guide concordance、多 target 排除；
4. 统计每个 TF 细胞数并执行 ≥48 threshold；
5. 将 16 个 reactions/动物批次保留在 metadata，做 pseudobulk 或分层验证；
6. network inference 区分 target-gene direct effect 与状态组成间接 effect；
7. 单基因验证与主 screen 的 cluster annotation 使用同一 marker/signature 定义。

## 4. 主要发现

主 regulome 将 TIL 分成 6 个 cluster，并在 Tox⁺ CTL 中定义 Tpex1、Tpex2、Tex1、Tex2。不同 TF 对状态比例和表达程序有不同效应：

- **IKZF1**：限制 CTL differentiation/effector programme；
- **ETS1**：维持 stemness-associated programme，并限制过度 effector activation；
- **RBPJ**：促进/维持终末耗竭相关程序；删除后增加 Tex1、细胞毒基因和肿瘤控制；
- 遗传互作 screen 进一步连接 Bach2、Rbpj、Irf1 等节点。

研究的重点不是列一个 hit list，而是把每个 TF 对多个 cell states 和 genes 的因果效应组织成 regulome。

## 5. 状态与分子 driver

该文将状态导航形式化为：

`perturb TF → change expression programme + alter abundance of Tpex/Tex substate → change cytotoxicity/persistence → affect tumour control`。

RBPJ 的结果尤其重要：即使细胞已在 Tex 区域，删除 Rbpj 仍可提高 `Prf1/Gzmb/Gzmk` 等效应程序，说明 terminally exhausted-like state 并非所有功能维度都不可调。但这不是“完全逆转耗竭”，因为染色质、持久性和所有状态特征未必恢复到 effector/memory。

## 6. 推荐图版

- **Fig. 1**：dual-guide scCRISPR platform、42,209 cells 与状态图谱；本综述首选。
- **Fig. 2**：180-TF causal regulome 和命运分支。
- **Fig. 3–5**：IKZF1、ETS1 的独立验证和 ATAC 机制。
- **Fig. 6**：RBPJ 对 Tex 功能再激活；适合 therapeutic navigation。
- **Fig. 7**：genetic interaction/组合调控。

若只能选一张，选 Fig. 2；若强调可操纵 terminal Tex，选 Fig. 6。

## 7. 创新价值

1. 在肿瘤内原代 CD8⁺ T 细胞中完成大规模 single-cell CRISPR screen。
2. dual-guide direct capture 提高 knockout 和 guide assignment 可靠性。
3. 同时读取状态 abundance、全转录 perturbation effect 和 regulatory network。
4. 用独立 scRNA、ATAC、遗传互作与肿瘤疗效验证关键 TF。
5. 数据包包含从主 screen 到三个关键 TF 的完整多组学层。

## 8. 局限性

1. 仅覆盖基于先验筛选的 180 TF，不是 genome-wide regulome。
2. B16-OVA/OT-I 单一抗原模型限制可推广性。
3. 每 target median 185 cells，低丰度状态与弱效应检出能力有限。
4. 细胞层统计存在 pseudoreplication 风险，需保留 16 reactions/动物批次。
5. 单时间点 day 7 不能直接估计状态转移率和方向。
6. 代码仅 upon request，计算完全复现不如数据开放充分。
7. Rbpj 缺失提高细胞毒不等同于完整恢复年轻效应/记忆状态。

## 9. 对本综述架构的作用

该文应是“link cell state/function transitions with molecular drivers”的核心文献：它把状态图谱从相关 atlas 推到因果 regulome。也适合“optimize conditions for navigating states”，因为多个 TF 对不同分支具有可比较的控制方向。

距离实时优化仍有两步：无损连续 state reporter，以及根据当前状态自适应选择 perturbation 的反馈控制。当前数据是一次性破坏性 scRNA readout。

## 10. 可直接用于综述的观点

> 体内 dual-guide scCRISPR screen 在 42,209 个高质量 OT-I TIL 中解析 180 个 TF 对 Tpex/Tex 命运的因果作用，并以独立 scRNA/ATAC 验证 IKZF1、ETS1 和 RBPJ；其中 RBPJ 删除可在 exhausted compartment 内提升细胞毒程序，说明因果 regulome 能识别仅靠状态 marker 无法发现的可导航控制点（Nature 2023, Zhou）。

## 11. 避免误读

- 不要把 720 guides 写成 720 genes；target 是 180 TFs。
- 不要把 42,209 cells 都视为 180 target 的均匀覆盖。
- 不要写成 180 TF 全部进入 network；实际为 172，8 个因细胞数低被排除。
- 不要把 89 GEO sample records 写成 89 mice 或 donors。
- 不要把 RBPJ KO 描述成“完全逆转 terminal exhaustion”。
- 不要声称代码已公开在 GitHub；论文写的是 upon request。
