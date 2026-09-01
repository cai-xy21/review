# 《Genome-wide CRISPR screens of T cell exhaustion identify chromatin remodeling factors that limit T cell persistence》精读

## 论文信息

- 作者：Julia A. Belk、Winnie Yao、Nghi Ly 等
- 期刊：*Cancer Cell*
- 年份：2022；40: 768–786.e7
- DOI：10.1016/j.ccell.2022.06.001
- 原文：[Cancer Cell](https://doi.org/10.1016/j.ccell.2022.06.001)
- PubMed：[PMID 35750052](https://pubmed.ncbi.nlm.nih.gov/35750052/)
- 数据总入口：[GEO SuperSeries GSE203593](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE203593)
- ATAC-seq：[GSE203591](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE203591)
- Perturb-seq：[GSE203592](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE203592)
- 论文给出的脚本入口：[GitHub](https://github.com/juliabelk/sarscov2_chirp_ms)

## 一句话结论

体外重复刺激和体内肿瘤 CRISPR 筛选共同锁定 cBAF/INO80 染色质重塑复合体；其中 Arid1a 缺失阻止终末耗竭表观遗传状态建立，维持效应功能和持久性，并增强小鼠及人 T 细胞抗肿瘤反应。

## 数据护照（先看这一表）

| 数据层 | 内容 | 公开位置 | 分析提醒 |
|---|---|---|---|
| genome-wide screen | 小鼠全基因组逆转录病毒库约 90,230 条 sgRNA；急性/慢性体外刺激及体内验证 | 补充表中的 counts/z-scores/命中 | SuperSeries 主要存 ATAC/Perturb-seq，不等于完整 screen 原始测序仓库 |
| ATAC-seq | 小鼠/人 T 细胞，无扰动、对照或 ARID1A 扰动，多状态设计 | GSE203591；42 个样本；BioProject PRJNA841558 | RAW TAR 约 7.1 GB，以 bigWig 等处理后文件为主；原始 reads 走 SRA |
| Perturb-seq | MC38 肿瘤浸润 T 细胞，GEX+sgRNA library | GSE203592；16 个 GEO library/sample 条目；PRJNA841559 | library 条目不直接等于动物数；含两个实验和多 capture |
| 处理后单细胞对象 | 整合的 Perturb-seq Seurat 对象 | `integrated_v2.rds.gz` 约 2.7 GB | 快速复用首选，需检查元数据中的 experiment/mouse/library |
| SuperSeries | GSE203591 + GSE203592 | GSE203593，共 58 个 sample entries | 应从两个子系列下载，不要把 ATAC 和 Perturb-seq 混成一种数据 |
| 代码 | 论文 Data availability 指向 GitHub | `juliabelk/sarscov2_chirp_ms` | 仓库名与本论文主题不一致；使用前需逐脚本确认是否覆盖本文分析 |

## 1. 研究要解决的问题

T 细胞耗竭由持续刺激驱动并形成稳定表观遗传状态。传统比较组学能发现相关因子，却难以系统确定哪些基因对耗竭建立是必需的。本文建立可筛选的体外耗竭模型，并与肿瘤体内筛选交叉验证。

## 2. 筛选与验证框架

1. 构建急性一次刺激与慢性重复刺激的体外 CD8⁺ T 细胞体系。
2. 使用约 90,230 条 sgRNA 的小鼠 genome-wide CRISPR 库筛选持续性/功能调控因子。
3. 在肿瘤环境进行体内筛选并与体外命中交叉。
4. 聚焦 cBAF 和 INO80 复合体，深入验证 Arid1a。
5. 用 ATAC-seq、Perturb-seq、功能和肿瘤模型解析 Arid1a 缺失如何阻止终末耗竭。

## 3. 数据内容详解

### 3.1 GSE203591：ATAC-seq

42 个样本覆盖小鼠和人 T 细胞、急性/慢性或肿瘤相关状态，以及 no perturbation、control 和 ARID1A/Arid1a 扰动。它不是简单两组设计；重分析前必须用 GEO sample table 建立物种、供者/动物、刺激状态、基因扰动和重复字段。SuperSeries 的约 7.1 GB RAW TAR 主要汇总处理后信号文件；统一 peak calling 需要 SRA 原始 reads。

### 3.2 GSE203592：Perturb-seq

该子系列含 16 个 GEX/sgRNA library 条目，来自 MC38 肿瘤内 T 细胞的两个实验：实验 1 包括多个小鼠/capture，实验 2 包括双侧肿瘤等设计。GEX 和 sgRNA capture 可能以不同 GEO/SRA library 记录，因此 16 不能直接写成 16 只小鼠。

`integrated_v2.rds.gz` 约 2.7 GB，是快速复用的主文件。下载后应先检查每个细胞的 sgRNA、mouse、experiment、tumor/capture、cluster 和 QC 字段，并用动物层伪 bulk 评价扰动效应。

### 3.3 screen counts

全基因组筛选的 sgRNA counts、标准化分数和候选基因主要随 Supplementary Tables 发布。GEO GSE203593 的标题虽然对应整篇论文，但其子系列集中在 ATAC-seq 与 Perturb-seq；不能假设全套 90,230 sgRNA 的原始筛选 FASTQ 已全部包含在 GEO。

## 4. 数据下载方式

### 4.1 快速路线

```text
https://ftp.ncbi.nlm.nih.gov/geo/series/GSE203nnn/GSE203592/suppl/
```

优先下载 `integrated_v2.rds.gz`（约 2.7 GB）做单细胞状态和扰动分析。ATAC 若只浏览 locus，可下载 GSE203591 的 bigWig/TAR；若只研究一个物种或处理，按样本逐文件选择，避免整包 7.1 GB。

### 4.2 原始路线

分别从 PRJNA841558（ATAC）和 PRJNA841559（Perturb-seq）进入 SRA Run Selector：

```bash
prefetch --option-file SraAccList.txt
fasterq-dump --split-files --threads 12 SRRxxxxxxx
```

Perturb-seq 必须同时保存 GEX 与 guide-capture library 的对应关系；只下载 GEX 会丢失扰动身份。

### 4.3 筛选表和代码

从期刊 Supplementary Information 下载 screen count/z-score 表。GitHub 链接按论文 Data availability 记录，但仓库名指向 SARS-CoV-2 CHIRP-MS 项目，使用前应验证 commit、目录和脚本是否真的对应本文；不能仅因存在链接就写“完整代码已公开”。

## 5. 主要发现

1. 体外和体内筛选共同富集 cBAF/INO80 染色质重塑因子。
2. Arid1a 缺失提高慢性刺激下 T 细胞持续性和效应功能。
3. Arid1a 对终末耗竭染色质可及性建立是必需的。
4. Arid1a 扰动改善肿瘤控制，并在人 T 细胞中表现出保守效果。

## 6. ARID1A 的机制解释

ARID1A 是 cBAF 复合体组成部分。其缺失并非简单“阻止激活”，而是改变持续刺激下的染色质重塑路径，使细胞较少进入稳定的终末耗竭状态，同时保留效应程序。由于 cBAF 广泛调控染色质，长期编辑可能带来状态稳定性和安全性问题。

## 7. 推荐图版

- 急性/慢性体外筛选体系与 genome-wide screen 结果。
- 体外与体内筛选命中交集。
- Arid1a 缺失 Perturb-seq 状态图。
- ATAC 显示终末耗竭可及性受阻的图。
- 人 T 细胞/肿瘤控制验证。

## 8. 创新价值

1. 建立可进行全基因组筛选的体外耗竭模型。
2. 用体内肿瘤筛选提高命中的生理相关性。
3. 以 Perturb-seq 和 ATAC 将基因命中连接到细胞状态和表观遗传机制。
4. 揭示染色质重塑复合体是耗竭建立的主动执行者。

## 9. 局限性

1. 体外重复刺激只能模拟持续抗原的一部分。
2. 大型 CRISPR screen 存在 sgRNA 效率、瓶颈和细胞适应度混杂。
3. ARID1A 广泛调控染色质，长期编辑的基因组稳定性和潜在转化风险需评估。
4. GEO 设计复杂，library 条目、细胞和动物层级易混淆。
5. 论文代码链接的主题/仓库名异常，完整可复现性需人工审计。

## 10. 对综述的作用

该文是“染色质重塑限制 T 细胞持久性”和“多层 CRISPR 筛选发现耗竭靶点”的核心文献，可与代谢、激酶类靶点形成机制对照。

## 11. 可直接用于综述的观点

> 体外和肿瘤体内 genome-wide CRISPR 筛选共同识别 cBAF/INO80 复合体；Arid1a 缺失可阻断终末耗竭表观遗传状态建立，并维持 T 细胞效应功能和持久性（Cancer Cell 2022, Belk）。

## 12. 数据复用建议

- 单细胞优先使用 `integrated_v2.rds.gz`，但统计以动物/实验为单位。
- ATAC 先选择同物种、同刺激和同扰动子集，再统一 peak calling。
- 将 GEX 与 guide library 建立一一对应表。
- screen 原始/处理后 counts 从补充表单独下载并记录版本。

## 13. 避免误读

- GSE203593 的 58 个条目是 42 个 ATAC + 16 个 Perturb-seq library/sample 记录，不是 58 只动物。
- 不要把 SuperSeries 的 ATAC 和 Perturb-seq 直接合并成同一表达矩阵。
- 不要假定 GEO 包含全部 genome-wide screen 原始 reads。
- GitHub 链接存在不匹配迹象，引用“代码公开”时必须加核验说明。
