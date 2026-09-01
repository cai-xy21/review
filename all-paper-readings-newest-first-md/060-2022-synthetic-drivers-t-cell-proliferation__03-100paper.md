# 《A genome-scale screen for synthetic drivers of T-cell proliferation》精读

## 论文信息

- 作者：Mateusz Legut、Zoran Gajic、Maria Guarino 等
- 期刊：*Nature*
- 年份：2022；603: 728–735；在线发表于 2022 年 3 月 16 日
- DOI：10.1038/s41586-022-04494-7
- 原文：[Nature](https://www.nature.com/articles/s41586-022-04494-7)
- PubMed：[PMID 35296855](https://pubmed.ncbi.nlm.nih.gov/35296855/)
- 免费全文：[PMC9908437](https://pmc.ncbi.nlm.nih.gov/articles/PMC9908437/)
- 全部新数据：[GEO GSE193736](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE193736)

## 一句话结论

近 12,000 个全长人 ORF 的 barcoded gain-of-function screen 在 3 名供者的 CD4⁺/CD8⁺ T 细胞中寻找合成增殖驱动因子，并以 OverCITE-seq、RNA-seq 和 ATAC-seq解析命中；LTBR 过表达通过持续激活 canonical NF-κB 建立天然淋巴细胞中不存在的合成状态，增强增殖、细胞因子、抗凋亡及 CAR-T/γδ T 功能。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| ORF library | 近 12,000 个 full-length genes | 每个 ORF 平均约 6 个 DNA barcode；screen 单位首先是 barcode，不是 sgRNA |
| 主 screen donors | 3 名健康供者 | CD4 与 CD8 分开筛；14 天培养后 CFSE 再刺激 |
| screen readout | prestim、presort、CFSE-low | 捕获 TCR 刺激后的高增殖；不是 knockout |
| focused validation | 33 个 ORF | screen-independent donors；表型、cytokine 和生存验证 |
| OverCITE-seq | 约 30 个 ORF；1 名供者 | GEX + ADT + HTO + ORF + TCR；作者列出的质控后 32 类共 4,312 cells |
| 单细胞加载 | 30,000 cells 上机 | 加载数不等于最终分析细胞数；最终逐 ORF 计数合计 4,312 |
| GSE193736 | 44 GEO sample records | 7 screen + 5 single-cell library types + 24 bulk RNA + 8 ATAC |
| processed files | screen、bulk、ATAC counts + RAW.tar | 单细胞多模态处理 CSV 多在 sample supplementary/RAW.tar；原始 reads 在 SRA |

## 1. 研究要解决的问题

多数 T 细胞 CRISPR screen 是 loss-of-function，只能寻找“删除后更好”的负调节器。作者提出相反问题：是否可以把一个正常不在 T 细胞表达、或表达不足的完整人基因装入 T 细胞，创造新的、有治疗价值的合成程序？

研究依次解决：

1. 哪些 ORF 足以增强 CD4⁺ 和 CD8⁺ T 细胞增殖；
2. 命中是否同时改善激活、细胞因子和抗凋亡；
3. 单细胞多模态能否把每个 ORF 与表达、表面蛋白和 TCR 信息对应；
4. 最强命中能否增强 CAR-T 和广谱肿瘤反应性 γδ T 细胞。

## 2. 筛选与 OverCITE-seq 框架

### 2.1 barcoded ORF screen

CD4⁺/CD8⁺ T 细胞分别来自 3 名健康供者。转导 ORF 文库并选择阳性细胞，培养 14 天后 CFSE 标记并用 CD3/CD28 再刺激，比较 CFSE-low 高增殖与 presort/prestim 中每个 ORF barcode 的丰度。

每个基因约 6 个独立 barcode 是重要的内部重复；一个 ORF 的排名应来自多个 barcode 一致富集，而不是单个异常 barcode。

### 2.2 OverCITE-seq

作者构建 Overexpression-compatible CITE-seq：除了 10x 5′ gene expression 外，同时捕获：

- HTO：样本/哈希标签；
- ADT：表面蛋白；
- ORF transcript：扰动身份；
- αβ TCR；
- GEX：全转录组。

约 30 个 ORF 分别转导、选择后混池，以较低转导率减少多 ORF。该设计把“遗传操纵—表面 phenotype—转录状态—克隆受体”连接到单细胞，但只有 1 名健康供者。

### 2.3 LTBR 机制验证

LTBR 通常不在淋巴细胞表达。过表达后，作者用 bulk RNA-seq、ATAC-seq、NF-κB 通路干预、慢性刺激、CAR-T 和 γδ T 细胞验证其作用，证明它建立的是 synthetic state，而非简单放大已有 TCR signal。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

`GSE193736` 把四类新数据放在一个 Series：

1. ORF screen barcode counts；
2. OverCITE-seq 五种 library type；
3. LTBR vs tNGFR、CD4/CD8、rest/stim 的 bulk RNA-seq；
4. LTBR vs tNGFR、rest/stim 的 ATAC-seq。

GEO sample record 的“一个 sample”有时是一类单细胞 library（如 `sc_GEX` 或 `sc_ORF`），不是一个独立供者。因此不能从 44 samples 直接推断 biological n。

### 3.2 多大规模、覆盖哪些实验情境

| 数据块 | GEO records | 具体组成 |
|---|---:|---|
| ORF screen | 7 | plasmid；CD4 prestim/presort/CFSE-low；CD8 prestim/presort/CFSE-low |
| OverCITE-seq | 5 | HTO、ADT、ORF、GEX、TCR 各 1 个 library record |
| bulk RNA-seq | 24 | 2 cell types × 2 ORFs（LTBR/tNGFR）× 2 states（rest/stim）× 3 replicates |
| ATAC-seq | 8 | 2 ORFs × 2 states × 2 replicates；以 CD8 T 细胞为主 |
| 合计 | 44 | GEO Series 全部 sample records |

OverCITE-seq 论文图中逐 ORF 质控后细胞数合计 **4,312 cells**，覆盖 32 个标签（包括 31 个候选/对照类别；tNGFR 计数最高 355）。作者实验上向 Chromium 加载 30,000 cells；加载数、捕获数和最终质控数必须分开报告。

### 3.3 公共数据包有什么

| 文件/资源 | 体积 | 内容与用途 |
|---|---:|---|
| `GSE193736_ORF_screen_counts.csv.gz` | 2.0 MB | plasmid/prestim/presort/CFSE-low 的 barcode count；复核主 screen |
| `GSE193736_bulkRNAseq_counts.csv.gz` | 698.4 KB | 24-sample bulk gene counts |
| `GSE193736_ATAC_counts.csv.gz` | 1.8 MB | ATAC peak × sample count matrix |
| `GSE193736_RAW.tar` | 13.1 MB | sample-level processed CSV，包括单细胞多模态导出；不是 sequencing raw FASTQ |
| SRA Run Selector | 44 sample records 关联 runs | 真正原始测序 reads；按 assay 分类下载 |
| Supplementary Tables workbook | 约 21.6 MB xlsx | ORF/barcode 注释、screen 排名、guide/primer、差异结果等 |
| Single Cell Portal SCP424 | 外部 PBMC reference | 论文用来比较的公开数据，不是本研究产生的 OverCITE-seq |

`RAW.tar` 是 GEO 页面“custom TAR (of CSV)”的处理数据集合，名称中的 RAW 不应自动解释为 FASTQ。真正 raw reads 在 SRA。

### 3.4 如何获取：按目的选择

#### 路线 A：复核 ORF screen

下载 `GSE193736_ORF_screen_counts.csv.gz` 和补充工作簿。先建立 barcode→ORF 映射，再比较 CFSE-low 与 presort/prestim，并检查同一 ORF 的多个 barcode 是否一致。

#### 路线 B：复核 LTBR 转录/染色质机制

下载 bulk 和 ATAC count matrix。bulk 设计可写成：

```r
~ cell_type + stimulation + transgene +
  stimulation:transgene + cell_type:transgene
```

若供者/批次信息存在于 sample metadata，还应纳入设计；不要把 3 个 replicate 自动当 3 名供者。

#### 路线 C：复核 OverCITE-seq

下载 `GSE193736_RAW.tar` 并按 sample accession 整理 GEX、ADT、HTO、ORF、TCR 的 barcode。五种 modality 通过 cell barcode 连接，任何一类 barcode 前缀或 suffix 不一致都可能造成大量丢配。

#### 路线 D：从原始 reads 重建

在 GSE193736 的 SRA Run Selector 中按 assay/exported sample 分组。OverCITE-seq 使用 10x 5′，还含自定义 ORF capture；不能只按标准 Cell Ranger GEX 流程忽略 custom library。

### 3.5 下载后先做什么

1. 解压 `RAW.tar` 后列出文件名和对应 GSM，建立 modality manifest；
2. screen count 做 library-size QC、barcode dropout 与 ORF 内 barcode concordance；
3. 单细胞先做 HTO demultiplex，再做 ORF assignment 和 doublet/multiple-ORF 排除；
4. 对 ADT、GEX 和 ORF capture 分别归一化，不要把 UMI 混为同一量纲；
5. OverCITE-seq 只有一名供者，差异表达以 cell-level 描述为主，避免伪重复；
6. ATAC 与 RNA 的 replicate/condition 要从 GSM 名称严格配对。

## 4. 主要发现

主 screen 找到多类可增强 T 细胞增殖的合成驱动因子，包括 MAPK3、CD59、BATF、IL12B、IL23A 和 LTBR。top 33 ORF 的 arrayed validation 显示不少候选还增强激活和 cytokine。

LTBR 最具概念性：它在天然淋巴细胞中几乎不表达，但转入 T 细胞后通过 canonical NF-κB 引起广泛转录与染色质重塑，提高效应功能并增强抗凋亡。LTBR 及其他命中还能改善 CD19 CAR-T 和 γδ T 细胞对肿瘤靶细胞的反应。

## 5. 状态与分子 driver

LTBR 说明 T 细胞状态导航不仅是沿天然分化轨迹移动，也可以通过 ectopic receptor 构建新的状态空间。其因果链为：

`LTBR ectopic expression → constitutive canonical NF-κB → chromatin/transcriptome remodelling → activation/cytokine/anti-apoptosis → improved antigen-specific function`。

这类 synthetic state 的优势是功能强，风险是可能脱离正常抗原门控。未来优化必须同时测 tonic signaling、耗竭、细胞因子毒性和存活，而不能只最大化增殖。

## 6. 推荐图版

- **Fig. 1**：近 12,000 ORF 的 CD4/CD8 screen；适合作为 gain-of-function 方法图。
- **Fig. 2–3**：top ORF validation 和 cytokine；适合 phenotype quantification。
- **Fig. 4**：OverCITE-seq 多模态扰动；本综述最推荐。
- **Fig. 5**：LTBR 的 NF-κB、RNA/ATAC 机制。
- **Fig. 6**：CAR-T 与 γδ T 转化验证。

若只能选一张，选 Fig. 4；若强调 synthetic state，选 Fig. 5。

## 7. 创新价值

1. 从 loss-of-function 跳到近 genome-scale full-length ORF gain-of-function。
2. 同时覆盖 CD4 和 CD8，且每个 ORF 有多 barcode 内部重复。
3. 开发 OverCITE-seq，将 ORF、GEX、ADT、HTO 和 TCR 放入同一单细胞对象。
4. 提出“引入非 T 细胞天然基因可创建治疗性合成状态”的工程范式。

## 8. 局限性

1. screen readout 主要是 CFSE proliferation，可能偏向快速扩增而非长期持久或安全。
2. ORF 大小和慢病毒包装/表达效率会造成文库偏差。
3. OverCITE-seq 只有 1 名健康供者，最终每个 ORF 的 cells 不多。
4. LTBR 的持续 NF-κB 可能引起 tonic activation、炎症或失控存活。
5. CAR/γδ 验证仍是体外及前临床层级。
6. GSE 将多种 library type 混在一个 Series，若只看 44 samples 很容易错误计算独立样本数。

## 9. 对本综述架构的作用

该文非常适合“the techniques to perturb/manipulate cell states”：它提供了一种不同于 KO 的导航算子——将完整合成基因程序写入 T 细胞。OverCITE-seq 又可支撑“quantitatively characterizing phenotypes/functions/markers”。

对于“real-time optimization”，该研究仍是终点测序；但 ORF identity 与多模态状态的配对数据可用来训练离线 surrogate model，为后续闭环筛选提供先验。

## 10. 可直接用于综述的观点

> 近 12,000 个全长 ORF 的 gain-of-function screen 证明，T 细胞可通过引入天然不表达的信号受体获得合成状态；LTBR 过表达以 canonical NF-κB 为核心重塑转录组和染色质，增强增殖、效应和抗凋亡，并可通过 OverCITE-seq 在单细胞层连接 ORF、表面蛋白、转录组与 TCR（Nature 2022, Legut）。

## 11. 避免误读

- 不要把约 12,000 ORF 写成 CRISPR sgRNA 文库。
- 不要把 30,000 上机细胞写成最终分析细胞；论文列出的最终逐 ORF 细胞合计 4,312。
- 不要把 GSE193736 的 44 records 当作 44 独立供者。
- 不要把 `GSE193736_RAW.tar` 当作原始 FASTQ；raw reads 在 SRA。
- 不要把持续 NF-κB 与临床安全的持久功能等同。
