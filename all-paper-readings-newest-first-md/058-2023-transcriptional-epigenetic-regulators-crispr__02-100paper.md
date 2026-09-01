# 《Transcriptional and epigenetic regulators of human CD8+ T cell function identified through orthogonal CRISPR screens》精读

## 论文信息

- 作者：Sean R. McCutcheon、Amanda M. Swartz、Matthew C. Brown 等
- 期刊：*Nature Genetics*
- 年份：2023；55: 2211–2223；在线发表于 2023 年 11 月 9 日
- DOI：10.1038/s41588-023-01554-0
- 原文：[Nature Genetics](https://www.nature.com/articles/s41588-023-01554-0)
- PubMed：[PMID 37945901](https://pubmed.ncbi.nlm.nih.gov/37945901/)
- 主数据：[GEO GSE218988](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE218988)
- 分析代码：[Zenodo 10.5281/zenodo.8370763](https://doi.org/10.5281/zenodo.8370763)
- 外部临床比较数据：[GEO GSE197268](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE197268)

## 一句话结论

作者建立了可在原代人 CD8⁺ T 细胞中运行的正交 CRISPRi、CRISPRa 和 CRISPRko 筛选体系，以 CCR7 为状态读出系统评估 120 个转录/表观遗传调节因子，并通过约 12 万个 Perturb-seq 细胞、bulk RNA-seq、ATAC-seq 和 CAR-T 功能实验证明 BATF3 可增强记忆/持续性程序、压低耗竭与细胞毒程序并改善体内肿瘤控制。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 初始候选基因 | 120 个 TF/表观遗传调节因子 | 不是全基因组；是状态调节因子定向库 |
| CRISPRi/a 单细胞验证 | 每种模式约 60,000 个细胞，3 名供者 | 论文表述为每个 screen 约 6 万；GEO 为 6 个样本 |
| Perturb-seq 小库 | 32 条命中 gRNA + 8 条非靶向 gRNA，分别克隆进 CRISPRi/a | 单细胞内还需按 gRNA assignment 和每供者覆盖过滤 |
| 主 GEO SuperSeries | GSE218988，75 个 GSM | 实际由 4 个子系列组成，样本数不能当成供者数 |
| bulk RNA-seq | GSE218986，19 个 GSM | 包含 BATF3 OE 及后续 KO/OE 组合实验 |
| ATAC-seq | GSE218987，10 个 GSM | 急性刺激 3 供者、慢性刺激 2 供者，各有对照/BATF3 |
| bulk CRISPR counts | GSE241933，40 个 GSM | 包含 tiling、120基因 CCR7 筛选及 TFome KO |
| scRNA 原始/处理包 | GSE218985，6 个 GSM；RAW.tar 约 2.09 GB | 3供者 × CRISPRi/a；每个 GSM 另有约 310–408 MB tar.gz |
| 外部临床队列 | GSE197268，32 位 CAR-T 患者、109 个 GSM | 不是本研究产生的数据，仅用于临床相关性映射 |

## 1. 研究要解决的问题

细胞治疗效果不仅由受体构型决定，也受输入 T 细胞的转录和染色质状态支配。已有全基因组 CRISPRko 可以找到“删除后改善功能”的基因，却难以系统回答：某个内源转录因子被上调或下调时，T 细胞状态如何改变；这些变化是否能转化为抗耗竭与体内疗效。

论文用三种互补扰动回答这个问题：

1. CRISPRi：降低内源基因表达，寻找维持记忆/CCR7 状态的正调节因子；
2. CRISPRa：激活内源基因，寻找可主动编程有利状态的因子；
3. CRISPRko：在 BATF3 过表达背景和对照背景中寻找其辅因子及下游依赖。

## 2. 方法框架

### 2.1 原代 T 细胞中的表观编辑平台

作者以失活的 *Staphylococcus aureus* Cas9 构建 CRISPRi/a 编辑器，并先用 CD2、B2M 和 IL2RA 启动子 tiling 筛选确定适合 SaCas9 的 gRNA 位置与 PAM 规律。相比直接切断 DNA，CRISPRi/a 通过招募抑制或激活结构域调节内源转录，因而适合同时做相反方向的状态扰动。

### 2.2 状态筛选与多层验证

- 以 CCR7 高/低分选作为记忆样与效应样状态的代理读出；
- 对 120 个候选基因运行 CRISPRi 和 CRISPRa pooled screen；
- 将命中 gRNA 做小规模 Perturb-seq，直接读取每个扰动的全转录组效应；
- 重点验证 BATF3 OE 的 bulk RNA、急/慢性刺激 ATAC、耗竭标志物、CAR-T 体外杀伤及小鼠肿瘤控制；
- 用 TFome CRISPRko 在有/无 BATF3 OE 条件下寻找 JUNB、IRF4、ZNF217 等网络依赖。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

这不是传统的静态 T 细胞 atlas，而是一套“扰动—分子状态—功能”多层数据资源：

1. **pooled gRNA count 数据**：比较高/低表面标志物分选群或不同时间点中 gRNA 丰度；
2. **Perturb-seq 数据**：在单细胞层面同时获得转录组与 gRNA 身份；
3. **bulk RNA-seq**：量化 BATF3 OE 和后续组合编辑引起的平均表达变化；
4. **ATAC-seq**：比较急性或反复抗原刺激下 BATF3 对染色质可及性的影响；
5. **流式、细胞因子、杀伤及肿瘤体积数据**：把分子变化连接到 CAR-T 功能；
6. **外部临床 scRNA-seq**：只用于询问 BATF3 诱导程序是否与患者 CAR-T 扩增/结局相关。

因此，最小可分析单位随数据层而变：CRISPR counts 的单位是 gRNA×样本，Perturb-seq 是 cell×gene 并带 gRNA 标签，ATAC 是 peak×样本，体内实验则是 mouse×time。

### 3.2 GSE218988 的四层组成

| 子系列 | GSM数 | 生物设计 | 公开内容/用途 |
|---|---:|---|---|
| [GSE218985](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE218985) | 6 | 3供者 × CRISPRi/CRISPRa；约6万细胞/模式 | 10x 单细胞表达与 gRNA assignment；6个 tar.gz，合并 RAW.tar 约2.09 GB |
| [GSE218986](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE218986) | 19 | BATF3 OE 对照及 ZNF217/GATA3 等后续组合 | 原始测序经 SRA；GEO另给 TPM 表，适合直接复核 DE/热图 |
| [GSE218987](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE218987) | 10 | 急性刺激3供者、慢性刺激2供者；对照与 BATF3 OE | FASTQ/SRA + bigWig + narrowPeak；RAW.tar约1.48 GB |
| [GSE241933](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE241933) | 40 | CD2/B2M/IL2RA tiling、CCR7 CRISPRi/a、±BATF3 TFome KO | 8个核心压缩 counts CSV，可直接进入 DESeq2/MAGeCK 类分析 |

SuperSeries 显示 75 个 GSM，恰好是 6+19+10+40。它们不是 75 名供者，也不是 75 次独立生物重复。

### 3.3 筛选与单细胞规模

- 定向库覆盖 120 个 TF/表观遗传因子，并含 120 条非靶向对照 gRNA；
- CRISPRi/a 命中集合用于 Perturb-seq 时为 32 条候选 gRNA 加 8 条非靶向 gRNA；
- 每种 Perturb-seq 模式跨 3 名供者约 60,000 个细胞，合计约 120,000 个细胞；
- 论文要求在每种模式中 gRNA 至少分配到每供者一定细胞数再分析；56/61 条满足覆盖条件的 targeting gRNA（92%）产生预期方向的靶基因调节；
- MYB CRISPRi 的两条 gRNA 分别产生 8,976 和 7,899 个显著 DEG；两条 BATF3 CRISPRa gRNA分别产生 3,056 和 1,402 个 DEG，说明部分转录因子是全局状态调节器而非单一 marker 调节器。

### 3.4 bulk RNA、ATAC 与体内验证规模

- BATF3 OE 的核心 bulk RNA-seq 比较在论文主图中为 5 名供者，检测到约 1,100 个 DEG；GEO 子系列还包括后续组合编辑，故总 GSM 为19；
- ATAC-seq：急性刺激 n=3 供者，慢性刺激 n=2 供者，各比较 BATF3 OE 与对照，共10个样本；
- 慢性刺激下，三种耗竭标志 TIGIT/LAG3/TIM3 全阳性的比例由对照约59%–65%降至 BATF3 OE 的13%；
- 体内 CAR-T 剂量验证使用多个 T 细胞供者和不同小鼠剂量；这些流式、肿瘤体积和生存数据主要位于 source data/补充表，不在 GEO 的表达矩阵中。

### 3.5 GSE197268 的正确定位

`paperlist.md` 同时列出 GSE197268，但该 accession 标题为“Differential dynamics of response…following CAR-T therapy”，包含32位患者的输注产品与治疗后 PBMC 单细胞数据，共109个GSM，原始数据受控访问。本文用它比较 BATF3 OE 诱导的基因程序与临床 CAR-T 扩增/应答特征；它不是本研究 CRISPR 实验产生的数据，下载与引用时应单列为 external validation。

## 4. 如何获取与复用数据

### 4.1 最省事的下载路线

1. Perturb-seq：进入 GSE218985，按 GSM 下载 6 个 tar.gz，或下载约2.09 GB的 `GSE218985_RAW.tar`；
2. bulk RNA：先下载 GSE218986 的两个 TPM 表；需要从头比对时再经 SRA Run Selector 获取 FASTQ；
3. ATAC：从 GSE218987 下载 bigWig 看轨迹、narrowPeak 做区间分析；重跑 peak calling 时使用 SRA 原始读段；
4. pooled screen：从 GSE241933 直接下载 counts CSV；
5. 论文图表：Nature 页面下载 Supplementary Tables 与 Source Data；
6. 基因层 screen 分析代码：下载 Zenodo 代码快照，避免依赖作者工作站环境。

命令行可用 NCBI HTTPS/FTP 路径批量下载，例如：

```bash
wget -c https://ftp.ncbi.nlm.nih.gov/geo/series/GSE218nnn/GSE218985/suppl/GSE218985_RAW.tar
wget -c https://ftp.ncbi.nlm.nih.gov/geo/series/GSE218nnn/GSE218987/suppl/GSE218987_RAW.tar
```

### 4.2 下载后优先核查

- scRNA 对象中是否保留 donor、CRISPRi/a、gRNA、perturbed/nonperturbed 标签；
- 同一 gRNA 在三位供者中的细胞覆盖，不能只按合并细胞数判断命中；
- counts 文件的行是 gRNA 还是 gene，列是 sorted/unsorted 还是时间点；
- ATAC bigWig 是可视化信号，narrowPeak 是离散区间，不能互换用于计数；
- bulk TPM 可用于展示，但严格差异表达应回到 raw counts/FASTQ 并建供者配对模型。

## 5. 主要生物学发现

CRISPRi 找到 FOXO1、MYB、BACH2、DNMT1 等维持 CCR7/记忆程序的调节器；CRISPRa 找到 EOMES、BATF、JUN、BATF3 等可主动重塑状态的因子。BATF3 是两种方向筛选中相互印证的强命中：激活 BATF3 提高 IL7R 等持续性相关表型，降低 TIGIT、LAG3、TIM3、CISH 及部分细胞毒/调节性程序，并在反复刺激中减轻耗竭样状态。

BATF3 并非简单“让所有效应功能更高”，而是把细胞推向更可持续、代谢活跃但即时细胞毒程序较低的状态。它与论文综述主题中的“导航状态”非常契合：优化目标应是长期疗效，而不只是短时杀伤峰值。

## 6. 从分子状态到功能

RNA-seq 与 ATAC-seq联合显示 BATF3 同时改变转录和顺式调控景观；CAR-T 实验进一步显示其能在慢性 HER2 抗原刺激下保留较少耗竭表型，并改善小鼠肿瘤控制。TFome KO 又表明 BATF3 的作用依赖 JUNB、IRF4 等 AP-1 网络成员，说明“一个驱动因子”实际嵌入组合调控网络。

## 7. 推荐图版

- **Fig. 1**：CRISPRi/a 平台与 CCR7 筛选命中；适合讲可逆状态扰动。
- **Fig. 2**：Perturb-seq 对候选调节器的全转录组解析；适合讲扰动后状态地图。
- **Fig. 3–4**：BATF3 对表达、耗竭与染色质的影响；最适合综述主线。
- **Fig. 5**：CAR-T 体内肿瘤控制；把分子导航连接到治疗结局。
- **Fig. 6**：±BATF3 TFome KO 网络；适合讲分子驱动与组合优化。

若只能选一组，选 Fig. 2 + Fig. 4：前者展示正交扰动图谱，后者展示状态改变的表观遗传基础。

## 8. 创新价值

1. 在原代人 CD8⁺ T 细胞中并行实现 CRISPRi 与 CRISPRa，而非只做 knockout。
2. 把表面状态筛选、Perturb-seq、RNA、ATAC 和 CAR-T 功能串成因果证据链。
3. 证明有利细胞治疗状态可由内源转录网络主动编程。
4. 提供了可复用的 gRNA counts、单细胞矩阵、ATAC 轨迹和分析代码。

## 9. 局限性

1. 初始库只有120个预选调节因子，不能视为无偏全基因组发现。
2. CCR7 是有用代理指标，但高 CCR7 不等同于体内持久性或疗效。
3. Perturb-seq 是约10天后的转录快照，不能直接给出状态转换速度和方向。
4. BATF3 OE 程度高于内源 CRISPRa，后续实验不完全等同于筛选扰动强度。
5. bulk RNA/ATAC 的供者数较小，慢性 ATAC 仅2名供者。
6. 小鼠异种移植不能完整模拟人肿瘤微环境及安全性；BATF3长期过表达的转化风险需独立评估。

## 10. 对本综述架构的作用

该文位于“the techniques to perturb/manipulate cell states”和“link cell state/function transitions with molecular drivers”之间：它展示如何先用正交 CRISPR 建立因果扰动地图，再把命中调节器推进到多组学与治疗功能验证。它不是实时控制系统，但为实时优化提供了候选控制变量和可量化状态读出。

## 11. 可直接用于综述的观点

> 在原代人 CD8⁺ T 细胞中，互补的 CRISPRi/CRISPRa 筛选与 Perturb-seq 可把 CCR7 等表面状态读出分解为具体转录调节网络；BATF3 激活/过表达通过重塑表达与染色质程序降低慢性刺激下的耗竭表型并改善 CAR-T 肿瘤控制，说明治疗性 T 细胞状态能够被因果地导航而非仅被事后描述（Nature Genetics 2023, McCutcheon）。

## 12. 避免误读

- 不要把 GSE197268 写成本研究新生成的 CRISPR 数据。
- 不要把约12万 Perturb-seq 细胞理解为12万名独立样本。
- 不要把 CCR7 升高直接等同于临床持久性。
- 不要把 BATF3 OE 的效果等同于任意强度的内源 BATF3 激活。
- 不要只用 UMAP/相关性宣称 BATF3 决定单向分化轨迹；这里更强的证据来自遗传扰动。

