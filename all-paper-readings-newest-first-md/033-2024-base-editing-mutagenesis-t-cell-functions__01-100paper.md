# 《Base-editing mutagenesis maps alleles to tune human T cell functions》精读

## 论文信息

- 作者：Ralf Schmidt、Carl C. Ward、Rama Dajani 等
- 期刊：*Nature*
- 年份：2024；625: 805–812；在线发表于 2023 年 12 月 13 日
- DOI：10.1038/s41586-023-06835-6
- 原文：[Nature](https://www.nature.com/articles/s41586-023-06835-6)
- PubMed：[PMID 38093011](https://pubmed.ncbi.nlm.nih.gov/38093011/)
- 免费全文：[PMC11065414](https://pmc.ncbi.nlm.nih.gov/articles/PMC11065414/)
- 原始 screen 数据：[GEO GSE244774](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE244774)
- 代码与 figure-level 数据：[Zenodo 10.5281/zenodo.8415038](https://doi.org/10.5281/zenodo.8415038)

## 一句话结论

作者以 ABE/CBE 在原代人 T 细胞中对 385 个免疫功能基因实施 117,249-guide 编码区饱和式筛选，按 TNF、IFN-γ、CD25、PD-1 高低分箱，将基因级“敲除”推进到结构域/残基/等位基因级的功能地图，并发现 PIK3CD、VAV1、LCP2、PLCG1、DGKZ 等同一基因内可同时存在增强或抑制 T 细胞功能的变体。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| NGG library | 117,249 unique sgRNA | 385 genes + 1,000 NTC；覆盖 coding sequence/splice sites，不是 117,249 个独立等位基因 |
| base editors | ABE8e(V106W)、evoCDA1-BE4max | 每条 guide 可产生多个碱基/氨基酸结果；guide effect 不等于单一 variant effect |
| NGG screen donors | 3 名 | 原代人 CD4⁺ T cells；ABE/CBE；TNF、IFN-γ、CD25、PD-1 high/low |
| NG high-resolution library | 45,941 oligos，58 genes | SpG ABE、放宽为 NG PAM；平均每基因 guide 密度约提高 4 倍 |
| NG screen donors | 2 名 | CD4/CD8 TNF high/low；GEO 8 个样本记录 |
| GEO GSE244774 | 52 sample records | 44 个 NGG + 8 个 NG；全部为 guide-amplicon sequencing `Other` |
| processed GEO files | 4 个 count tables，共约 12.1 MB | NGG IFNG/CD25、PD1、TNFa；NG TNFa |
| Zenodo | 11 files，670,444,793 bytes | notebook、libraries、Supplementary Tables 1–8 文本版；不是 raw FASTQ |
| functional validation | 多基因、多 donor；1G4/A375 killing | 证明某些 allele 可双向调节 cytotoxicity，但仍是 arrayed subset |

## 1. 研究要解决的问题

普通 CRISPR KO screen 给出“哪个基因重要”，却无法回答“蛋白的哪个结构域、哪个残基、哪种等位基因能把功能调到理想水平”。细胞治疗工程常需要精细调节而非完全删除。

作者要建立一种 forward-genetic allele map：

1. 在原代人 T 细胞中高通量引入 A→G 或 C→T 类变体；
2. 将变体连接到 cytokine/activation phenotype；
3. 区分 loss-of-function、partial-function 和 gain-of-function；
4. 用更宽 PAM 提高结构域和 protein–protein interface 的解析度；
5. 将 screen hit 转化为可增强或抑制杀伤的工程 allele。

## 2. 碱基编辑筛选框架

### 2.1 NGG screen

作者从文献、GO 和既有 T 细胞 CRISPR screens 选择 385 genes，针对最长转录本的 coding exons 和 splice-site 邻域设计所有含可编辑碱基的 NGG PAM guides，加入 1,000 NTC，得到 117,249 条唯一 sgRNA。

原代人 CD4⁺ T 细胞转导 sgRNA 与：

- ABE8e(V106W)：主要 A→G；
- evoCDA1-BE4max：主要 C→T；

刺激后按 TNF、IFN-γ、CD25、PD-1 high/low 分选并测 guide abundance。3 名独立供者用于主 screen，screen 全程保持 >1,000× cells/sgRNA coverage，排序时目标 >500 cells/sgRNA/bin，测序目标约 1,000× read coverage/sample。

### 2.2 guide 到 allele 的映射

每条 guide 在编辑窗口内可能含多个 A/C，也可能产生多个 codon change。因此作者不仅做 guide-level MAGeCK，还将 effect 分配到：

- 可编辑碱基；
- 预测氨基酸替换；
- start-loss/splice-site 等 knockout-like guide；
- 蛋白结构域和三维结构位置。

这一步有不可辨识性：两个或多个碱基总由同一组 guides 覆盖时，统计 effect 无法完全拆开，作者会给它们相同估计。

### 2.3 NG PAM 高分辨率 screen

选择 58 个重点基因，用 SpG Cas9-ABE 支持 NG PAM，设计 45,941 个 oligos，使平均 guide 密度约提高 4 倍。2 名供者的 CD4/CD8 T 细胞以 TNF high/low 为 readout，用于更精细定位结构域和接口。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

本研究数据是 pooled guide-amplicon count，而不是 RNA-seq/scRNA-seq：

1. **GEO GSE244774**：52 个 high/low screen samples 的原始测序和 4 张处理 count matrix；
2. **Nature Supplementary Tables**：NGG/NG library、guide annotation、MAGeCK output、base-level statistics；
3. **Zenodo 8415038**：上述表的文本版、library 文件、figure notebook 和中间统计结果；
4. **Source data**：各图 arrayed validation 的流式、编辑率和 killing 数值。

没有单细胞转录组；“map alleles”指 pooled guide/variant-to-phenotype mapping。

### 3.2 多大规模、覆盖哪些生物情境

GEO 52 个 records 的构成为：

| 数据块 | 样本数 | 组成 |
|---|---:|---|
| NG screen | 8 | 2 donors × CD4/CD8 × TNF high/low |
| NGG PD-1 | 12 | 3 donors × ABE/CBE × high/low |
| NGG IFN-γ/TNF 主体 | 22 | donor/editor/readout 组合；个别 donor/editor/readout 组合未出现 |
| NGG CD25 | 10 | donor/editor high/low；并非所有 donor 都有 CBE |
| 合计 | 52 | 44 NGG + 8 NG |

GEO sample titles不是完整的笛卡尔积：例如 NGG IFN-γ 和 CD25 的某些 donor/CBE 组合缺失。分析时应以实际 GSM 列表为准，不能用“3 donors×2 editors×4 readouts×2 bins=48”自行补齐。

文库层：

| 文库 | genes | guides/oligos | 供者 |
|---|---:|---:|---:|
| NGG ABE/CBE | 385 | 117,249（含 1,000 NTC） | 3 |
| NG SpG-ABE | 58 | 45,941 | 2 |

### 3.3 GEO 公共数据包有什么

| 文件 | 体积 | 内容与用途 |
|---|---:|---|
| `GSE244774_BE022_NGG_IFNG_CD25.count.txt.gz` | 4.9 MB | NGG IFN-γ/CD25 screen counts |
| `GSE244774_BE022_NGG_PD1.txt.gz` | 3.3 MB | NGG PD-1 high/low counts |
| `GSE244774_BE022_NGG_TNFa.count.txt.gz` | 2.9 MB | NGG TNF screen counts |
| `GSE244774_BE026_NG_TNFa.count.txt.gz` | 956.5 KB | NG high-resolution TNF counts |
| SRA Run Selector | 52 records 关联 runs | guide amplicon FASTQ；不是 WGS 或 RNA-seq |

GEO processed counts 适合重做 guide enrichment；真正的 guide→base→protein annotation 在 Supplementary Tables/Zenodo。

### 3.4 Zenodo 8415038 数据包有什么

截至核查时，Zenodo record 含 **11 个文件，总计 670,444,793 bytes（约 670.4 MB）**：

| 文件 | 体积 | 内容与用途 |
|---|---:|---|
| `Figures.ipynb` | 97.9 MB | 关键图的分析/绘图 notebook，内含大量输出所以体积较大 |
| `libraries.tar.gz` | 43.1 MB | screen libraries/annotation 文件 |
| `README.txt` | 4.9 KB | 使用说明 |
| `Supplementary Table 1.txt` | 15.0 KB | NGG gene list |
| `Supplementary Table 2.txt` | 49.6 MB | NGG guide-level library annotation |
| `Supplementary Table 3.txt` | 219.8 MB | 全 screen MAGeCK/guide-level outputs；核心结果表 |
| `Supplementary Table 4.txt` | 2.3 KB | NG focused gene list |
| `Supplementary Table 5.txt` | 5.8 MB | NG guide library |
| `Supplementary Table 6 base_level_stats.txt` | 106.1 MB | base-level effect statistics |
| `Supplementary Table 7 base_level_stats_abe_3-9.txt` | 42.2 MB | ABE 扩展窗口 3–9 的 base-level statistics |
| `Supplementary Table 8 base_level_stats_filtered.txt` | 105.9 MB | 过滤后的 base-level statistics |

Zenodo 页面给出每个文件的 MD5；下载后应校验，尤其是 100–220 MB 的文本表。

### 3.5 如何获取：按目的选择

#### 路线 A：只看 screen hit

下载 GEO 的 4 个 count tables + Zenodo `Supplementary Table 3.txt`。前者用于重算，后者用于直接查询论文 MAGeCK 结果。

#### 路线 B：做结构域/残基图谱

下载 `libraries.tar.gz`、Tables 2、5、6–8 和 `Figures.ipynb`。仅有 GEO counts 无法知道每条 guide 覆盖的 transcript、codon、editable base 和 predicted consequence。

#### 路线 C：严格从 raw reads 重做

从 GSE244774 SRA Run Selector 获取 52 个 run。按 sample title 建立 `PAM × donor × subset × editor × phenotype × bin` manifest，提取 guide spacer counts，再与 Zenodo library 对齐。

#### 路线 D：只复核 arrayed allele

从 Nature source data 与 Supplementary Tables 9–11 获取具体 sgRNA、编辑率、PIK3CD 等位基因和功能 readout。screen count 不能替代 amplicon deep sequencing 对实际编辑产物的验证。

### 3.6 下载后先做什么

```python
import pandas as pd

tnf = pd.read_csv(
    "GSE244774_BE022_NGG_TNFa.count.txt.gz",
    sep="\t",
)
print(tnf.shape)
print(tnf.columns.tolist())
```

随后：

1. 检查 guide ID 能否与 Supplementary Table 2/5 一一对应；
2. 分开 NGG 与 NG，分开 ABE 与 CBE；
3. 对 high/low 做 donor-paired effect，再做跨 donor 聚合；
4. knockout-inducing guides 与 missense/synonymous guides分层；
5. 预测 variant effect 时显式保留一个 guide 多编辑产物的不确定性；
6. notebook 执行前清理/固定环境，并确认嵌入输出是否导致内存压力；
7. 大文本用分块读取或 Arrow/Polars，避免电子表格软件截断行数。

## 4. 主要发现

screen 在 PIK3CD、VAV1、LCP2、PLCG1、DGKZ 等核心 TCR signaling genes 中发现同时具有正/负功能效应的 guides。它们可对应：

- knockout-like loss-of-function；
- partial-function；
- gain-of-function；
- protein interface/structural hotspot。

在 1G4 TCR 的 NY-ESO-1/A375 系统中，候选 guides 能双向改变细胞因子和杀伤。PIK3CD Y524 周围的多个编辑揭示与已知 activated PI3Kδ syndrome 类似的 gain-of-function hotspots。NG PAM screen进一步提高了 PIK3R1 等结构域的解析度。

## 5. 状态与分子 driver

这篇论文把导航控制粒度从“gene on/off”推进到“allele tuning”。同一基因可出现方向相反的编辑，说明理想细胞状态不一定通过最大化或完全删除基因获得，而可通过选择特定功能增益/减弱 allele 调节信号增益。

概念上的控制链为：

`base edit → protein residue/domain/interface change → signalling gain → cytokine/activation marker distribution → cytotoxic function`。

但 pooled screen 的前半段是预测；真正从 base edit 到蛋白等位基因需要 arrayed amplicon sequencing 确认实际编辑组成。

## 6. 推荐图版

- **Fig. 1**：117,249-guide ABE/CBE screen 设计与 QC；适合方法页。
- **Fig. 2**：guide/base/ClinVar/structure-level functional map；本综述最推荐。
- **Fig. 3**：PIK3CD 等位基因及 T 细胞杀伤验证。
- **Fig. 4**：NG PAM 高分辨率 screen 和 protein interface。

若只能选一张，选 Fig. 2；若强调细胞治疗工程，配 Fig. 3。

## 7. 创新价值

1. 在原代人 T 细胞中实现超过 10 万 guides 的 base-editing forward genetics。
2. 从基因级 hit 扩展到 domain/residue/allele 级调控。
3. ABE/CBE 与 NGG/NG PAM 提供互补序列覆盖。
4. 将 screen allele 连接到 cytokine 和实时 Incucyte killing 功能验证。
5. GEO + Zenodo + source data 分工清晰，公开数据量和可复用性高。

## 8. 局限性

1. 编辑化学限制可实现的碱基替换，不是全氨基酸饱和突变。
2. bystander edits 使 guide effect 难归因到单一 variant。
3. PAM、编辑窗口和转录本选择造成覆盖偏差。
4. 主 screen 以 CD4 T 细胞和短期 TNF/IFN-γ/CD25/PD-1 为主，长期状态/持久性未直接测量。
5. 供者只有 3（NGG）和 2（NG）。
6. 功能验证只覆盖 screen hits 的小子集。
7. 增强信号的疾病相关 allele 可能同时增加免疫毒性或自主激活风险。

## 9. 对本综述架构的作用

该文适合“the techniques to perturb/manipulate cell states”和“optimize the conditions for navigating T cell states”：它说明导航手柄不必是 gene knockout，而可以是连续可调的 protein-function allele。

它还与 live-cell tracking 有一个接口：作者用 Incucyte 做连续 killing 验证，但 screen 本身是终点 FACS/测序。未来可把 allele library 与实时 killing/metabolic reporter 结合，进行闭环优化。

## 10. 可直接用于综述的观点

> 117,249-guide ABE/CBE screen 将 T 细胞工程从基因级删除推进到蛋白残基和等位基因级调节；同一 TCR signaling gene 内可存在方向相反的功能变体，而 PIK3CD 等位基因能在 arrayed 验证中双向调节 cytokine 与肿瘤细胞杀伤，显示细胞状态导航可通过精细调节信号增益而非简单 on/off 实现（Nature 2024, Schmidt）。

## 11. 避免误读

- 不要把 117,249 guides 写成 117,249 个已确认单一等位基因。
- 不要把一个 guide 的 phenotype 自动归因于一个碱基；可能有多个 editable bases/bystander edits。
- 不要把 GSE244774 称为 RNA-seq/scRNA-seq；它是 screen guide sequencing。
- 不要假定 52 个 GEO samples 构成完整平衡设计；实际有缺失组合。
- 不要把 disease gain-of-function allele 直接视为安全的治疗 allele。
