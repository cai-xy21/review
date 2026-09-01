# 《In vivo CRISPR screening reveals nutrient signaling processes underpinning CD8+ T cell fate decisions》精读

## 论文信息

- 作者：Hongling Huang、Peipei Zhou、Jun Wei 等
- 期刊：*Cell*
- 年份：2021；184(5): 1245–1261.e21；在线发表于 2021 年 2 月 25 日
- DOI：10.1016/j.cell.2021.02.021
- 原文：[Cell](https://doi.org/10.1016/j.cell.2021.02.021)
- PubMed：[PMID 33636132](https://pubmed.ncbi.nlm.nih.gov/33636132/)
- 免费全文：[PMC8101447](https://pmc.ncbi.nlm.nih.gov/articles/PMC8101447/)
- 论文正文引用的数据：[GEO GSE148681](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE148681)
- 更完整的后续 SuperSeries：[GEO GSE160341](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE160341)

## 一句话结论

3,017-gene 代谢 CRISPR screen 在 LCMV 急性感染中直接比较 memory precursor 与 terminal effector 命运，揭示氨基酸转运及 GDP-fucose–POFUT1–Notch–RBPJ 轴控制 CD8⁺ T 细胞由高增殖中间态走向终末效应；Pofut1 缺失扩大效应池并产生功能强、可召回的记忆细胞。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| screen library | 3,017 genes × 6 guides + 1,000 NTC | 两个 sublibrary；每个各 3 guides/gene + 500 NTC |
| screen recipients | 60 mice，3 pooled replicates | 每 replicate 合并回收；不是 60 个独立 count profiles |
| screen input/output coverage | 约 500× / >240× cells per sgRNA | output 分选 MP 与 TE；选择 readout 是命运比例而非总积累 |
| screen states | MP vs TE | MP=KLRG1⁻CD127⁺；TE=KLRG1⁺CD127⁻，后续又细分 T_INT/TE′ |
| GSE148681 | 28 microarray samples | 论文 Data and Code Availability 明示入口，但只覆盖早期 microarray dataset1 |
| GSE160341 | 40 samples，4 subseries | 后续完整入口：NICD/Pofut1 microarray、scRNA、Pofut1 ATAC |
| scRNA-seq | 8 GEO samples；目标 9,000 GEM/sample | sgPofut1/spike 与 NICD/vector；最终 QC cell 总数不应按 8×9,000 推算 |
| ATAC-seq | 8 samples；300M reads/sample 目标 | Pofut1 KO vs spike；处理 count matrix 1.7 MB |
| 公开入口存在版本/登记差异 | GSE148681 vs GSE160341 | 精读报告必须同时记录，避免只下载 28 个 microarray 样本 |

## 1. 研究要解决的问题

急性感染后，活化 CD8⁺ T 细胞分为 terminal effector（TE）与 memory precursor（MP），但代谢/营养信号如何决定两条命运并不清楚。作者希望：

1. 在生理性抗原和体内炎症环境中筛选代谢调节因子；
2. 同时测每个扰动对 MP 和 TE 的影响，而非只测总扩增；
3. 找到连接营养物质、糖基化和命运转录程序的因果轴；
4. 证明扰动后生成的 memory 不只是 marker 改变，而具有召回、杀伤和抗肿瘤功能。

## 2. 筛选与命运解析框架

### 2.1 in vivo MP/TE screen

Cas9-P14 CD8⁺ T 细胞转导代谢文库后进入 LCMV Armstrong 感染小鼠。第 7.5 天分选：

- MP：KLRG1⁻CD127⁺；
- TE：KLRG1⁺CD127⁻；
- input：转移前文库。

比较 `MP/input`、`TE/input` 及 `MP/TE` 可区分：提高总体扩增、偏向 memory、偏向 terminal effector 的不同 perturbation effect。

### 2.2 dual-colour validation

每个候选的 sgRNA-Ametrine 细胞与 sgNTC-GFP/mCherry spike 细胞按 1:1 共转入同一宿主，以控制宿主差异。这种同宿主竞争比跨鼠比较更敏感，但 reporter、转导效率和初始比例仍需校正。

### 2.3 POFUT1–Notch 机制

作者通过 microarray、scRNA、ATAC、NICD overexpression 和 Notch1/2/Rbpj perturbation，把 GDP-fucose synthesis、POFUT1-mediated O-fucosylation 与 Notch–RBPJ activity 连接到 T_INT→TE′ 分化。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

公开数据有两个登记批次：

1. **GSE148681**：论文正文明确引用，28 个 Clariom S microarray 样本，比较 sgPofut1/spike（day 5、7.5）以及 T_INT、TE′、MP；
2. **GSE160341**：与同一 PMID/论文关联的后续完整 SuperSeries，包含 NICD/Pofut1 microarray、scRNA 和 ATAC。

因此，严格描述应写：论文 Data and Code Availability 标为 GSE148681，但 GEO 中与该文关联的多组学完整包实际还包括 GSE160341。两者不是同一 Series 的不同版本，必须分别下载。

### 3.2 多大规模、覆盖哪些生物情境

| 入口 | 样本数 | 组成 |
|---|---:|---|
| [GSE148681](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE148681) | 28 | sgPofut1/spike day 5/7.5；T_INT、TE′、MP，各多重复 |
| [GSE160218](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE160218) | 8 | NICD vs empty/spike microarray，day 7.5，4+4 |
| [GSE160225](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE160225) | 16 | WT/Pofut1-null × empty/NICD，4 repeats/cell |
| [GSE160305](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE160305) | 8 | scRNA：sgPofut1/spike（3+3）及 NICD/vector（1+1） |
| [GSE160313](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE160313) | 8 | Pofut1 ATAC：KO/spike，4+4 |
| GSE160341 | 40 | 上述 4 个子系列总计 |

scRNA 每个样本目标 9,000 GEM、约 50,000 reads/cell；ATAC 目标 300 million reads/sample。这些是建库/测序目标，不是最终 QC 后规模。

### 3.3 公共数据包有什么

| 文件/资源 | 体积 | 内容与用途 |
|---|---:|---|
| `GSE148681_RAW.tar` | 35.2 MB | 28 个 CEL；sample table 含 RMA processed expression |
| `GSE160341_RAW.tar` | 721.6 MB | CEL + scRNA/ATAC TAR 聚合；可与子系列文件重复 |
| `GSE160218_RAW.tar` | 9.6 MB | NICD microarray CEL |
| `GSE160225_RAW.tar` | 20.4 MB | Pofut1/NICD 2×2 microarray CEL |
| `GSE160305_RAW.tar` | 691.6 MB | 8 个 scRNA sample 的处理 TAR；raw FASTQ 在 SRA |
| `GSE160313_Pofut1_ATAC_summarizedCounts.tsv.gz` | 1.7 MB | 8-sample ATAC summarized peak count matrix |
| Supplementary Table 1 | xlsx | MP/TE screen 结果、显著基因及转录比较 |
| Supplementary Table 8 | xlsx | 3,017-gene library/sgRNA 等方法级信息 |
| 其他 Supplementary Tables | xlsx | metabolite、signature、差异表达、验证；按 workbook guide 定位 |

GSE160305 的原始 reads 总量较大；BioProject 页面报告约 466 Gbases、约 0.12 TB。处理 TAR 约 692 MB，更适合先做快速复核。

### 3.4 如何获取：按目的选择

#### 路线 A：复核命运分群和 Pofut1/NICD

下载 GSE148681 的 series matrix/CEL、GSE160218/160225 的 matrix，以及 GSE160305 处理 TAR。这样可重画 MP/T_INT/TE′ signatures 与 scRNA UMAP。

#### 路线 B：复核染色质机制

直接下载 `GSE160313_Pofut1_ATAC_summarizedCounts.tsv.gz` 做 differential accessibility；若要重做 alignment/peak calling，再从 SRA 获取 FASTQ。

#### 路线 C：重做 pooled screen

screen counts/hit ranking 主要在 Supplementary Tables，而不是上述 GEO Series。先读取 Table 1/8，明确 input、MP、TE 三类列和两个 sublibrary，再以 3 个 pooled replicates 作为统计单位。

#### 路线 D：严格对应论文 Data Availability

在方法复现记录中同时写：`GSE148681`（正文声明）和 `GSE160341`（GEO 后续完整 SuperSeries）。仅引用后者会与正文不一致，仅引用前者会漏掉 scRNA/ATAC。

### 3.5 下载后先做什么

1. 建立两个 Series 的统一 sample manifest，避免把同一条件重复计数；
2. microarray 统一 RMA/probe annotation，检查 GSE148681 与 GSE160xxx 是否使用同一平台 `GPL23038`；
3. scRNA 检查最终细胞数、样本批次和 sgPofut1/NICD 标签；
4. ATAC 确认 genome build 与 peak coordinate；
5. screen 分开比较 MP/input、TE/input、MP/TE，不要只用一个 ratio；
6. 以 pooled replicate 而不是 cell 或 mouse 数作为 screen inferential n。

## 4. 主要发现

screen 识别了多种氨基酸转运和营养信号调节器。Pofut1 缺失提高早期 effector 增殖和代谢活性，扩大 effector pool，同时生成数量和质量更高的 memory cells，在二次感染和肿瘤模型中表现更强。

作者进一步定义：

- T_INT：高增殖、代谢活跃的中间态；
- TE′：KLRG1^hi CXCR3^lo CD127^lo 的更终末效应态；
- POFUT1/Notch 活性推动 T_INT→TE′；
- Pofut1 缺失或 Notch/Rbpj 抑制减少过早终末化；
- NICD 过表达促进 TE′，并可挽救 Pofut1-null 的表型。

## 5. 状态与分子 driver

该文最重要的因果链是：

`GDP-fucose availability → POFUT1-dependent Notch glycosylation → Notch–RBPJ activity → T_INT to TE′ terminal differentiation`。

Pofut1 缺失并非简单把细胞推回静息 memory，而是先保留/扩大代谢活跃的 effector pool，之后产生更强 memory。它揭示“最终细胞命运”取决于早期状态停留时间和转换速率，而不只是某一终点 marker。

## 6. 推荐图版

- **Fig. 1**：MP/TE in vivo metabolic screen；适合作为命运筛选方法图。
- **Fig. 4–5**：Pofut1 对 effector/memory 数量与功能的影响。
- **Fig. 6**：GDP-fucose–POFUT1–Notch–RBPJ 机制；本综述最推荐。
- **Fig. 7**：NICD scRNA 与 TE′ program；适合状态导航。

若只能选一张，选 Fig. 6；若强调单细胞状态变化，配 Fig. 7。

## 7. 创新价值

1. 把 pooled CRISPR 输出拆成 MP 与 TE 两个命运端点，而不是只测总细胞数。
2. 以同宿主 dual-colour competition 提高状态比较精度。
3. 将营养物质、蛋白糖基化、Notch transcription 与 T 细胞命运串联。
4. 同时验证 memory quantity、recall quality、杀伤和抗肿瘤功能。

## 8. 局限性

1. LCMV-P14 是单一 TCR/感染模型，未涵盖人 T 细胞异质性。
2. MP/TE 是流式离散门，真实命运连续且含 T_INT 等过渡态。
3. screen 样本为多鼠 pooling，个体差异不可估计。
4. scRNA 和 ATAC 是不同细胞、离线终点，不能直接测转换速率。
5. POFUT1 影响广泛糖基化底物，Notch 之外的机制不能完全排除。
6. 数据登记分散，若不核查 GEO 会误以为只有 GSE148681。

## 9. 对本综述架构的作用

该文最适合“link cell state/function transitions with molecular drivers”及“optimize the conditions for navigating T cell states”。它将一个可操作的营养/糖基化轴连接到早期中间态、终末效应化和最终记忆质量。

其数据可用于建立离线 trajectory/transition model，但缺少同一细胞的实时追踪；未来需要无损 reporter 直接测 Notch activity、代谢与 T_INT→TE′ 的转换。

## 10. 可直接用于综述的观点

> 在 LCMV 感染中按 MP 与 TE 命运分选的 3,017-gene CRISPR screen 揭示 GDP-fucose–POFUT1–Notch–RBPJ 轴推动高增殖 T_INT 向 TE′ 终末效应态分化；限制该轴可扩大效应池并产生具有更强召回和抗肿瘤能力的记忆 T 细胞（Cell 2021, Huang）。

## 11. 避免误读

- 不要只写 GSE148681；多组学完整入口还包括 GSE160341。
- 不要把 9,000 GEM/sample 写成最终细胞数。
- 不要把 60 只小鼠写成 60 个独立 screen replicate。
- 不要把 MP/TE 门视为全部状态结构；论文自身定义了 T_INT/TE′。
- 不要把 POFUT1 作用完全等同于 Notch，尽管 Notch–RBPJ 是核心机制证据。
