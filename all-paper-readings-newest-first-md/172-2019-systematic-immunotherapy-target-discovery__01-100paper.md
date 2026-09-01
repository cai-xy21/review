# 《Systematic Immunotherapy Target Discovery Using Genome-Scale In Vivo CRISPR Screens in CD8 T Cells》精读

## 论文信息

- 作者：Matthew B. Dong、Guangchuan Wang、Ryan D. Chow 等
- 期刊：*Cell*
- 年份：2019；178(5): 1189–1204.e23；在线发表于 2019 年 8 月 1 日
- DOI：10.1016/j.cell.2019.07.044
- 原文：[Cell](https://doi.org/10.1016/j.cell.2019.07.044)
- PubMed：[PMID 31442407](https://pubmed.ncbi.nlm.nih.gov/31442407/)
- 免费全文：[PMC6719679](https://pmc.ncbi.nlm.nih.gov/articles/PMC6719679/)
- 转录组总入口：[GEO GSE132960](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE132960)
- DNA/靶向扩增测序：[SRA BioProject PRJNA549266](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA549266)
- 非基因组 source data：[Mendeley Data](https://doi.org/10.17632/2ffxmf3d7k.1)

## 一句话结论

作者把全基因组 CRISPR 敲除直接放进抗原特异性小鼠 CD8⁺ T 细胞的肿瘤浸润过程，并与体外 CD107a 脱颗粒筛选交叉，锁定 DHX37；删除 Dhx37 可增强小鼠及人 CD8⁺ T 细胞的激活、细胞因子与杀伤功能，并通过 NF-κB 轴改变效应状态。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 主筛选文库 | 129,209 条 sgRNA | 128,209 条基因靶向 sgRNA + 1,000 条非靶向对照，不是 129,209 个基因 |
| 体内浸润筛选 | 3 个注射前 input；10 个肿瘤样本 | OT-I;Cas9 CD8⁺ T 细胞进入 E0771-OVA 乳腺肿瘤；统计单位是 sgRNA abundance |
| 体外脱颗粒筛选 | 3 个生物学重复；分选 CD107a 最高 5% | 与肿瘤浸润筛选是正交 readout，交集命中 Dhx37、Lyn、Odc1 |
| TIL scRNA-seq | 552 个质控后细胞 | sgDhx37 191；vector 361；共检测 8,244 个表达基因，规模很小 |
| bulk RNA-seq | 8 个样本 | Dhx37 KO 与 vector 各 4 个生物学重复 |
| GEO | GSE132960，共 10 个样本记录 | 子系列 GSE132926（bulk，8）+ GSE132959（scRNA，2） |
| 屏幕计数 | Supplementary Table 1 | GEO 主要存转录组；筛选归一化计数和序列优先取补充表 |
| DNA 层数据 | PRJNA549266 | 主要为 Dhx37/Odc1 的 Cas9/Cas12a 靶点扩增与 indel 测序，不应误称为 scRNA 数据 |

## 1. 研究要解决的问题

体外 T 细胞 CRISPR 筛选容易测到“能不能增殖”，但无法完整复现 T 细胞归巢、穿越组织、识别肿瘤抗原并在肿瘤微环境中存活的连续选择压力。本研究同时问：

1. 哪些基因缺失能提高抗原特异性 CD8⁺ T 细胞进入肿瘤并在其中积累的能力；
2. 哪些基因缺失能提高肿瘤靶细胞刺激下的脱颗粒；
3. 两类功能 readout 的交集能否给出比单一筛选更可信的工程靶点；
4. 命中基因是否在人 T 细胞中仍有效，并能解释为具体分子通路。

## 2. 扰动与筛选框架

### 2.1 体内肿瘤浸润筛选

- 供体：表达 OVA 特异性 TCR 的 OT-I;Cas9 小鼠 CD8⁺ T 细胞；
- 文库：MKO 全基因组慢病毒 sgRNA 文库；
- 肿瘤：Rag1⁻/⁻ 小鼠乳腺脂肪垫中的 E0771-mCherry-OVA 三阴性乳腺癌模型；
- 选择：比较注射前 T 细胞与肿瘤内回收 TIL 的 sgRNA 丰度；
- 设计：每次感染使用超过 10⁸ 个 T 细胞，初始覆盖度大于 700×，通常有 3 个独立感染重复；论文展示 3 个 input 与 10 个肿瘤样本。

它测到的是“从制备到肿瘤内回收的综合适应度”，包含转导后扩增、体内存活、迁移、肿瘤浸润和局部扩增，不能只解释为“迁移能力”。

### 2.2 体外脱颗粒筛选

OT-I;Cas9 T 细胞与 SIINFEKL 脉冲的 E0771 细胞按 1:1 共培养，使用表面 CD107a 作为脱颗粒 readout，分选最高 5% 的 CD107a⁺ 细胞并测 sgRNA。三个生物学重复与体内筛选交叉后，Dhx37、Lyn、Odc1 同时命中。

### 2.3 命中后的验证链

作者依次进行了单基因编辑、肿瘤控制、流式细胞术、靶向扩增测序、bulk RNA-seq、TIL scRNA-seq、蛋白互作和 NF-κB 报告系统验证。这里的“多组学”不是同一细胞同时测量，而是针对同一扰动的跨实验层证据拼接。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

这篇论文公开数据由四层组成：

1. **pooled CRISPR screen count**：每个样本中 129,209 条 sgRNA 的计数、RPM/富集及基因排名；
2. **TIL 单细胞转录组**：Dhx37 敲除与 vector 对照的肿瘤浸润 OT-I 细胞；
3. **bulk 转录组**：Dhx37 缺失与对照 CD8⁺ T 细胞的表达差异；
4. **靶向 DNA 测序和 source data**：Cas9/Cas12a 编辑位点的 indel、流式与图中定量数据。

最关键的区分是：GEO 的 `GSE132960` 只统管转录组；全基因组筛选的归一化计数主要在论文 Supplementary Table 1，而非一个可直接载入 AnnData 的“全筛选单细胞对象”。

### 3.2 多大规模、覆盖哪些实验情境

| 数据层 | 规模/组成 | 生物学比较 |
|---|---:|---|
| MKO 文库 | 128,209 gene-targeting + 1,000 NTC | 全基因组敲除 |
| 体内 screen | 3 个 input；10 个肿瘤 | 注射前 vs 肿瘤内 OT-I TIL |
| 脱颗粒 screen | 3 个生物学重复；CD107a top 5% | 未分选/输入 vs 高脱颗粒细胞 |
| scRNA-seq | 552 cells；8,244 expressed genes | sgDhx37 191 vs vector 361 |
| bulk RNA-seq | 8 samples | Dhx37 KO 4 vs vector 4 |
| GSE132960 | 10 GEO sample records | 8 个 bulk + 2 个 scRNA 样本记录 |

论文用 10x Genomics v2 建库。552 是过滤后的细胞数，不能用 GEO 的“2 samples”代替；反过来，也不能把 552 说成供者或生物学重复。该单细胞数据用于观察 KO 后的表达状态差异，不足以建立一般性的 TIL 状态图谱。

### 3.3 公共数据包有什么

| 入口/文件 | 规模 | 内容与用途 |
|---|---:|---|
| [GSE132960](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE132960) | 10 samples | SuperSeries；用于统一定位两类转录组 |
| [GSE132926](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE132926) | 8 samples | bulk RNA-seq；Dhx37 KO 与 vector 各 4 |
| `GSE132926_norm-tpm.dhx.txt.gz` | 959.1 KB | 归一化 TPM 表；适合快速差异趋势与通路复核 |
| [GSE132959](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE132959) | 2 samples | Dhx37 与 vector TIL scRNA-seq |
| `GSE132959_TCell1_nonorm_matrix.rn.2.txt.gz` | 1.6 MB | 未归一化表达矩阵；需另核对细胞标签和基因行列方向 |
| Supplementary Table 1 | 约 12.1 MB xlsx | MKO 序列、各筛选样本归一化 read count；重做 screen 排名的核心 |
| Supplementary Tables 2–3 | xlsx | scRNA 表达/通路结果与小鼠、人 T 细胞验证数据 |
| Supplementary Tables 4–6 | xlsx | bulk TPM、差异表达、GSEA/DAVID 通路 |
| Supplementary Table 7 | xlsx | sgRNA、amplicon primer 等序列 |
| [PRJNA549266](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA549266) | SRA runs | DNA 层靶向测序原始 FASTQ |
| [Mendeley 2ffxmf3d7k.1](https://doi.org/10.17632/2ffxmf3d7k.1) | Excel/source data | qPCR、流式和图中非基因组数值 |

GEO 的原始 RNA-seq reads 可从每个子系列的 SRA Run Selector 获取；页面上的 processed TXT 很小，适合快速复核，不等同于原始 FASTQ。

### 3.4 如何获取：按目的选择

#### 路线 A：快速查看 DHX37 转录效应

直接从 GSE132926 下载 TPM 表，从 GSE132959 下载未归一化单细胞矩阵。用于复画差异表达、热图和状态 signature 时最省空间。

#### 路线 B：重做全基因组 screen

先从 PMC/Cell 补充材料下载 Supplementary Table 1；它包含文库序列与样本级归一化计数。若需从最原始 reads 开始，按论文 Data availability 和样本元数据进入相应 SRA，但要先确认 run 是 screen amplicon、RNA-seq 还是编辑位点测序。

#### 路线 C：重做 scRNA-seq 或 bulk RNA-seq

从 GSE132959 或 GSE132926 的 SRA Run Selector 导出 RunInfo/Accession List，再使用 SRA Toolkit：

```bash
prefetch SRRxxxxxxx
fasterq-dump --split-files SRRxxxxxxx
```

单细胞数据使用的是 10x v2；严格复现时应按样本页面确认每个 SRR 对应 gene-expression library，而不是仅下载处理矩阵。

#### 路线 D：复核图中数值与编辑率

非组学图用 Mendeley source data；编辑位点用 PRJNA549266；筛选 hit、差异表达和通路分别用 Supplementary Tables 1、5、6。不要把四种来源混成同一“GEO 数据集”。

### 3.5 下载后先做什么

1. 读取 Supplementary Table 1 的 sheet names，确认 input、tumour 和 CD107a screen 的列；
2. 检查 sgRNA 序列是否唯一，分开 128,209 gene-targeting 与 1,000 NTC；
3. 用 raw counts 重新做 library-size normalization，并在基因聚合前查看单 guide 一致性；
4. 单细胞矩阵核对行列方向、细胞条形码、KO/对照标签及是否已有 QC；
5. bulk TPM 可用于展示，但正式差异统计应优先从 raw counts/FASTQ 重建，而不是对 TPM 直接套计数模型。

## 4. 主要发现

体内 screen 能无偏重现 `Pdcd1` 和 `Havcr2` 等已知免疫检查点，说明选择压力确实与抗肿瘤 T 细胞功能有关。体内浸润与体外脱颗粒交集得到 Dhx37、Lyn、Odc1；后续研究集中于此前缺乏 T 细胞功能注释的 RNA helicase DHX37。

Dhx37 缺失增强：

- 肿瘤内 T 细胞积累和肿瘤控制；
- CD107a 脱颗粒、IFN-γ/TNF 等细胞因子；
- 小鼠和人原代 CD8⁺ T 细胞的抗原响应；
- NF-κB 相关转录程序。

作者发现 DHX37 与 PDCD11、NF-κB p65 发生联系，支持其通过核糖体/RNA 相关蛋白网络影响 NF-κB，而不是把 DHX37 简化为一个新的表面 checkpoint。

## 5. 从扰动到状态转换的证据链

本研究的逻辑是：`Dhx37 KO → NF-κB 活性改变 → 激活/效应基因上升 → 脱颗粒与细胞因子增强 → 肿瘤内积累和治疗效果改善`。其中前半段来自 bulk/scRNA 和生化实验，后半段来自功能及体内肿瘤实验。

scRNA 显示的是扰动后状态分布和表达差异，不能证明每个细胞沿某条连续轨迹从“低效应”转成“高效应”；没有谱系条形码或实时成像来确定转换方向。

## 6. 推荐图版

- **Fig. 1**：全基因组体内浸润筛选设计及 PD-1/Tim-3 等正对照命中；适合介绍“体内选择压力”。
- **Fig. 2**：CD107a 脱颗粒筛选与两种 screen 的交集；最适合讲正交 readout。
- **Fig. 3**：Dhx37 KO TIL 的 552-cell scRNA-seq；适合说明早期单细胞扰动读出。
- **Fig. 6**：bulk 转录组和 NF-κB 机制；适合“分子 driver—功能”部分。

如果只能选一组，建议 Fig. 1 + Fig. 2；若综述强调 molecular driver，再加 Fig. 6。

## 7. 创新价值

1. 将全基因组 CRISPR 筛选直接放进抗原特异性 CD8⁺ T 细胞的实体瘤 ACT 场景。
2. 用肿瘤浸润和脱颗粒两个功能选择压强求交集，降低单一 readout 假阳性。
3. 从 pooled screen 一直推进到人 T 细胞、单细胞表达与分子互作验证。
4. 证明“可工程化靶点”不必是经典表面受体，也可以是 RNA/核糖体相关调节因子。

## 8. 局限性

1. 体内 screen 使用 OVA、OT-I、Rag1⁻/⁻ 和单一小鼠乳腺癌模型，抗原、宿主免疫和克隆多样性高度简化。
2. 肿瘤内 sgRNA 富集混合了迁移、存活、增殖和回收效率，不能归为单一表型。
3. 文库在体内出现 bottleneck；高覆盖 input 不保证每个肿瘤中每条 guide 都有足够细胞。
4. 单细胞数据仅 552 cells，且不是全文库 Perturb-seq。
5. DHX37 缺失伴随检查点表达上升，说明“高激活”不等于长期无耗竭。
6. 人 T 细胞验证以体外为主，不能直接推出临床安全性和持续性。

## 9. 对本综述架构的作用

该文最适合放在“the techniques to perturb/manipulate cell states”与“link cell state/function transitions with molecular drivers”之间：它展示了如何先用体内功能筛选寻找 navigation handle，再用转录组和生化实验解释该 handle 如何把细胞推向更强效应状态。

它不属于 live-cell tracking，也没有建立实时闭环优化系统；它提供的是离线的“扰动—选择—测序—验证”导航范式。

## 10. 可直接用于综述的观点

> Dong 等将 129,209 条 sgRNA 的全基因组文库导入 OT-I;Cas9 CD8⁺ T 细胞，在 E0771-OVA 肿瘤中进行体内浸润筛选，并与 CD107a 脱颗粒筛选交叉定位 DHX37；后续转录组和生化分析表明，Dhx37 缺失可通过 NF-κB 相关程序增强 T 细胞效应功能，说明体内功能选择可将细胞状态调控因子直接连接到治疗表型（Cell 2019, Dong）。

## 11. 避免误读

- 不要把 129,209 写成靶基因数；它是 sgRNA 数。
- 不要把“肿瘤内富集”直接写成“迁移增强”。
- 不要把 552-cell scRNA-seq 称为全基因组 Perturb-seq。
- 不要声称全部 screen 原始计数都在 GSE132960；核心 screen count 在补充表。
- 不要把 OVA/OT-I 小鼠模型的效果直接等同于多克隆人肿瘤 TIL。
