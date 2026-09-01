# 《A human memory T cell subset with stem cell–like properties》精读

## 论文信息

- 作者：Luca Gattinoni、Enrico Lugli、Yong Ji 等
- 期刊：*Nature Medicine*
- 年份：2011；17: 1290–1297；发表于 2011 年 9 月 18 日
- DOI：10.1038/nm.2446
- 原文：[Nature Medicine](https://www.nature.com/articles/nm.2446)
- PubMed：[PMID 21926977](https://pubmed.ncbi.nlm.nih.gov/21926977/)
- 开放全文：[PMC3192229](https://pmc.ncbi.nlm.nih.gov/articles/PMC3192229/)
- 表达数据：[GEO GSE23321](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE23321)
- BioProject：[PRJNA131239](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA131239)

## 一句话结论

该文在人外周血中定义了 CD45RO⁻CCR7⁺CD45RA⁺CD62L⁺CD27⁺CD28⁺IL-7Rα⁺CD95⁺ 的 TSCM，并用 3 位供者、4 个 CD8 亚群的 Affymetrix 数据证明其转录位置介于 TN 与 TCM 之间；GEO 实际有 12 个物理 CEL 文件，但以 gene/exon 两套分析视图登记为 24 个 GSM。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 流式发现队列 | 29 位健康供者 | TSCM 约占循环 CD4/CD8 T 细胞 2%–3% |
| 转录组供者 | 3 | 4 亚群配对设计：TN、TSCM、TCM、TEM |
| 物理芯片 | 12 个 CEL | 3 donors × 4 subsets |
| GEO Samples | 24 GSM | 同 12 个 CEL 分别映射为 exon-level 和 gene-level，不能误写成 24 生物样本 |
| 平台 | Affymetrix Human Gene 1.0 ST | GPL10739 exon-level；GPL6244 gene-level |
| 处理矩阵 | 253,002 exon probesets × 12；28,869 gene probesets × 12 | 当前 GEO matrix 压缩约 10 MB 与 1.2 MB |
| 原始文件 | 12 个 `.CEL.gz`，每个约 4.0–4.3 MB | GEO `suppl/` 逐文件下载，无单一 RAW.tar |
| 差异基因 | 四亚群 900 genes；565 呈单调分化趋势 | 阈值 P<0.01、FDR<5%；pairwise 另加 >2-fold |

## 1. 研究要解决的问题

在人类中是否存在比 TCM 更早、更具自我更新和多向分化能力的记忆 T 细胞？如果存在，怎样从表面 marker、抗原经历、转录组、功能和体内重建能力上把它与真正 naïve T cell 区分开。

## 2. 研究设计与方法框架

### 2.1 状态定义

作者先用 TWS119/Wnt 条件生成 naïve-like memory 候选，再在人 PBMC 中寻找相同表型。最终以严格的 7 个 naïve marker 加 CD95 区分：

- TN：CD3⁺CD8⁺CD45RO⁻CCR7⁺CD45RA⁺CD62L⁺CD27⁺CD28⁺IL-7Rα⁺CD95⁻；
- TSCM：上述 marker 相同但 CD95⁺；
- TCM：CD45RO⁺CD45RA⁻CCR7⁺CD62L⁺；
- TEM：CD45RO⁺CD45RA⁻CCR7⁻CD62L⁻。

### 2.2 多层证据

- 多参数流式测频率和 BCL-2、IL-2Rβ、CXCR3、LFA-1 等；
- TREC 和 TCRβ 克隆追踪证明抗原经历与长期克隆维持；
- SEB、CD3/CD2/CD28、IL-15/IL-7 测快速细胞因子、增殖、自我更新与多向分化；
- 4 亚群 whole-transcriptome microarray 构建分化顺序；
- NSG 人源化模型比较组织重建；
- mesothelin CAR + M108 mesothelioma 比较抗肿瘤效力。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

论文主体是多实验、多供者的经典免疫学证据集合；公开且机器可读的核心数据仅是 **3 位健康供者的 12 张 bulk microarray**。流式 FCS、TCR 序列、NSG 重建和肿瘤 BLI 没有作为独立公共数据集上传。

因此，应区分：

1. 论文的总体证据规模（29 位供者流式、多个小型功能实验和小鼠组）；
2. GEO 可重分析规模（3 donors × 4 sorted subsets）。

### 3.2 各实验层规模

| 实验层 | 规模 | 内容 |
|---|---:|---|
| 循环频率 | 29 健康供者 | TSCM 约占 2%–3% |
| TREC | 4 donors | 证明 TSCM 已经历分裂 |
| SEB cytokine | 6 donors | IFN-γ、IL-2、TNF-α 与 polyfunctionality |
| IL-15 proliferation | 9 donors | 10 d CFSE，比较四亚群 |
| 抗原特异 | 健康供者 + 11 melanoma patients | 7/11 melanoma 患者有 MART-1-specific CD95⁺ naïve-like cells |
| TCRβ longitudinal | 1 CMV donor，22 months | 两个免疫优势 clonotypes 的长期分布 |
| microarray | 3 donors × 4 subsets | TN/TSCM/TCM/TEM 配对 |
| in vitro stemness | 4–8 donors，视 panel 而定 | IL-15 自我更新、TCR 刺激多向分化 |
| NSG reconstitution | 6 mice/subset，2 experiments | 脾、LN、肝的人 CD8 恢复 |
| 肿瘤模型 | 多组 NSG | mesothelin-CAR TSCM/TCM/TEM，文中未给所有组统一 n |

### 3.3 GEO 为什么是 24 samples 但只有 12 个 CEL

[GSE23321](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE23321) 将同一 Affymetrix Human Gene 1.0 ST 芯片按两种平台注释登记：

- GPL10739：probe set/exon-level；12 GSM；
- GPL6244：transcript/gene-level；12 GSM；
- 每个原始 CEL 文件名同时包含一个 exon GSM 和一个 gene GSM，例如 `GSM572410_GSM572422`。

生物设计仍是：

| Donor | TN | TSCM (`Tcd95`) | TCM | TEM |
|---|---|---|---|---|
| 1 | 1 array | 1 | 1 | 1 |
| 2 | 1 | 1 | 1 | 1 |
| 3 | 1 | 1 | 1 | 1 |

所以 24 GSM 是两个分析视图，不能当作技术重复或 24 个独立样本。

### 3.4 可下载文件与尺寸

| 文件/目录 | 规模 | 用途 |
|---|---:|---|
| `GSE23321/suppl/` | 12 个 `.CEL.gz`，每个 4.0–4.3 MB，总计约 49 MB | 原始 Affymetrix 强度；最适合统一重处理 |
| `GSE23321-GPL10739_series_matrix.txt.gz` | 约 10 MB；253,002 features × 12 samples | exon/probeset-level 处理矩阵 |
| `GSE23321-GPL6244_series_matrix.txt.gz` | 约 1.2 MB；28,869 features × 12 samples | gene/transcript-level 处理矩阵 |
| SOFT/MINiML family | 元数据 | 样本—平台—文件关系 |
| 论文 Supplementary PDF | 约 5.5 MB | Figures 1–15、Tables 1–9、Methods；不是原始数据 |

GEO 页面标注“processed data included within Sample table”。当前 series matrix 文件的修改时间可能晚于论文发表，这是 GEO 再生成文件的时间，不代表生物数据后来新增。

### 3.5 如何下载

#### 路线 A：处理后的 gene-level 矩阵

```bash
wget -c https://ftp.ncbi.nlm.nih.gov/geo/series/GSE23nnn/GSE23321/matrix/GSE23321-GPL6244_series_matrix.txt.gz
```

适合快速复核 TN→TSCM→TCM→TEM 排序和 marker；但需确认矩阵是否已经 RMA、probe-to-gene 映射版本及重复基因处理。

#### 路线 B：exon-level 矩阵

```bash
wget -c https://ftp.ncbi.nlm.nih.gov/geo/series/GSE23nnn/GSE23321/matrix/GSE23321-GPL10739_series_matrix.txt.gz
```

适合研究 exon/probeset 层变化，但 253,002 features 对 3 donors 极易过拟合。

#### 路线 C：原始 CEL

浏览：

`https://ftp.ncbi.nlm.nih.gov/geo/series/GSE23nnn/GSE23321/suppl/`

目录中逐个下载 12 个 CEL.gz；文件名含 `#`，脚本 URL 中应编码为 `%23`。该系列没有可用的单一 `GSE23321_RAW.tar`，批量下载可递归抓取目录后按 `.CEL.gz` 过滤。

### 3.6 下载后先做什么

原始数据建议用 `oligo`/`affy` 和固定版本注释包做 RMA，并以 donor 为 blocking factor。四亚群来自同一供者，应使用配对/重复测量设计，而不是普通两组 t test。先确认 12 个物理 CEL，不要把 gene/exon 两套 GSM 合并成 24 行。

## 4. 主要发现

1. TSCM 在健康循环 CD4/CD8 中约占 2%–3%，表型 naïve-like 但表达 CD95、IL-2Rβ、BCL-2、CXCR3 和 LFA-1。
2. TSCM 的 TREC 低、能快速释放 IFN-γ/IL-2/TNF-α、对 IL-15 强增殖，支持已经历抗原而非真正 naïve。
3. 900 个基因在四亚群间差异，565 个沿 TN→TSCM→TCM→TEM 单调变化。
4. TN 与 TSCM 仅 75 个 >2-fold 差异基因；TSCM 与 TCM 仅 20 个，说明 TSCM 位于早期记忆位置。
5. TSCM 具更强自我更新、多向分化和 NSG 重建能力。
6. mesothelin CAR-TSCM 在 M108 模型中优于 CAR-TCM/TEM，显示起始状态决定治疗效力。

## 5. 关键 marker 与状态定义

- 核心鉴别：CD95 将 TSCM 从严格 naïve-like gate 中分出；
- 支持 marker：IL-2Rβ/CD122、BCL-2、CXCR3、LFA-1；
- naïve/干性转录：LEF1、FOXP1 等较高；
- 随分化上升：EOMES、TBX21、PRDM1、GZMA、PRF1、KLRG1；
- 随分化下降：LEF1、FOXP1、CERS6。

CD95 只有在严格的 naïve-like 多 marker gate 内才有意义；单独 CD95⁺ 不能定义 TSCM。

## 6. 对细胞状态导航的意义

该文定义了细胞治疗的“优质起点”：状态越早并不等于功能更弱，而是保留更大扩增、自我更新和后续分化空间。它还给出可操作导航逻辑：通过 Wnt/培养条件保持 naïve-like phenotype，再用 CD95 与功能/转录证据确认已经进入记忆而非停留在 TN。

## 7. 推荐图版

- **Fig. 1**：TSCM gating 与 29 供者频率，适合作为定义图。
- **Fig. 2**：抗原经历、细胞因子、增殖和 TCR 克隆证据。
- **Fig. 3**：四状态转录顺序，适合 molecular landscape。
- **Fig. 4–5**：stemness、重建和抗肿瘤功能。

如果只能选一张，选 Fig. 3；若章节强调治疗起始状态，选 Fig. 1 + Fig. 5。

## 8. 创新价值

1. 首次系统定义人 TSCM 的可分选表型。
2. 用表型、克隆、转录、功能和体内模型形成多证据链。
3. 提出 TN→TSCM→TCM→TEM 的早期记忆层级。
4. 直接连接起始亚群与 CAR-T 抗肿瘤效力。
5. 公开可重分析的配对 microarray 数据。

## 9. 局限性

1. 公开转录组只有 3 donors，统计功效有限。
2. microarray 动态范围和探针注释不及 RNA-seq。
3. 许多功能实验的供者集合不同，不能把所有证据视为同一队列多组学。
4. NSG xenogeneic 环境会驱动异常分化/GVHD，不等于人内稳态。
5. CD95 gating 对仪器、阈值和 naïve gate 纯度敏感。
6. 流式 FCS、TCR 原始序列和动物源数据未公开。

## 10. 对本综述章节的作用

这篇应作为“t cell is at the start point and the frontier of cell therapy”与“optimize conditions for navigating states”的奠基文献：它定义了一个兼具早期状态和记忆功能的治疗起点，并提供可重分析的分子坐标。

## 11. 可直接用于综述的观点

> 人 TSCM 位于严格 naïve-like 表型内但表达 CD95，并在转录组上介于 TN 与 TCM 之间；其更强的自我更新、组织重建和 CAR 抗肿瘤能力使“保留早期记忆状态”成为细胞治疗制造的核心原则（Nature Medicine 2011, Gattinoni）。

## 12. 数据复用建议

GSE23321 最适合构建早期 T 细胞状态 signature、验证后续 scRNA-seq atlas 的 TN/TSCM/TCM 顺序。建模时应做 leave-one-donor-out，且优先使用 12 个原始 CEL 统一处理；不要将两个平台视图重复纳入。

## 13. 避免误读

- 不要把 GEO 的 24 GSM 写成 24 位供者或 24 个物理芯片。
- 不要只用 CD95 定义 TSCM；必须放在严格 naïve-like gate 内。
- 不要把 NSG 中的优越性直接等同于患者临床疗效。
- 不要把 TSCM 当成完全静止或未经历抗原的 naïve 细胞。
