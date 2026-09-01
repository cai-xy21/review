# 《RASA2 ablation in T cells boosts antigen sensitivity and long-term function》精读

## 论文信息

- 作者：Joseph Carnevale、Erez Shifrut、Nina Kale 等
- 期刊：*Nature*
- 年份：2022；609: 174–182；在线发表于 2022 年 8 月 24 日
- DOI：10.1038/s41586-022-05126-w
- 原文：[Nature](https://www.nature.com/articles/s41586-022-05126-w)
- PubMed：[PMID 36002574](https://pubmed.ncbi.nlm.nih.gov/36002574/)
- 免费全文：[PMC9433322](https://pmc.ncbi.nlm.nih.gov/articles/PMC9433322/)
- 本研究 RNA-seq：[GEO GSE204862](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE204862)
- 分析代码：[Zenodo 10.5281/zenodo.6808407](https://doi.org/10.5281/zenodo.6808407)

## 一句话结论

六类原代人 T 细胞全基因组 CRISPR-KO 筛选共同锁定 RASA2；删除 RASA2 解除 RAS–MAPK 信号刹车，使 TCR-T/CAR-T 对低抗原更敏感，并在反复抗原刺激后维持代谢、细胞因子和持续杀伤能力。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| pooled screens | 6 类条件 | stimulation、Treg、adenosine、cyclosporine、tacrolimus、TGFβ |
| 文库 | Brunello 全基因组 KO | 论文未把全部 screen raw reads 放进 GSE204862；计数和 MAGeCK 结果在补充表 |
| screen donors | 4/4/2/2/2/1 | 分别对应 stimulation、Treg、adenosine、cyclosporine、tacrolimus、TGFβ |
| screen readout | CFSE-high vs CFSE-low | 识别在抑制条件下仍能增殖的 KO；不是细胞因子或单细胞 readout |
| GSE204862 | 50 个 RNA-seq 样本 | 36 个重复刺激时间序列 + 8 个 Stim1 KO/CTRL + 6 个 Stim5 KO/CTRL |
| 重复刺激序列 | 3 donors × TCR/CAR × 6 time points | Input、Stim1–Stim5，共 36 样本；这部分是未按 KO/CTRL 分组的自然响应时间序列 |
| RASA2 比较 | Stim1 4 donors × 2；Stim5 3 donors × 2 | 处理矩阵分开提供，统计时必须控制 donor |
| 外部数据 | GSE119450、GSE138459、GSE86881、GSE89307 等 | 用于 RASA2 表达背景，不属于本研究新产生的 screen 数据 |

## 1. 研究要解决的问题

细胞治疗失败既可能来自肿瘤微环境抑制，也可能来自持续低抗原和反复刺激造成的内在功能衰减。作者希望找到一种基因编辑，使 T 细胞同时：

1. 抵抗多种免疫抑制信号；
2. 对低密度抗原仍有响应；
3. 在慢性刺激下维持杀伤、细胞因子与代谢；
4. 不依赖单一 CAR 或 TCR 架构；
5. 在血液瘤和实体瘤模型中均产生治疗获益。

## 2. 扰动与筛选框架

### 2.1 六类全基因组 KO 筛选

作者使用 SLICE：先以慢病毒将 Brunello sgRNA 文库导入原代人 T 细胞，再电转 Cas9 蛋白。第 14 天以 CFSE 标记并重新刺激，同时施加不同抑制条件；三天后分选 CFSE-low 高增殖与 CFSE-high 低增殖细胞，测序 sgRNA，并用 MAGeCK 做配对分析。

条件和供者数为：

| 条件 | 供者数 | 代表的压力 |
|---|---:|---|
| stimulation only | 4 | 基础再刺激/增殖 |
| Treg co-culture | 4 | 细胞性免疫抑制 |
| adenosine agonist CGS-21680 | 2 | 腺苷信号 |
| cyclosporine | 2 | calcineurin 抑制 |
| tacrolimus | 2 | calcineurin 抑制 |
| TGFβ | 1 | TGFβ 免疫抑制 |

由于 TGFβ 只有 1 个供者，它适合发现候选，不适合单独支撑稳健的人群结论。

### 2.2 RASA2 定点验证

RASA2 是 Ras GTPase-activating protein。作者用多条 sgRNA 编辑原代 T 细胞，并在 anti-CD3/CD28、NY-ESO-1 TCR-T、CD19 CAR-T、不同抗原密度、Treg 抑制和反复肿瘤刺激中验证。核心机制读出包括 p-MEK/p-ERK、AP-1/NF-κB/NFAT 报告、RNA-seq、线粒体染料和 Seahorse OCR/ECAR。

### 2.3 体内验证

使用白血病和实体瘤异种移植模型比较 RASA2 KO 与 control TCR-T/CAR-T 的竞争适应度、肿瘤控制和生存。体内证据支持治疗潜力，但仍是前临床异种移植，不等同于人体安全性。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

公开数据有三个互相独立的层次：

1. **全基因组 CRISPR screen 表**：六类条件下 guide/gene counts、LFC、MAGeCK 分数和跨 screen 命中；
2. **RASA2 验证 RNA-seq**：Stim1 与 Stim5 的 RASA2 KO/CTRL，以及 TCR-T/CAR-T 的 Input→Stim5 时间序列；
3. **外部表达数据再分析**：既有 CROP-seq、感染、耗竭、DICE/BioGPS 和人肿瘤 TIL 数据，用于说明 RASA2 的表达背景。

`GSE204862` 只对应第 2 层。论文的主发现来自第 1 层，因此只下载 GEO 会遗漏最重要的 screen count 和 hit ranking。

### 3.2 多大规模、覆盖哪些生物情境

`GSE204862` 的 50 个样本可严格拆分为：

| 数据块 | 样本数 | 组成 |
|---|---:|---|
| TCR/CAR repeated stimulation | 36 | 3 donors × 2 receptors（NY-ESO-1 TCR、CD19 CAR）× 6 time points（Input、Stim1–Stim5） |
| RASA2 KO vs CTRL at Stim1 | 8 | 4 donors × 2 genotypes |
| RASA2 KO vs CTRL at Stim5 | 6 | 3 donors × 2 genotypes |
| 合计 | 50 | 均为 Homo sapiens bulk RNA-seq 样本记录 |

50 个 GEO samples 不是 50 名供者。时间序列主要来自 donor 1、2、4，Stim1 KO/CTRL 包括 donor 1–4，Stim5 KO/CTRL 为 3 名供者。设计矩阵应以 GSM 标题为准，不可只用文件列顺序猜测配对。

### 3.3 公共数据包有什么

| 文件/资源 | 体积 | 内容与用途 |
|---|---:|---|
| `GSE204862_RepStim_CARnTCR_countmatrix.tsv.gz` | 970.4 KB | 36 个 TCR/CAR 重复刺激时间序列的 gene count matrix |
| `GSE204862_RASA2KO_Stim1_countmatrix_fix.tsv.gz` | 238.6 KB | 4 donors 的 Stim1 RASA2 KO vs CTRL |
| `GSE204862_RASA2KO_Stim5_countmatrix_fix.tsv.gz` | 137.7 KB | 3 donors 的 Stim5 RASA2 KO vs CTRL |
| SRA raw reads | 50 sample records 对应 runs | 从 GSE204862 的 SRA Run Selector 获取 FASTQ |
| Supplementary Table 1 | 约 118.7 MB xlsx | 六类 screen 的 gene/guide-level MAGeCK 结果及 shared-hit 分析；主筛选核心数据 |
| Supplementary Table 2 | 约 6.4 MB xlsx | 条件特异性 hit 分析 |
| Supplementary Table 3 | 约 82.2 KB xlsx | guide 序列与 arrayed validation |
| Supplementary Table 4 | 约 4.4 MB xlsx | 全部 RNA-seq 差异表达结果 |
| [Zenodo 6808407](https://doi.org/10.5281/zenodo.6808407) | 代码仓库快照 | 论文分析代码；建议记录版本和文件 checksum |

外部数据还包括：

- [GSE119450](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE119450)：原代人 T 细胞 CROP-seq，用于比较 RASA2、CBLB 和 control 扰动；
- GSE89307、GSE86881、GSE138459：感染或慢性刺激表达背景；
- DICE、BioGPS、结直肠癌和 NSCLC 单细胞门户：说明 RASA2 的组织/状态表达。

这些外部队列不能计入本研究的 50 个新 RNA-seq 样本。

### 3.4 如何获取：按目的选择

#### 路线 A：只复核 RASA2 转录效应

从 GSE204862 页面下载三张 `.tsv.gz` 计数矩阵。用样本名解析 donor、受体类型、刺激轮次和 genotype，再分别建立设计矩阵。

#### 路线 B：复现六类 genome-wide screen

从 Nature/PMC 下载 Supplementary Tables 1–3，并使用 Zenodo 代码。Table 1 很大，优先用 R/Python 的 `readxl` 按 sheet 读取；不要在表格软件中手工复制，容易丢失长序列或科学计数格式。

#### 路线 C：从原始 RNA-seq 开始

在 GSE204862 的 SRA Run Selector 导出 accession list，然后：

```bash
prefetch --option-file SRR_Acc_List.txt
fasterq-dump --split-files --threads 8 SRRxxxxxxx
```

论文使用 Kallisto 对 GRCh38/Ensembl release 96 定量；严格复现应固定 transcriptome release，而不是直接换成当前版本。

#### 路线 D：复核“RASA2 与耗竭/激活相关”

分别下载外部 GEO/DICE/BioGPS 数据，并把它们当作独立再分析。不同物种、平台和归一化方式之间只做趋势比较，不宜把归一化表达值合并成一个统计模型。

### 3.5 下载后先做什么

```python
import pandas as pd

stim1 = pd.read_csv(
    "GSE204862_RASA2KO_Stim1_countmatrix_fix.tsv.gz",
    sep="\t",
    index_col=0,
)
print(stim1.shape)
print(stim1.columns.tolist())
```

随后应：

1. 从列名提取 donor 与 KO/CTRL；
2. 以 `~ donor + genotype` 建立配对差异模型；
3. 对 Stim1 和 Stim5 分开分析，避免把轮次当普通重复；
4. 对 36 样本时间序列使用 donor、receptor、stimulation round 及交互项；
5. screen 表中先过滤低 count guides，再按原文 MAGeCK 配对策略复核；
6. 区分技术重复、供者重复和连续时间点。

## 4. 主要发现

六类 screen 的交集将 TMEM222 与 RASA2 推到前列；条件特异 hit 还能重现 ADORA2A、TGFBR1 等预期通路。RASA2 KO 在多种抑制条件下提高增殖，并在低 TCR/CAR 抗原量时增强 p-MEK/p-ERK、激活和杀伤。

反复抗原刺激后，RASA2 KO T 细胞维持：

- 更高的细胞增殖与存活；
- 更强的 IFN-γ、TNF 和 IL-2 产生；
- 更持久的连续肿瘤细胞清除；
- 更高的氧化磷酸化和糖酵解能力；
- 更强的线粒体质量、膜电位和备用代谢能力。

## 5. 状态与分子 driver

RASA2 促进 RAS-GTP 水解，相当于 TCR 下游 RAS–MAPK 的负反馈节点。删除 RASA2 后，细胞对弱抗原的输入—输出曲线左移，同时 AP-1/NF-κB 程序增强。RNA-seq 提示细胞周期、转录和代谢通路上调；慢性刺激后还出现线粒体 fitness 相关基因变化。

值得注意的是，RASA2 KO 在急性条件下可呈更强 effector-memory-like 特征（例如 TCF7/SELL 下降），但在反复刺激中仍保持长期功能。这说明“初始/干样 marker 高”不是持续功能的唯一道路；状态质量要结合抗原敏感性、代谢储备和连续杀伤测量。

## 6. 推荐图版

- **Fig. 1**：六类全基因组 screen 设计、交集及 RASA2 候选；适合作为 perturbation 章节主图。
- **Fig. 2**：低抗原敏感性、RAS–MAPK 和转录重编程；适合 molecular driver。
- **Fig. 3**：重复刺激后的持久杀伤、细胞因子和代谢；与“长期功能状态”最相关。
- **Fig. 4–5**：白血病和实体瘤模型的治疗效果；适合转化意义。

若只能选一张，选 Fig. 3；它最直接连接 cell state、function 和 therapy-relevant persistence。

## 7. 创新价值

1. 在六种不同免疫抑制条件下做系统性全基因组筛选，寻找跨压力通用靶点。
2. 将低抗原敏感性、慢性刺激、代谢和连续杀伤放进同一验证链。
3. 同时覆盖 TCR-T 与 CAR-T、血液瘤与实体瘤。
4. 提供 screen tables、RNA-seq、代码和 source data，数据复用路径较完整。

## 8. 局限性

1. 各 screen 的供者数不平衡，TGFβ 仅 1 人。
2. CFSE readout 偏向增殖，可能遗漏能提升杀伤但不提升扩增的扰动。
3. RASA2 是广泛信号放大器；过强敏感性可能增加脱靶组织识别、细胞因子毒性或自主激活风险。
4. 异种移植模型不能完整评估人免疫系统中的毒性和竞争生态。
5. 公开 GEO 不是 screen raw-count 主入口，复现者容易漏下补充表。
6. 许多外部表达分析是相关性证据，不能证明 RASA2 上调导致耗竭。

## 9. 对本综述架构的作用

该文适合支撑“optimize the conditions for navigating T cell states”：它不是只改变一个 marker，而是通过调节信号阈值，使 T 细胞在低抗原和慢性刺激下仍位于高功能区域。它也说明优化目标应是多维的——敏感性、连续杀伤、细胞因子、代谢和体内持久性需要联合约束。

它尚未构成 real-time optimization system，因为所有关键 readout 都是分批、离线采样；但重复刺激时间序列可作为未来建立动态状态控制模型的训练数据原型。

## 10. 可直接用于综述的观点

> 多条件 genome-wide CRISPR screens 将 RASA2 识别为原代人 T 细胞的共享信号检查点；RASA2 缺失放大 RAS–MAPK 响应，使 TCR-T 和 CAR-T 对低抗原更敏感，并在连续抗原刺激后维持代谢储备、细胞因子和持续杀伤，提示细胞治疗状态优化不仅是“保持干性”，还可以通过重设抗原响应阈值实现（Nature 2022, Carnevale）。

## 11. 避免误读

- 不要把 GSE204862 的 50 samples 写成 50 donors。
- 不要把 GSE204862 当成六类 CRISPR screen 的完整原始数据包。
- 不要把外部 GSE119450 等算入本研究新产生的数据规模。
- 不要将“低抗原更敏感”自动等同于“临床更安全”。
- 不要只用 TCF7/SELL 判断长期功能；本文的核心证据是连续功能和代谢 readout。
