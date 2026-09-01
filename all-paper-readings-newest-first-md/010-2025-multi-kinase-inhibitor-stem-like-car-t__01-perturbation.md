# 《A multi-kinase inhibitor screen identifies inhibitors preserving stem-cell-like chimeric antigen receptor T cells》精读

## 论文信息

- **作者**：Feifei Song、Ourania Tsahouridis、Simone Stucchi 等
- **期刊与年份**：*Nature Immunology*, 2025；26: 279–293
- **DOI**：10.1038/s41590-024-02042-1
- **本地原文**：[PDF](<D:/research/review/perturbation33references/28-A multi-kinase inhibitor screen identifies inhibitors preserving stem-cell-like chimeric antigen receptor T cells.pdf>)
- **RNA-seq**：[GEO GSE261109](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE261109)
- **ATAC-seq**：[GEO GSE261127](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE261127)
- **SuperSeries**：[GEO GSE261133](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE261133)

## 一句话结论

对 320 个 PKIS 激酶抑制剂的自动化筛选识别出三药组合 UNC10225387B、UNC10225263A 和 UNC10112761A；其在 CAR-T 制造期短暂使用，可富集 CD45RA⁺CCR7⁺TCF1^hi 干样细胞并增强体内疗效，主要伴随可逆转录变化而非明显染色质重塑。

## 数据护照

| 模块 | 规模/组成 | 获取位置 |
|---|---|---|
| 初筛 | 320 个 PKIS1/PKIS2 compounds；384 孔；2 位健康供体 | 论文 Source Data |
| 初筛命中 | donor 1：80；donor 2：24；共享 15 | Fig. 1 / Source Data |
| 剂量响应 | 7 个候选，最终 3 个组成 cocktail | 论文图表 |
| bulk RNA-seq | 2 供体 × DMSO/KI × day 1/3/7 = 12 | GSE261109 |
| ATAC-seq | naive、DMSO-day3、KI-day3 × 2 重复 = 6 | GSE261127 |
| GEO 合计 | 18 个 GSM | GSE261133 |

## 1. 研究问题

CAR-T 产品中 TSCM-like 细胞比例与持久疗效相关，但针对单一路径的抑制常被旁路补偿。作者希望通过多激酶化合物库找到能在制造期同时抑制多个分化轴、又不阻断扩增和功能的组合。

## 2. 实验设计与方法框架

作者在 384 孔平台中以 3 μM PKIS 化合物处理 anti-CD3/CD28 激活的人 T 细胞 5 天，用 CD45RA→CD45RO 转换、细胞数和激活指标筛选。共享命中进入剂量响应，三种化合物组成 KI cocktail；随后在健康供体和 CLL 患者来源 CD19 CAR-T、体外杀伤和小鼠模型中验证，并用 RNA-seq、ATAC-seq、kinome profiling 与基因敲低追踪靶点。

## 3. 数据规模与图谱组成

### 3.1 自动化化合物筛选：320 个 compounds

每块 384 孔板包含 320 个 PKIS 化合物孔、32 个 DMSO 对照和 32 个未刺激对照。两位供体独立筛选；donor 1 有 80 个、donor 2 有 24 个符合排除/活性标准，15 个在两者中共享。再从中识别 7 个具有一致剂量响应的候选，最终选 3 个组成 cocktail。

初筛的 320 个化合物—表型矩阵主要存在论文 Source Data/Extended Data，而不是 GEO。GEO 只承载后续 RNA/ATAC；在 GEO 中找不到 320 条“样本”是正常的。

### 3.2 GSE261109：12 个 bulk RNA-seq 文库

页面给出明确的纵向设计：

- 供体：Donor 1、Donor 3，共 2 位；
- 处理：DMSO、KI cocktail；
- 时间：day 1、day 3、day 7；
- 总数：**2 × 2 × 3 = 12**。

提供 `GSE261109_RNA.norm.count.csv.gz`（约 340 KB）和 SRA 原始 reads。测序为 paired-end 100 bp，STAR 对齐、Salmon 定量。页面文字提到 baseline，但 12 个 GEO 标题只列 day1/3/7；因此不要把 baseline 另加为文库。

### 3.3 GSE261127：6 个 ATAC-seq 文库

| 状态 | 重复数 |
|---|---:|
| naive cells | 2 |
| DMSO day 3 | 2 |
| KI day 3 | 2 |
| **合计** | **6** |

