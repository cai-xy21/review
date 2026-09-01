# 《Acetate acts as a metabolic immunomodulator by bolstering T-cell effector function and potentiating antitumor immunity in breast cancer》精读

## 论文信息

- **作者**：Katelyn D. Miller、Seamus O’Connor、Katherine A. Pniewski 等
- **期刊与年份**：*Nature Cancer*, 2023；4: 1491–1507
- **DOI**：10.1038/s43018-023-00636-6
- **本地原文**：[PDF](<D:/research/review/perturbation33references/29-Acetate acts as a metabolic immunomodulator by bolstering T-cell effector function and potentiating antitumor immunity in breast cancer.pdf>)
- **转录组 SuperSeries**：[GEO GSE202281](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE202281)
- **代谢组**：[Metabolomics Workbench ST002740](https://www.metabolomicsworkbench.org/data/DRCCMetadata.php?Mode=Study&ResultType=1&StudyID=ST002740&StudyType=MS)
- **项目 DOI**：[10.21228/M89T4G](https://doi.org/10.21228/M89T4G)

## 一句话结论

抑制肿瘤细胞 ACSS2 使其从乙酸消费者转为释放者，从而增加微环境乙酸；CD8⁺ T 细胞可通过 ACSS1 氧化乙酸、增强增殖和效应功能，形成同时打击肿瘤代谢并支持抗肿瘤免疫的双重策略。

## 数据护照

| 模块 | 规模/组成 | 获取位置 |
|---|---|---|
| 肿瘤 bulk RNA-seq | 14 个样本：免疫缺陷 NGS 6 + 免疫完整 C57BL/6 8 | GSE202279 |
| 肿瘤 scRNA-seq | 6 只肿瘤 multiplex 成 2 个测序库；vehicle 3、ACSS2i 3 | GSE202280 |
| GEO SuperSeries | 16 个 GSM，但对应 14 bulk + 2 pooled libraries | GSE202281 |
| 非靶向 ^13C2-acetate 代谢组 | 42 个 LC-MS 样本 | ST002740 / PR001705 |
| 代谢组组成 | WT/Acss2−/− CD8⁺ T + WT NK；100/500 μM；1/2 h；部分 VY-3-135；多为 n=3 | ST002740 sample table |
| 外部人乳腺图谱 | 健康乳腺 GSE164898；乳腺癌 GSE176078 | 仅再分析 ACSS1/2 表达 |

## 1. 研究问题

ACSS2 是肿瘤利用乙酸合成 acetyl-CoA 的关键酶，但阻断肿瘤乙酸代谢会如何改变免疫微环境尚不清楚。作者提出“代谢底物再分配”假说：抑制肿瘤 ACSS2 不仅饿死肿瘤，还可能把乙酸留给免疫细胞。

## 2. 实验设计与方法框架

研究用 CRISPR 或小分子抑制 ACSS2，在免疫缺陷和免疫完整乳腺癌模型中比较肿瘤生长；用 bulk/scRNA-seq观察 TME，用 ^13C2-acetate LC-MS 追踪 CD8⁺ T/NK 细胞代谢，并验证 ACSS1 依赖性与化疗联合。

## 3. 数据规模与图谱组成

### 3.1 GSE202281 是 16-GSM SuperSeries

SuperSeries 包含 GSE202279（14 bulk）与 GSE202280（2 scRNA pooled libraries）。16 是 GEO library 记录数，不等同于 16 只小鼠；单细胞两个 GSM 实际汇集 6 个生物样本。

### 3.2 GSE202279：14 个 bulk RNA-seq 样本

| 模型/宿主 | Vehicle | ACSS2 inhibitor | 合计 |
|---|---:|---:|---:|
| `NGS` 免疫缺陷背景 | 3 | 3 | 6 |
| C57BL/6 免疫完整背景 | 4 | 4 | 8 |
| **合计** | **7** | **7** | **14** |

提供 `GSE202279_ACSS2i_RNAseq.txt.gz`（约 586.6 KB）和 SRA 原始 reads。免疫缺陷与免疫完整模型可用于区分直接肿瘤代谢效应和免疫依赖效应，但它们不是同一背景下可随意合并的 7 vs 7。

### 3.3 GSE202280：6 只肿瘤、2 个 pooled 单细胞库

GEO 只有 2 个 GSM：一个 gene-expression library、一个 antibody/protein library；样本注释指出 3 个 vehicle 和 3 个 ACSS2i 肿瘤通过 antibody capture barcode multiplex。公开文件包括：

- `GSE202280_pool_matrix.mtx.gz`，约 117.5 MB；
- `barcodes.tsv.gz`、`features.tsv.gz`；
- `GSE202280_samples_564_names.xlsx`，用于 barcode/样本映射。

因此生物统计单位是 **6 只小鼠**，不是 2 个测序库，也不是单细胞数。必须先按 hashtag/barcode demultiplex，才能做每只小鼠 pseudobulk。

### 3.4 ST002740：42 个非靶向代谢组样本

Metabolomics Workbench 页面逐样本列出 42 个 LC-MS 样本：

- Acss2−/− CD8⁺ T 细胞：100 或 500 μM ^13C2-acetate，1 或 2 h，部分 500 μM/2 h 加 ACSS2 inhibitor；
- WT CD8⁺ T 细胞：对应 100/500 μM、1/2 h，部分加 inhibitor；
- WT NK 细胞：100 μM/1 h 与 500 μM/2 h；
- 多数组合为 3 个重复，WT 部分组合包含两个命名批次，故总数不是简单的全因子 2×2×2×3。

平台为 HILIC–LC、Thermo Q Exactive HF-X Orbitrap，全扫描 polarity switching；raw Thermo 文件可下载，亦提供 mwTab text/JSON 和 named-metabolite data。

### 3.5 外部单细胞数据

- GSE164898：11 位健康供体，但 GEO 以 8 个测序记录归档，部分 donor pooled；
- GSE176078：26 个原发乳腺肿瘤，含 11 ER⁺、5 HER2⁺、10 TNBC；原始人数据受控于 EGA `EGAS00001005173`，GEO提供处理数据。

本文只重分析 ACSS1/ACSS2 表达，不能把这两个外部 atlas 的所有细胞算作本文新生成数据。

### 3.6 推荐下载方式

1. bulk：直接取 GSE202279 count table并按模型分层。
2. scRNA：下载 MTX 三件套和样本 barcode Excel，先去多重再注释。
3. 代谢：快速复用取 mwTab/命名代谢物；做同位素峰重提取再下载 raw zip。
4. 外部人图谱：优先使用 GEO processed objects；需要原始患者 reads 时按 EGA 申请。

## 4. 主要结果

ACSS2 缺失或抑制限制肿瘤生长，在免疫完整宿主中效果更强，并增强化疗。TME 单细胞显示 T 细胞活化/分化；乙酸补充提高 T 细胞增殖和效应功能。CD8⁺ T 细胞以 ACSS1 利用乙酸，形成与肿瘤 ACSS2 的代谢分工。

## 5. 机制理解

关键不是“乙酸对所有细胞都有益”，而是不同细胞使用不同酶：肿瘤依赖 ACSS2 同化乙酸，T 细胞可通过 ACSS1 氧化乙酸。ACSS2 抑制因而改变底物竞争，把代谢资源从肿瘤转给免疫细胞。

## 6. 推荐重点阅读的图

- 免疫缺陷与免疫完整宿主的 ACSS2i 疗效对照。
- 6 小鼠 multiplex scRNA TME 图谱。
- ^13C2-acetate 示踪和 ACSS1 依赖实验。
- ACSS2i 与化疗联合。

## 7. 创新性

提出“肿瘤代谢抑制剂也是免疫代谢调节剂”的双效框架，并用同位素代谢组把底物再分配直接连接到 T 细胞功能。

## 8. 局限性

单细胞只有 6 只小鼠并汇成两个测序库；代谢组设计非完全平衡。肿瘤间 ACSS1/ACSS2 表达与乙酸供给差异可能影响外推。药物对其他组织乙酸代谢的影响仍需评估。

## 9. 在综述中的定位

适合作为“重定向肿瘤—免疫细胞营养竞争”的代表，与葡萄糖、乳酸、谷氨酰胺和甲酸/乙酸代谢干预比较。

## 10. 可直接写入综述的表述

> 抑制肿瘤 ACSS2 不仅阻断癌细胞乙酸同化，还提高微环境乙酸可用性，使 CD8⁺ T 细胞通过 ACSS1 加强氧化代谢和效应功能，从而形成代谢—免疫双重抗肿瘤作用。

## 11. 数据复用建议

用 14 个 bulk 样本做 host immune status × treatment 交互；6 个 scRNA 样本做 mouse-level pseudobulk；42 个代谢样本按 genotype、cell type、dose、time、inhibitor 建模。不要把 pooled library 当生物重复。

## 12. 转化与安全性关注

需评估 ACSS2i 在肝脏、脂肪和免疫细胞中的系统代谢影响，以及与化疗联用的骨髓抑制。患者选择可能依赖肿瘤 ACSS2、免疫细胞 ACSS1 和微环境乙酸供给的联合标志物。

## 13. 避免误读

- GSE202281 的 16 GSM 中，scRNA 两个库代表 6 只小鼠。
- ST002740 有 42 个样本，但不是完全平衡的全因子设计。
- 外部 GSE164898/GSE176078 是再分析数据，不是本文新队列。
- 乙酸效应依赖细胞类型和 ACSS1/2，不应概括为“补乙酸即可抗癌”。

