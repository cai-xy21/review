# 《Multiplexed transcriptomic profiling of the fate of human CAR T cells in vivo via genetic barcoding with shielded small nucleotides》精读

## 论文信息

- 作者：Xiaotao Lu、Stephen M. Lofgren、Ying Zhao、Przemyslaw K. Mazur
- 期刊：*Nature Biomedical Engineering*
- 年份：2023
- DOI：[10.1038/s41551-023-01085-3](https://doi.org/10.1038/s41551-023-01085-3)
- PubMed：[PMID 37652986](https://pubmed.ncbi.nlm.nih.gov/37652986/)
- 数据：[GEO GSE201647](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE201647)；BioProject `PRJNA832482`

## 一句话结论

SSN-seq 以可遗传的小 RNA 条形码把制造条件与后续单细胞转录状态连接起来，并在 8 路 CAR T 制造筛选中发现 TWS119+JQ1+IL-7/15 更有利于持续性 CD4 记忆状态。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 方法 | SSN-seq 小 RNA 遗传条形码 | 记录来源/处理身份，不直接记录自然状态方向 |
| 初始混种验证 | 10,245 个细胞 | 检验 barcode 与 species assignment |
| 制造筛选 | 8 条件 | 4 药物条件 × 2 cytokine 条件 |
| ex vivo CAR T | 9,854 个细胞 | >93% 分配至少一个 SSN |
| in vivo CAR T | 8,679 个高质量细胞 | >73% 检出 SSN；小鼠复挑战后 |
| 两供者验证 | in vivo 6,299 个细胞 | 82% 分配 SSN |
| GEO | 15 GSM library records | mRNA 与 SSN 文库按实验拆分 |
| 处理后文件 | 约 690 MB | 7 套矩阵 + 2.2 MB cell–SSN 注释表 |

## 1. 研究要解决的问题

传统 pooled perturbation 能在终点读出 sgRNA，但多种培养条件、细胞来源和体内命运常难在一次实验中追踪。SSN-seq 设计可被标准 3′/5′ scRNA 捕获的 small-RNA barcode，使每个终末细胞仍携带其初始处理身份。

## 2. 方法框架

- 构建可遗传表达且可被直接捕获的 SSN barcode；
- 在混种、20 组细胞和人/鼠 T 细胞中验证特异性与串样；
- 给 8 种 CAR T 制造条件赋予不同 barcode 后混池；
- 在 NALM6-NSG 小鼠中输注、day 21 复挑战、day 42 取脾；
- 用 scRNA + SSN 读取状态和制造来源，另以供者/小鼠独立验证。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

SSN-seq 数据有两套配对矩阵：

1. **mRNA matrix**：终点单细胞转录状态；
2. **SSN count matrix**：每个细胞捕获的 synthetic barcode；
3. **cell–SSN annotation**：将 barcode 解码成药物、细胞因子、供者或实验组；
4. **实验/动物元数据**：区分 ex vivo、首次输注、复挑战后、供者与小鼠；
5. **部分 V(D)J**：Exp6 提供 TCR contigs。

SSN 与自然 TCR clonotype 不同：前者是人为赋予的处理标签，可追踪群体来源；后者是免疫受体重排，可追踪克隆。SSN 也不是连续传感器，不能告诉细胞在中途何时转换状态。

### 3.2 规模与实验构成

| 实验层 | 规模 | 用途 |
|---|---:|---|
| 混种 sgRNA/SSN 验证 | 10,245 cells | 测试 assignment 准确性 |
| 多路编码验证 | 20 barcoded groups | 细胞系 + 人/鼠 T 细胞 |
| 8 路制造 ex vivo | 9,854 cells | >93% 至少一个 SSN |
| 8 路制造 in vivo | 8,679 high-quality cells | >73% 至少一个 SSN |
| 独立双供者 in vivo 验证 | 6,299 cells | 82% 至少一个 SSN |

8 路条件为药物 `DMSO / TWS119 / JQ1 / TWS119+JQ1` 与细胞因子 `IL-2 / IL-7+IL-15` 的 4×2 组合。主体模型为 NALM6 异种移植 NSG 小鼠，day 21 肿瘤复挑战，day 42 取脾分析。

### 3.3 GEO GSE201647 有什么

- BioProject：`PRJNA832482`；
- GEO 当前列出 **15 个 GSM library records**，对应 7 组实验中的 mRNA 与 SSN 文库，最后一组另含 TCR；
- 核心注释：`Cell-SSN_annotation.xlsx`，约 **2.2 MB**；
- 7 套处理后表达矩阵，单个矩阵压缩体积约 51.6–134.0 MB，总计约 **683 MB**；
- 连同条形码/features/TCR contigs，BioProject supplementary 约 **690 MB**；
- 原始 SRA 约 **0.26 TB / 792 Gb**。

7 套矩阵通常以 `matrix.mtx.gz`、`barcodes.tsv.gz`、`features.tsv.gz` 组织。下载后必须用注释表把 SSN 序列解码为实验条件；仅看表达矩阵无法知道制造来源。

### 3.4 外部数据与下载路线

作者还比较了 GSE78220、GSE83978、GSE114631、GSE120575、GSE150992 等公开数据；这些是复用参考，不属于 SSN-seq 新生成数据。

- **复用状态—条件关系**：下载 690 MB 处理后包和 2.2 MB annotation 即可；
- **评估 barcode 错配/多重标记**：下载 mRNA 与 SSN 原始 FASTQ，重新运行 assignment；
- **做克隆—条件联合**：只在有 TCR contig 的实验中分析，并区分 TCR 与 SSN ID；
- **复现 8 路筛选**：保留 drug、cytokine、donor、mouse、timepoint 五个层级。

### 3.5 下载后先做什么

1. 检查每细胞 SSN 数量和 assignment posterior/阈值；
2. 标记 0、1、>1 SSN 的细胞，评估 doublet 与 barcode leakage；
3. 在 donor/mouse 层做重复，不用细胞数制造虚假显著性；
4. 将 ex vivo 与 in vivo 分开归一化/比较；
5. 对公开参考 accession 标注 external，避免与新数据混算。

## 4. 主要发现

TWS119+JQ1 与 IL-7/15 的组合在体内富集持续性 CD4 记忆样程序。SSN 使多种制造条件能在同一动物环境中竞争，从而降低跨动物环境差异。

## 5. 与“状态导航”最相关的分析

SSN-seq 是“条件—终点状态”导航的重要工具：它允许高通量并行测试输入条件，并用终点单细胞转录组读出命运。但它不是实时 live tracking；要构建闭环系统仍需加入在线功能/代谢传感。

## 6. 推荐图版

- SSN 构建、捕获和解码示意图。
- 8 路 CAR T 制造筛选设计。
- ex vivo/in vivo 条件富集与 CD4 memory program 图。

## 7. 创新价值

将制造配方本身编码进细胞，使多个条件可混池、共享体内环境并在终点单细胞解析。

## 8. 局限性

依赖基因工程 barcode；存在漏检/多重 SSN；异种移植模型与患者差异大；barcode 只追踪来源，不直接证明状态转变路径或功能因果。

## 9. 对本章节的作用

适合“perturb/manipulate states”和“optimize conditions”，作为规模化制造条件搜索技术。

## 10. 可直接用于综述的观点

> SSN-seq 以可遗传的小 RNA 条形码把多路制造条件与终点单细胞状态连接，并在 CAR T 筛选中提名 TWS119+JQ1+IL-7/15 为促进持续性 CD4 记忆程序的组合（Nature Biomedical Engineering 2023, Lu）。

## 11. 避免误读

- 不要把 SSN barcode 当作天然 clonotype 或实时轨迹。
- 不要把小鼠异种移植结果写成患者临床验证。
- 不要把复用 GEO 数据计入 GSE201647 的新生成规模。
