# 《Nutrient-driven histone code determines exhausted CD8+ T cell fates》精读

## 论文信息

- **作者**：Shixin Ma、Michael S. Dahabieh、Thomas H. Mann 等
- **期刊与年份**：*Science*, 2025；387: eadj3020
- **DOI**：10.1126/science.adj3020
- **本地原文**：[PDF](<D:/research/review/perturbation33references/33-Nutrient-driven histone code determines exhausted CD8+ T cell fates.pdf>)
- **表观组/转录组 SuperSeries**：[GEO GSE235007](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE235007)
- **MC38 单细胞数据**：[GEO GSE235195](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE235195)

## 一句话结论

CD8⁺ T 细胞耗竭伴随乙酸—ACSS2 到柠檬酸—ACLY 的核内 acetyl-CoA 来源切换；KAT2A–ACLY 在耗竭基因位点增加组蛋白乙酰化，而 p300–ACSS2 支持效应/记忆基因，因而核定位 ACSS2 或抑制 ACLY 可延缓耗竭并增强肿瘤控制。

## 数据护照

| 数据集 | 数据类型 | 样本数 | 组成 |
|---|---|---:|---|
| GSE235005 | CUT&RUN/CUT&Tag/ChIP 类组蛋白乙酰化图谱 | 50 | H3K27ac/H3K9ac/H4ac；TEXprog/term、sgAcly/sgAcss2、ACSS2NLS、sgEp300、sgKat2a |
| GSE235006 | bulk RNA-seq | 4 | B16-GP33 TIL：EV 2 vs ACSS2NLS 2 |
| GSE235007 | SuperSeries | 54 | 50 表观组 + 4 RNA |
| GSE235195 | scRNA-seq | 2 个测序样本 | MC38 肿瘤浸润免疫细胞；2 个生物重复 |
| 公开处理层 | BigWig/BW、FPKM、10x raw tar | GSE235005/6/195 |

## 1. 研究问题

耗竭 T 细胞同时发生代谢和表观遗传重塑，但营养底物如何在特定位点生成 acetyl-CoA 并决定组蛋白乙酰化，仍不清楚。作者要区分“全细胞 acetyl-CoA 总量”与“核内酶复合物在特定位点局部供给”的作用。

## 2. 实验设计与方法框架

研究结合慢性 LCMV Clone 13、B16-GP33/MC38 肿瘤和体外慢性刺激，比较 acetate/ACSS2 与 citrate/ACLY。用同位素示踪、代谢分析、H3K27ac/H3K9ac/H4ac CUT&Tag/CUT&RUN、bulk RNA、scRNA、CRISPR 扰动及核定位 ACSS2，测试 KAT2A–ACLY 与 p300–ACSS2 的位点特异复合物。

## 3. 数据规模与图谱组成

### 3.1 GSE235005：50 个组蛋白乙酰化文库

这 50 个文库可按实验模块完整拆解：

#### A. LCMV TEXprog vs TEXterm：12

2 个状态 × 3 个 marks（H3K27ac、H3K9ac、H4ac）× 2 重复 = 12。

#### B. 体外 TEX，sgAcly vs scramble：12

2 个扰动 × 3 marks × 2 重复 = 12。

#### C. B16-GP33 TIL，ACSS2NLS vs EV：12

2 个构建 × 3 marks × 2 重复 = 12。

#### D. 体外 TEX，sgAcss2：6

sgAcss2 × 3 marks × 2 重复 = 6；其对照可与相应 scramble 设计核对，但不能在不查 metadata 时随意复用其他批次对照。

#### E. H3K27ac 乙酰转移酶验证：8

- scramble vs sgEp300：2 + 2 = 4；
- scramble vs sgKat2a：2 + 2 = 4。

合计 **12 + 12 + 12 + 6 + 8 = 50**。

GEO 将实验类型标为 genome binding/occupancy，并提供 `GSE235005_RAW.tar` 约 7.0 GB，内含 BigWig/BW 等处理轨迹；原始 reads 在 SRA/BioProject `PRJNA984164`。

### 3.2 GSE235006：4 个 ACSS2NLS TIL bulk RNA-seq

- B16-GP33 肿瘤 day 21 TIL；
- EV 2 个重复；
- ACSS2NLS 2 个重复；
- 提供 `GSE235006_Acss2OE_fpkm.txt.gz` 和 SRA reads。

这 4 个样本与 GSE235005 中 ACSS2NLS/EV 的表观组模块生物情境匹配，适合在基因层连接表达与乙酰化，但重复数仅 2/组。

### 3.3 GSE235007：54-GSM SuperSeries

GSE235007 只是把 50 个表观组和 4 个 RNA 文库汇总。分析时应进入 GSE235005/6；`GSE235007_RAW.tar` 是总入口，但单独子系列更容易核对样本条件。

### 3.4 GSE235195：2 个 MC38 scRNA-seq 样本

