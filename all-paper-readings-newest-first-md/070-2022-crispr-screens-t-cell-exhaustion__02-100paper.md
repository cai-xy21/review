# 《Genome-wide CRISPR screens of T cell exhaustion identify chromatin remodeling factors that limit T cell persistence》精读

## 论文信息

- 作者：Julia A. Belk、Weida Yao、Nhi Ly 等
- 期刊：*Cancer Cell*
- 年份：2022；40(7): 768–786.e7；在线发表于 2022 年 6 月 23 日
- DOI：10.1016/j.ccell.2022.06.001
- 原文：[Cancer Cell](https://doi.org/10.1016/j.ccell.2022.06.001)
- PubMed：[PMID 35750052](https://pubmed.ncbi.nlm.nih.gov/35750052/)
- 免费全文：[PMC9949532](https://pmc.ncbi.nlm.nih.gov/articles/PMC9949532/)
- 组学总入口：[GEO GSE203593](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE203593)
- 公开代码：[GitHub](https://github.com/juliabelk/sarscov2_chirp_ms)

## 一句话结论

全基因组慢性刺激筛选和小鼠/人肿瘤体内验证共同表明，cBAF 与 INO80 染色质重塑复合体限制 T 细胞持久性；48-guide 体内 direct-capture Perturb-seq 在 70,646 个 TIL 中解析各复合体的不同转录作用，并显示 Arid1a 缺失维持效应程序、抑制终末耗竭染色质建立。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| genome-wide library | 90,230 sgRNA | 小鼠 Cas9 CD8⁺ T 细胞；论文 Fig. 2 的两次 screen replicate 合并排名 |
| in vitro model | acute IL-2 vs chronic anti-CD3 + IL-2 | Day 4 分流，Day 10 readout；是慢性刺激模型，不是自然肿瘤 TIL |
| top-hit mini-pool | 300 genes | 用于急性/慢性复核并进入 MC-38、B16-OVA 等体内筛选 |
| human mini-pool | 48 sgRNA，20 genes + 8 controls | 2 donors × 2 mice/donor × 2 guides/target；A375/NY-ESO-1 TCR 模型 |
| Perturb-seq pool | 48 sgRNA，9 genes + 12 single-targeting controls | cBAF、INO80、Pdcd1、Gata3 等；不是全基因组单细胞 screen |
| Perturb-seq cells | 70,646 QC cells | 52,607（74.4%）有高置信 sgRNA；6 个细胞状态 cluster |
| GSE203591 | 42 ATAC-seq samples | 7.1 GB bigWig TAR；小鼠与人、体内与体外多组比较 |
| GSE203592 | 16 Perturb-seq samples | 2.7 GB integrated Seurat `.rds.gz`；raw data 在 SRA |
| screen counts | 论文 Supplementary Tables | GEO 主要是 ATAC/Perturb-seq；screen count/z-score 不应在 GEO 里找 |

## 1. 研究要解决的问题

耗竭不只是抑制受体高表达，而是慢性抗原驱动的稳定转录与表观遗传重塑。作者要解决：

1. 如何建立适配 pooled CRISPR 的 T 细胞慢性刺激模型；
2. 哪些基因缺失可使细胞在慢性刺激下持续增殖/存活，却不只是普遍提高急性扩增；
3. 命中是否在小鼠实体瘤和人 T 细胞中仍成立；
4. 染色质重塑因子如何改变 TIL 的效应—耗竭状态及开放染色质。

## 2. 筛选与多组学框架

### 2.1 体外耗竭模型与 genome-wide screen

小鼠 CD8⁺ T 细胞先激活，随后在第 4 天分为：

- acute：只保留 IL-2；
- chronic：持续 anti-CD3 + IL-2，每两天换新的包被板。

第 10 天比较 sgRNA 丰度。90,230-guide screen 以慢性/急性 LFC 识别在持续 TCR 压力下选择性提高适应度的扰动。模型在 inhibitory receptors、cytokine defect 和 ATAC motif progression 上与体内终末耗竭相似。

### 2.2 从 300-gene mini-pool 到体内

作者把 top 300 genes 组成 mini-pool，先区分“只在 chronic 获益”与“一般性增殖优势”，再在 MC-38 和 B16-OVA TIL 中筛选。cBAF 与 INO80 复合体成员反复命中，Arid1a 成为重点。

### 2.3 direct-capture Perturb-seq

第三个 micro-pool 包含 48 条 sgRNA：靶向 9 个基因，包括 cBAF 的 `Arid1a/Arid1b/Smarcc1/Smarcd2`，INO80 的 `Actr5/Ino80c`，以及 `Pdcd1/Gata3` 等对照，并含 12 条 single-targeting negative controls。肿瘤回收 TIL 后同时测 sgRNA 与 10x 单细胞转录组。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

该研究的公开数据分两大体系：

1. **补充表中的筛选矩阵**：genome-wide screen、300-gene mini-pool、小鼠体内 screen、人源 mini-pool 的 counts、LFC 与 z-score；
2. **GEO 中的机制组学**：ATAC-seq 和 focused in vivo Perturb-seq。

`GSE203593` 不是 genome-wide screen 的 SuperSeries，而是 ATAC-seq + Perturb-seq 的组合入口。全基因组 screen 的核心 count/z-score 位于 manuscript supplemental data。

### 3.2 多大规模、覆盖哪些生物情境

| 层级 | 规模/内容 | 应如何理解 |
|---|---:|---|
| genome-wide in vitro screen | 90,230 sgRNA；2 replicates | acute vs chronic Day 10 |
| focused mini-pool | 300 genes | 复核 chronic specificity 并进入小鼠肿瘤 |
| mouse in vivo screens | MC-38、B16-OVA；tumour/spleen/input | 测持久性和组织选择，不是单细胞 |
| human in vivo mini-pool | 48 guides，20 genes，8 controls | 1G4 TCR 人 T 细胞；2 donors；每供者 2 mice |
| Perturb-seq | 70,646 QC cells | 52,607 high-confidence sgRNA；6 clusters |
| Perturb-seq states | 6 | T_EM、T_ISG、T-4-1BB、T_EX Prog、T_EX Term、Cycling |
| GSE203591 | 42 samples | ATAC-seq；体外时间、Arid1a/CTRL、人/鼠等 |
| GSE203592 | 16 samples | focused Perturb-seq 10x GEX/guide libraries |

70,646 是全部高质量表达细胞；52,607 才是可做高置信 perturbation-to-state 归属的细胞。后者仍需按 guide/target 和样本构建 pseudobulk，不能把单细胞直接当独立重复。

### 3.3 公共数据包有什么

| 文件/资源 | 体积 | 内容与用途 |
|---|---:|---|
| [GSE203593](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE203593) | 58 sample records | SuperSeries；42 ATAC + 16 Perturb-seq |
| [GSE203591](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE203591) | 42 samples | ATAC-seq 子系列 |
| `GSE203591_RAW.tar` | 7.1 GB | 各样本 bigWig；适合浏览轨迹和重新定量，原始 reads 另从 SRA |
| [GSE203592](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE203592) | 16 samples | Perturb-seq 子系列 |
| `GSE203592_integrated_v2.rds.gz` | 2.7 GB | 作者整合的 Seurat 对象；最快复核 Fig. 6 |
| `GSE203593_RAW.tar` | 7.1 GB | SuperSeries 页面聚合的 bigWig TAR；与 GSE203591 内容存在重叠，不要重复下载两份 |
| Supplementary Table 1 | xlsx | genome-wide guide counts/排名 |
| Supplementary Tables 2–5 | xlsx | mini-pool、复合体、人源 screen 等结果；具体 sheet 以表内 guide 为准 |
| [GitHub](https://github.com/juliabelk/sarscov2_chirp_ms) | scripts | 作者引用的公开 screen 分析脚本；仓库名来自既有项目，应固定 commit |

GEO 页面注明 raw sequencing data 在 SRA。`RDS` 是处理对象，不包含原始 FASTQ，也不保证保留所有 read-level 信息。

### 3.4 如何获取：按目的选择

#### 路线 A：快速复核 Perturb-seq

下载 `GSE203592_integrated_v2.rds.gz`，在 R 中读取：

```r
library(Seurat)
obj <- readRDS(gzcon(file("GSE203592_integrated_v2.rds.gz", "rb")))
obj
table(obj$seurat_clusters)
```

先查看 `meta.data` 中真实的 guide、target、sample 和 cluster 字段，不要按论文文字自行猜列名。

#### 路线 B：复核 ATAC 轨迹

只看 locus 或 motif 时下载 GSE203591 的 bigWig/处理结果；要重做 peak calling 时从 SRA 下载 FASTQ。SuperSeries 与 SubSeries 的 7.1 GB TAR 可能指向同一批文件，先比对文件名/MD5，避免重复占用约 14 GB。

#### 路线 C：重做 screen 排名

从论文 Supplementary Tables 读取 genome-wide 和 mini-pool count/z-score；screen scripts 使用 GitHub 固定 commit。论文代码入口并不是专为此研究单独命名的仓库，运行前应确认脚本、输入列名与本论文补充表匹配。

#### 路线 D：从 raw Perturb-seq 重建

从 GSE203592 的 SRA Run Selector 获取 accession，按 10x GEX 和 direct-guide capture 分组运行 Cell Ranger/相应 feature-barcode pipeline，再复现 70,646→52,607 的 QC 和 guide assignment。

### 3.5 下载后先做什么

1. RDS 解压后会显著大于 2.7 GB，预留内存与磁盘；
2. 检查 70,646/52,607 两个层级能否从 metadata 重现；
3. 对每条 guide 统计细胞数，验证同一基因多 guide 的一致性；
4. 将 sample/replicate 纳入 pseudobulk 或混合模型；
5. 对 ATAC 区分 raw FASTQ、bigWig、peak set 与 count matrix；
6. screen enrichment 先确认比较方向是 chronic/acute、tumour/input 还是 tumour/spleen。

## 4. 主要发现

慢性刺激 screen 富集多个表观遗传因子，其中 cBAF 与 INO80 复合体在体外和肿瘤中均限制 T 细胞持久性。Arid1a 缺失：

- 提高慢性刺激下的扩增与存活；
- 降低 PD-1/Tim-3 等耗竭相关表型；
- 在 MC-38 模型中改善肿瘤清除与生存；
- 在人原代 T 细胞和 A375/NY-ESO-1 模型中表现保守；
- 抑制获得终末耗竭相关开放染色质，尤其是 AP-1 motif-associated regulatory elements。

Perturb-seq 还显示 cBAF 与 INO80 并非同义的“抗耗竭”模块：不同复合体成员改变不同基因程序和状态比例。

## 5. 状态与分子 driver

本文把状态转换拆成三层：

1. 慢性 TCR 选择下的 persistence；
2. 单细胞层面的 T_EM/T_EX Prog/T_EX Term/ISG/4-1BB/Cycling 组成；
3. 与终末耗竭对应的染色质可及性获得。

因此，ARID1A 的证据比“降低某个 checkpoint marker”更强：它同时影响细胞适应度、转录状态和表观遗传轨迹。但 Perturb-seq 是单时间点，不能单独确定每个细胞从 progenitor 到 terminal 的真实方向；方向主要来自既有生物学和状态排序。

## 6. 推荐图版

- **Fig. 1**：体外 chronic stimulation 与 ATAC 轨迹验证。
- **Fig. 2**：90,230-guide genome-wide screen；适合技术章节。
- **Fig. 3–4**：小鼠体内筛选和 Arid1a 验证；适合从筛选到疗效。
- **Fig. 6**：70,646-cell in vivo Perturb-seq；最适合本综述。
- **Fig. 7**：ARID1A 对耗竭染色质可及性的限制。

若只能选一张，选 Fig. 6；若强调 molecular mechanism，配 Fig. 7。

## 7. 创新价值

1. 用可规模化的 chronic-stimulation assay 连接 genome-wide screen 与耗竭生物学。
2. 通过 acute control 排除一般性增殖增强，突出 chronic-specific persistence。
3. 横跨小鼠/人、体外/体内、screen/Perturb-seq/ATAC 三层验证。
4. 将染色质重塑复合体定位为可导航的状态控制器，而非仅做 marker 描述。

## 8. 局限性

1. plate-bound anti-CD3 模型缺少真实 TME 的空间、代谢和细胞互作。
2. 体内 screen 仍使用模型抗原和移植瘤。
3. 48-guide Perturb-seq 是 focused micro-pool，不代表全 genome 的单细胞因果图谱。
4. ARID1A/BAF 是广泛染色质调节器，长期安全性、基因组稳定性和分化副作用需评估。
5. guide-level 细胞数不均衡会影响状态比例和差异表达。
6. 单时间点数据不能直接给出状态转移速率。

## 9. 对本综述架构的作用

该文可以作为“link cell state/function transitions with molecular drivers”的强案例：同一扰动在 pooled fitness、单细胞状态和 ATAC landscape 三个空间中都有可量化结果。它也提示导航目标应包括“阻止不利染色质吸引子形成”，而非仅短期提高 cytokine。

它尚不是实时系统，但 6-state Perturb-seq 图谱可用作离线状态模型，未来与活细胞 reporter/连续成像对接。

## 10. 可直接用于综述的观点

> 通过 90,230-guide 慢性刺激筛选、体内小鼠与人 T 细胞验证以及 70,646-cell direct-capture Perturb-seq，Belk 等将 cBAF/INO80 染色质重塑复合体连接到 T 细胞持久性和耗竭状态；其中 Arid1a 缺失既维持效应转录程序，也限制终末耗竭相关开放染色质的获得（Cancer Cell 2022, Belk）。

## 11. 避免误读

- 不要把 90,230 写成基因数；它是 sgRNA 数。
- 不要把 70,646 个细胞都说成有可靠 guide；高置信归属为 52,607。
- 不要把 48-guide Perturb-seq 称为 genome-wide Perturb-seq。
- 不要在 GEO 寻找全部 screen counts；它们主要在补充表。
- 不要把 chronic-stimulation persistence 与临床长期持久性画等号。
