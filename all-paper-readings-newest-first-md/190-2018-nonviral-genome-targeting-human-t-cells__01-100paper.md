# 《Reprogramming human T cell function and specificity with non-viral genome targeting》精读

## 论文信息

- **题目**：Reprogramming human T cell function and specificity with non-viral genome targeting
- **作者**：Roth et al.
- **期刊与年份**：Nature，2018
- **DOI**：[10.1038/s41586-018-0326-5](https://doi.org/10.1038/s41586-018-0326-5)
- **PMID / PMC**：[29995861](https://pubmed.ncbi.nlm.nih.gov/29995861/) / [PMC6239417](https://pmc.ncbi.nlm.nih.gov/articles/PMC6239417/)
- **核心数据入口**：[GEO GSE108600](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE108600)、论文 Supplementary Tables、Addgene plasmids
- **研究类型**：CRISPR RNP + non-viral HDR donor 在人原代 T 细胞中进行大 DNA 片段定点 knock-in；流式、TLA、amplicon sequencing、CUT&RUN 与功能验证

## 一句话结论

本文证明人原代 T 细胞可在不依赖病毒载体的条件下，用 Cas9 RNP 和线性 DNA HDR template 高效完成大段定点 knock-in、multiplex editing、疾病突变纠正和 TCR specificity replacement，从而建立“按位点编程 T 细胞状态与特异性”的通用制造平台。

## 数据护照（先看这一表）

| 项目 | 内容 |
|---|---|
| 技术核心 | Cas9 RNP electroporation + non-viral dsDNA/ssDNA HDR template |
| 代表 knock-in | RAB11A-GFP，CD4/CD8 T 细胞可达约 50% |
| multiplex | 同时编辑 2 或 3 个基因位点 |
| 疾病纠正 | 3 名同一家系成员来源 T 细胞的 IL2RA mutation correction |
| TCR replacement | 约 1.5 kb NY-ESO-1 TCR cassette 定点整合至 TRAC；TCR-positive 细胞中最高约 70% dextramer-positive |
| 制造放大 | 6 donors，每名约 1×10^8 起始 T 细胞；10 d 后平均约 3.85×10^8 NY-ESO-1 TCR T cells/donor |
| 公共组学 | GSE108600，10 个 CUT&RUN 样本，主要用于 BATF/GFP-BATF 定位验证 |
| raw 组学 | PRJNA427801 / SRP127664；10 个 runs |
| processed 文件 | GSE108600_RAW.tar 约 5.2 GB，包含 10 个 BEDGRAPH.gz |
| 其他关键数据 | TLA 和 targeted amplicon sequencing 主要在文中/补充材料或可向作者索取；不是完整公共 sequencing repository |

## 1. 研究要解决的问题

病毒载体可高效导入 CAR/TCR，但随机整合、载量和制造复杂度限制了多位点、位点特异的细胞编程。早期 CRISPR 在原代 T 细胞中擅长 knockout，却难以把 kb 级功能 cassette 精确插入指定基因座。

本文试图建立一种不依赖病毒 donor 的通用 HDR 方法，使研究者能够：

- 在内源基因上加标签或功能模块；
- 同时改写多个位点；
- 修复患者突变；
- 用定点 TCR replacement 改变抗原特异性；
- 把反应从小规模 discovery 推进到细胞治疗相关制造规模。

## 2. 方法框架：RNP 与 HDR donor 的时序协同

### 2.1 核心流程

1. 激活人原代 T 细胞，使其进入适合 HDR 的状态。
2. 制备针对目标位点的 Cas9–sgRNA RNP。
3. 设计带左右 homology arms 的线性 dsDNA 或 ssDNA donor。
4. 电转 RNP 和 donor，使目标位点切割与 template 到达时间匹配。
5. 通过流式、junction PCR、amplicon sequencing 或 TLA 测量 knock-in 与整合特异性。

### 2.2 代表实验

- **RAB11A-GFP**：内源标签，约 50% knock-in，展示普适性。
- **multiplex knock-in**：同时处理 2–3 个位点，证明组合工程可行。
- **IL2RA repair**：用患者来源细胞展示临床相关突变纠正。
- **TRAC–TCR replacement**：将 NY-ESO-1 特异 TCR cassette 定点整合，重编程抗原识别。
- **GFP-BATF**：内源标签后用 CUT&RUN 验证融合蛋白保留预期染色质结合行为。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

本文是一篇 **engineering platform paper**，主要数据是：

- `locus × donor design × cell type × electroporation condition` 的 knock-in efficiency；
- multiplex editing 组合效率；
- junction/amplicon/TLA 的 on-target 与 off-target evidence；
- IL2RA correction 和 TCR replacement 的功能数据；
- 10-sample CUT&RUN 组学验证。

它不是全基因组筛选，也不是 T 细胞 atlas。GSE108600 只覆盖 GFP-BATF/CUT&RUN 这一支验证，不能代表论文全部 genome-engineering data。

### 3.2 工程实验的规模与组成

#### RAB11A-GFP 与多位点编辑

- CD4 和 CD8 人原代 T 细胞均可进行。
- 代表性 RAB11A-GFP knock-in 约 50%。
- 作者测试多个 loci、不同 donor 格式和多重组合。
- 2-locus 与 3-locus editing 用于证明单细胞内复合基因程序安装的可行性。

#### IL2RA correction

- 细胞来源：同一受累家系的 3 名成员。
- 数据包含临床/基因型背景、校正设计、IL2RA expression 和功能恢复。
- 该部分样本数小，属于 proof-of-concept，不是临床队列。

#### TCR replacement

- donor cassette 约 1.5 kb。
- integration locus：TRAC。
- 目标：用 NY-ESO-1 specificity 替代/重定向 TCR。
- 在 TCR-positive population 中，NY-ESO-1 dextramer-positive 比例最高约 70%。

#### manufacturing-scale test

- donors：6。
- 每 donor 起始约 `1 × 10^8` T cells。
- 扩增 10 d。
- 平均获得约 `3.85 × 10^8` NY-ESO-1 TCR T cells/donor。

这部分说明方法有放大潜力，但不是 GMP release study，也没有建立临床批次一致性标准。

### 3.3 GSE108600：10 个 CUT&RUN 样本

GEO：[GSE108600](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE108600)

- 样本数：10。
- 物种：Homo sapiens。
- assay：CUT&RUN。
- 生物学目的：验证 endogenously tagged GFP–BATF 的染色质结合是否与 BATF/相关对照一致。
- 样本名包含 donor 1/donor 2、non-edited 或 edited 条件，以及 anti-BATF、anti-GFP、rabbit/matched controls 等组合；精确 mapping 应以 GEO GSM title/characteristics 为准。
- raw repository：BioProject [PRJNA427801](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA427801)，SRA Study SRP127664。
- runs：10。
- processed archive：`GSE108600_RAW.tar`，约 5.2 GB。
- archive 内容：10 个 `.BEDGRAPH.gz`，单文件约 284–754 MB。

这些 bedGraph 是 genome-wide signal tracks，体积远大于普通 count matrix。可用于浏览器可视化和 peak/coverage 比较，但不是细胞 × 基因矩阵。

### 3.4 TLA 与 amplicon sequencing 的边界

作者对 2 个 donors 的 representative edited cells 进行了 targeted locus amplification（TLA）等分析。论文报告在检测灵敏度约 1% allele frequency 下未见明确 off-target integration；同时指出 dsDNA donor 可发生低水平 homology-independent integration，使用 ssDNA donor 可降低该风险。

需要注意：

- TLA 和 amplicon 数据并未像 GSE108600 一样形成完整、统一的公共 accession；
- 一部分数据在 source/supplementary materials；
- 进一步原始数据需根据论文 Data availability 向作者请求。

因此不能用“GSE108600 已包含全部安全性测序”来描述本文。

### 3.5 Supplementary Tables 的组成

| 表 | 内容 | 规模要点 |
|---|---|---|
| Supplementary Table 1 | Pulse-code/electroporation optimizations | 约 25 行 × 3 列 |
| Supplementary Table 2 | antibodies 与 dextramers | 约 27 行 × 8 列 |
| Supplementary Table 3 | HDR templates、DNA primers、gRNAs | 约 979 行 × 28 列；最重要的工程资源表 |
| Supplementary Table 4 | IL2RA family clinical/genotype information | 约 10 行 × 6 列 |

Supplementary Table 3 包含大量长序列和设计字段，是复现实验的核心。读取时必须禁止 Excel 自动把基因名/序列解释为日期或科学计数法；导出 TSV 时所有 sequence columns 应强制为字符串。

### 3.6 下载方式

#### CUT&RUN processed tracks

```text
https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE108600
```

在 GEO 下载 `GSE108600_RAW.tar`。解压后可把 bedGraph 转为 bigWig：

```bash
bedGraphToBigWig sample.bedGraph hg38.chrom.sizes sample.bw
```

转换前需核对原文使用的 genome build，并保证 bedGraph 排序；不能默认 hg38。

#### raw reads

从 BioProject PRJNA427801 或 GEO 的 SRA link 获取 10 个 SRR：

```bash
prefetch SRR_ACCESSION
fasterq-dump SRR_ACCESSION --split-files --threads 8
```

#### Nature supplementary tables

Supplementary Table 1 的文件路径模式为：

```text
https://media.springernature.com/original/springer-static/esm/art%3A10.1038%2Fs41586-018-0326-5/MediaObjects/41586_2018_326_MOESM3_ESM.xlsx
```

后续 table 使用相邻 MOESM 编号。建议从 Nature 页面点击下载，以避免出版社调整文件映射。

#### plasmids

论文 Data availability 指向 Addgene。使用论文页面给出的 plasmid links/IDs 下载 sequence map 或申请材料；申请实体质粒涉及第三方条款和费用，不属于数据下载。

### 3.7 下载后建议整理

```text
65_Roth_2018/
├── engineering_designs/
│   ├── hdr_templates.tsv
│   ├── guide_sequences.tsv
│   └── primers.tsv
├── efficiency_tables/
├── il2ra_correction/
├── tcr_replacement/
├── safety/
│   ├── TLA_summary/
│   └── amplicon_summary/
├── cut_and_run/GSE108600/
│   ├── bedgraph/
│   ├── fastq/
│   └── sample_manifest.tsv
└── plasmid_resources/
```

## 4. 主要技术与生物学发现

### 4.1 大片段 non-viral knock-in 在原代 T 细胞中可达到实用效率

Cas9 RNP 与线性 HDR donor 的组合突破了“原代 T 细胞只能高效 knockout”的限制。RAB11A-GFP 等实验说明 kb 级 cassette 可定点安装。

### 4.2 可同时重写功能与抗原特异性

TRAC 定点 TCR replacement 不只是添加一个 transgene，而是把 receptor specificity 与内源位点调控结合，为更一致的 receptor expression 提供基础。

### 4.3 多重编辑打开组合状态工程空间

2–3 位点同时 knock-in 表明多个状态调节模块有机会在同一细胞中组合，但效率、细胞损伤和产品异质性会随位点数增加。

### 4.4 donor 格式影响安全性

dsDNA donor 可能出现低水平非同源整合；ssDNA 可降低风险。这说明“knock-in efficiency 最大化”不能是唯一优化目标，必须同时评价错误整合。

## 5. 状态—功能—驱动因子的连接

本文更像“状态执行器”而非“状态发现器”：

```text
chosen molecular program
→ locus-specific HDR installation
→ controlled receptor/protein expression
→ antigen recognition or signaling-state change
→ T-cell function
```

它为综述中由 atlas/CRISPR screen 找到 driver 之后，如何把 driver 精确安装进 T 细胞提供技术桥梁。

## 6. 对细胞治疗状态导航的启示

- 可将状态调节器安装到特定内源位点，利用原有转录调控。
- knock-in、knockout 和 receptor replacement 可组合。
- donor format、pulse code、cell activation state 都是可优化的控制变量。
- 需要把 on-target efficiency、viability、genomic integrity 和最终功能作为联合 objective。

## 7. 可复用的分析思路

1. 用 flow cytometry 同时报导 `%KI among live cells` 与 cell recovery，避免只看阳性率。
2. 对 5′/3′ junction、full-length cassette 和 allele distribution 分别质控。
3. TLA/amplicon safety evidence 应注明检测下限。
4. multiplex editing 应在单细胞或克隆层确认共现，而不能用各位点群体阳性率相乘推断。
5. CUT&RUN 用于验证内源标签未明显改变蛋白定位，是工程后功能等价性的好范例。

## 8. 推荐图版

- non-viral HDR workflow。
- RAB11A-GFP knock-in efficiency 与多位点编辑图。
- IL2RA correction proof-of-concept。
- TRAC–NY-ESO-1 TCR replacement 与 dextramer staining。
- manufacturing-scale expansion。
- TLA/off-target 与 dsDNA/ssDNA donor 安全性对比。

## 9. 创新价值

- 建立人原代 T 细胞高效大段 non-viral knock-in。
- 将内源 tagging、突变修复和 TCR replacement 统一到同一平台。
- 证明组合/多位点工程与制造规模放大的可能性。
- 为后续 pooled knock-in screens 奠定技术基础。

## 10. 局限性

1. 论文不是无偏 screen，不能提供哪些状态 driver 最优的答案。
2. GSE108600 只覆盖 CUT&RUN 验证，公共原始数据不完整覆盖 TLA/amplicon/flow。
3. dsDNA 非同源整合和多位点 translocation 风险仍需更高灵敏度检测。
4. IL2RA correction 仅 3 名家系成员，属于 proof-of-concept。
5. 制造放大不是完整 GMP 工艺验证。
6. 高 knock-in 百分比不等于所有编辑细胞均具有相同 cassette copy number 和功能。

## 11. 对本章节的作用

| 综述模块 | 本文贡献 |
|---|---|
| Chart molecular landscape | 贡献有限；只有 CUT&RUN 验证而非 atlas |
| Quantify phenotype/function | 流式 KI、dextramer specificity、功能与规模化产量 |
| Perturb/manipulate states | non-viral targeted knock-in、repair、multiplex、TCR replacement |
| Link transitions with drivers | 提供把已知 driver 定点安装并验证功能的执行平台 |
| Optimize navigation | donor format、pulse、位点、cell state 的多变量优化 |

## 12. 可直接用于综述的观点

> Non-viral homology-directed repair turns primary human T cells from knockout-compatible targets into programmable substrates for locus-specific installation of large functional cassettes.

> State navigation requires an execution layer: drivers discovered by omics or screening must be installed with control over genomic position, expression and product identity.

> Editing efficiency should be optimized jointly with cell recovery, genomic integrity and functional specificity.

## 13. 避免误读

- **本文不是 genome-wide screen，也不是 T-cell atlas。**
- **GSE108600 只有 10 个 CUT&RUN 样本**，主要验证 GFP-BATF，不是全部编辑实验数据。
- **约 70% 是 TCR-positive population 中的 dextramer positivity 代表值**，不应写成所有起始细胞的绝对产率。
- **TLA 未检出 off-target 有检测限约束**，不能表述为绝对零风险。
- **dsDNA 与 ssDNA donor 的安全性不同**，不能只比较 knock-in efficiency。
- **六名 donor 的放大实验不等同于临床制造放行验证。**

## 数据与资源链接

- 论文：[Nature](https://www.nature.com/articles/s41586-018-0326-5)
- 全文：[PMC6239417](https://pmc.ncbi.nlm.nih.gov/articles/PMC6239417/)
- GEO：[GSE108600](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE108600)
- BioProject：[PRJNA427801](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA427801)