页面描述为 MC38 肿瘤浸润免疫细胞的单细胞表达图谱，两个样本名为 `MC38 tumor rep1, scRNAseq` 和 `rep2`。提供 `GSE235195_RAW.tar` 和 SRA/BioProject `PRJNA984745`。

该 accession 只有 2 个生物重复；即使有数千细胞，也不能支持把细胞当独立 n 的组间推断。页面标题未在 sample level 直接写清额外处理条件，下载后应查 GSM metadata 与论文 figure mapping。

### 3.5 三类图谱如何对应

| 生物情境 | 表达层 | 表观层 | 作用 |
|---|---|---|---|
| 慢性 LCMV | 论文/其他实验 readout | TEXprog vs TEXterm 三种 acetyl marks | 定义自然耗竭阶段差异 |
| 体外慢性刺激 | 功能/代谢 | sgAcly、sgAcss2、sgEp300、sgKat2a | 建立因果机制 |
| B16-GP33 | 4-sample bulk RNA | ACSS2NLS vs EV 三 marks | 测试治疗性重编程 |
| MC38 | 2-sample scRNA | 无匹配单细胞表观组 | 验证 TME 细胞状态 |

这不是同一批细胞的单细胞 multiome；“联合图谱”来自跨模型、跨 assay 的机制三角验证。

### 3.6 推荐下载方式

1. 位点浏览：先取 GSE235005 的 BigWig/BW，在 TOX、TCF7、effector/memory loci 比较。
2. 差异乙酰化：下载 50 个 SRA run，按五个模块分别 peak calling；禁止跨批次乱配 scramble。
3. 表达—表观连接：取 GSE235006 FPKM 和对应 ACSS2NLS/EV H3K27ac/H3K9ac/H4ac。
4. 单细胞：取 GSE235195 10x 文件；先确认两重复的处理标签，再做 sample-aware analysis。

## 4. 主要结果

耗竭进展时 ACSS2 下调而 ACLY 保持，使组蛋白乙酰化更依赖 citrate。KAT2A 与 ACLY 在耗竭基因位点协作，p300 与 ACSS2 支持效应/记忆位点。ACLY 删除/抑制或核定位 ACSS2 重塑 H3K27ac、限制终末耗竭并改善抗肿瘤响应。

## 5. 机制理解

细胞核中的 acetyl-CoA 不是完全均一池。代谢酶与 acetyltransferase 形成位点邻近复合物，使特定营养来源优先供给某组基因：ACLY–KAT2A 推进耗竭，ACSS2–p300 支持效应/记忆。营养选择因此被翻译为“histone code”。

## 6. 推荐重点阅读的图

- acetate/citrate isotope tracing 与 ACSS2/ACLY变化。
- TEXprog/term 三种 acetyl marks。
- sgAcly、sgAcss2 的 H3K27ac 与功能后果。
- KAT2A–ACLY、p300–ACSS2 互作/位点机制。
- ACSS2NLS TIL 的表达、表观和肿瘤控制。

## 7. 创新性

把营养底物、核内代谢酶、乙酰转移酶和特定位点组蛋白乙酰化串成因果链，超越“总 acetyl-CoA 决定全局乙酰化”的简单模型。

## 8. 局限性

大量组学模块只有 2 个重复；跨 LCMV、体外和肿瘤模型联合解释存在批次/情境差异。GSE235195 仅 2 个单细胞样本。代谢酶广泛作用，ACLY/ACSS2 干预的非组蛋白和非 T 细胞效应尚难完全排除。

## 9. 在综述中的定位

适合作为“营养—核内代谢—表观遗传决定耗竭命运”的机制代表，可与 acetate immunomodulation、NaCl/glutamine 和 potassium/低营养研究串联。

## 10. 可直接写入综述的表述

> CD8⁺ T 细胞耗竭伴随核内 acetyl-CoA 来源从 ACSS2–acetate 转向 ACLY–citrate；KAT2A–ACLY 促进耗竭位点乙酰化，而 p300–ACSS2 维持效应/记忆位点，揭示营养底物通过局部酶复合物编码 T 细胞命运。

## 11. 数据复用建议

先把 50 个表观文库按 12/12/12/6/8 五模块分开；每模块独立差异分析，再做基因集层 meta-integration。ACSS2NLS 的 4 RNA + 12 histone-mark 文库最适合直接表达—表观耦合。所有推断以重复/小鼠为单位。

## 12. 转化与安全性关注

ACLY 抑制可能影响全身脂质代谢和多种免疫细胞；ACSS2NLS 属基因工程策略，需评估持续表达和异常表观重编程。更安全方向可能是制造期短暂代谢干预或 T 细胞定向递送。

## 13. 避免误读

- GSE235007 的 54 是 50 个表观文库 + 4 个 bulk RNA 文库。
- GSE235195 只有 2 个生物样本，不因细胞多就拥有大 n。
- 三类数据不是同细胞 multiome。
- scramble controls 存在不同配对批次，不能跨实验模块随意共用。