提供 `GSE261127_ATAC_ucsctracks.bed.gz` 和 SRA 原始数据。ATAC 只在 day 3 检验 KI 与 DMSO，另以 naive 为参照；它不能支持 day1–day7 全时间的染色质动态结论。

### 3.4 SuperSeries GSE261133

GSE261133 合并 12 RNA + 6 ATAC，共 18 个 GSM。RNA 与 ATAC 的设计不完全对称：RNA 是两位供体的三时间点处理比较，ATAC 是 naive/DMSO/KI 的 day3 横截面。因此联合解释应围绕 day3，而不是强行构造全时序 multiome。

### 3.5 细胞来源与图谱边界

公开组学来自健康供体 PBMC 分离 T 细胞、CD19 CAR 转导后培养。论文也验证 CLL 患者来源细胞，但 CLL 流式/功能数据主要在 Source Data；不能把 GEO 12 个 RNA 文库描述成健康供体加患者混合队列。

### 3.6 推荐下载方式

1. 化合物排名：下载论文 Source Data 和 Extended Data Tables，恢复 320×readout 矩阵。
2. 转录动态：直接取 GSE261109 normalized counts；配对模型使用 donor block。
3. 染色质：取 GSE261127 BED 轨迹；重做 peak calling 时再下载 FASTQ。
4. 联合分析：只在 day3 比较 KI vs DMSO 的 RNA 与 ATAC，naive 作为参照状态。

## 4. 主要结果

三药 cocktail 在健康供体和 CLL 来源 CAR-T 中提高 CD45RA⁺CCR7⁺TCF1^hi 细胞比例，增强再刺激扩增和体内肿瘤控制。化合物靶向 ITK、ADCK3、MAP3K4、CDK13 等；基因敲低能富集 TSCM-like 表型，但只有药物 cocktail 同时保持充分扩增和功能。

## 5. 机制理解

cocktail 通过短暂、部分且多节点的激酶抑制减慢分化，而不是永久关闭单一主开关。RNA 变化明显、ATAC 变化有限，提示制造期表型主要是可逆信号/转录状态，而非稳定重写染色质。

## 6. 推荐重点阅读的图

- 320 compounds 初筛与两供体交集。
- 7 个剂量响应候选和三药组合。
- CAR-T TSCM-like 表型、扩增和再刺激功能。
- 12 RNA/6 ATAC 的转录—染色质比较。
- ITK/ADCK3/MAP3K4/CDK13 靶点验证。

## 7. 创新性

以“保持未分化但不牺牲扩增”为多目标筛选标准，并用多靶点短时药理扰动避免单基因敲低的功能折损。

## 8. 局限性

组学仅 2 位健康供体，统计外推有限；ATAC 只有 day3。PKIS 化合物具有多靶点，因果归因并不唯一。三药组合的制造残留、相互作用和临床级工艺仍需评估。

## 9. 在综述中的定位

适合作为“制造期小分子组合维持 CAR-T 干性”的代表，与 PI3K/AKT、MEK、mTOR 等单通路抑制策略比较。

## 10. 可直接写入综述的表述

> 320 个激酶抑制剂的多目标筛选得到三药 cocktail，可在 CAR-T 制造期短暂抑制多个分化相关激酶，富集 TCF1^hi TSCM-like 细胞而不牺牲后续扩增和杀伤。

## 11. 数据复用建议

用 `~ donor + day + treatment + day:treatment` 分析 12 个 RNA 文库；ATAC 仅做 day3 KI vs DMSO，并以 naive 参照。可将 RNA 中的即时应答基因与 ATAC 稳定性结合，识别无需染色质重塑即可调节的制造靶点。

## 12. 转化与安全性关注

三药联合需建立药代洗脱和残留标准，验证不造成基因毒性、长期增殖异常或 TCR/CAR 信号不可逆受损；患者来源细胞的异质性和既往治疗影响应纳入工艺放大。

## 13. 避免误读

- 320 compounds 的初筛数据主要不在 GEO。
- GSE261109 只有 12 个 day1/3/7 文库，页面提到 baseline 不代表多出两个样本。
- ATAC 只有 6 个样本和 day3 横截面。
- 基因敲低富集 TSCM-like 不等于能复制药物 cocktail 的完整功能。

