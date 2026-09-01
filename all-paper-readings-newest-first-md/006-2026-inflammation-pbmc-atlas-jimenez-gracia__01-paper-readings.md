# 《Interpretable inflammation landscape of circulating immune cells》精读

## 论文信息

- 第一作者：Laura Jiménez-Gracia、Davide Maspero、Sergio Aguilar-Fernández、Francesco Craighero（共同第一作者）
- 期刊：*Nature Medicine*，2026；32: 633–644
- DOI：10.1038/s41591-025-04126-3
- 原文：[Nature Medicine](https://www.nature.com/articles/s41591-025-04126-3)
- 开放全文：[PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12920142/)
- 处理图谱：[Zenodo 14851901](https://doi.org/10.5281/zenodo.14851901)
- 代码：[GitHub](https://github.com/Single-Cell-Genomics-Group-CNAG-CRG/Inflammation-PBMCs-Atlas)

## 一句话结论

研究整合 1,047 名患者/参与者、19 种疾病的 6,340,934 个 QC 后 PBMC，构建 64 类循环免疫状态和 119 个细胞类型特异炎症因子，并用可解释模型把新患者投影到统一炎症坐标。

## 1. 数据护照

| 维度 | 规模/内容 | 含义 |
|---|---:|---|
| QC 后 PBMC | 6,340,934 | 全部分析对象；原始汇总超过 650 万 |
| 参与者 | 1,047 | 56% 女性、43% 男性；不同年龄 |
| 疾病 | 19 | 7 IMID、1 急性、3 慢性炎症、4 感染、4 实体瘤，另含健康对照 |
| 主训练集 | 4,435,922 cells | 817 名 QC 后患者/参与者；Fig. 1 主 UMAP |
| 验证设计 | unseen patients + unseen studies | 区分同研究新患者与跨研究迁移 |
| 细胞注释 | 64 个 Level 2 群体 | 覆盖适应性与先天免疫 |
| 炎症程序 | 21 初始 signatures → 119 个 cell-type-specific factors | Spectra 精炼 |
| 技术 | 10x 3′/5′、CellPlex、基因型 multiplex 等 | 跨平台整合是核心难点 |
| TCR | 非核心模态 | 该图谱不能直接分析 clonotype 或抗原特异性 |

## 2. 疾病覆盖

- IMID：SLE、RA、PsA、银屑病、UC、CD、MS。
- 急性炎症：sepsis。
- 慢性炎症：COPD、哮喘、肝硬化。
- 感染：流感、COVID-19、HBV、HIV。
- 实体瘤：乳腺癌、结直肠癌、鼻咽癌、头颈鳞癌。

## 3. 方法框架

1. 原始 FASTQ 可得时统一处理，否则纳入公开 count matrix。
2. scVI/scANVI 学习跨研究、疾病、年龄和性别的共同潜在空间。
3. 自上而下细分为 64 个免疫群体。
4. 以 Spectra 将炎症相关基因集合精炼成 119 个细胞类型特异因子。
5. 将每名患者视为跨细胞表达分布的 ensemble，构建疾病分类/解释模型。

## 4. 关键发现

- 多种疾病共享干扰素、细胞毒、迁移和抗原呈递程序，但在不同细胞类型中的部署方式不同。
- 疾病分类信息既来自细胞组成，也来自相同细胞群内的炎症程序。
- 以 unseen studies 验证比随机细胞划分更接近真实部署，凸显跨平台泛化难度。
- 对 T 细胞而言，该图谱提供“循环炎症状态背景”，不能替代组织 TIL/TRM 或配对 TCR 图谱。

### 为什么实体瘤研究可以测 PBMC？

这里测量的不是肿瘤本体，而是实体瘤造成的**系统性免疫响应**。肿瘤及其间质释放的细胞因子、趋化因子和抗原可经血液或淋巴影响引流淋巴结、骨髓与循环免疫细胞；同时，免疫细胞会在血液、淋巴器官和肿瘤之间迁移。因此，PBMC 能记录细胞比例、IFN/TNF–NF-κB 等炎症程序、单核细胞抑制状态及部分活化 T 细胞变化，是可重复采血的“液体免疫活检”。论文在实体瘤中例如观察到 CRC 和 NPC 的循环免疫细胞 TNF–NF-κB 程序增强，以及 HNSCC 非经典单核细胞 SP1 活性增强。

但 PBMC 不能给出肿瘤内 TIL 的组成、空间生态位、耗竭状态或肿瘤反应性。血中某个 T 细胞发生变化也不等于它识别肿瘤；本图谱没有统一 TCR，且未以配对肿瘤—血液克隆追踪为设计核心。故应将四种实体瘤样本理解为“癌症患者的循环免疫图谱”，而不是“实体瘤微环境图谱”。

## 5. 数据获取

- 统一 QC 后 counts + metadata：[Zenodo 14851901](https://doi.org/10.5281/zenodo.14851901)。
- 自建原始数据：GEO SuperSeries [GSE248688](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE248688)，下含 GSE248689、GSE248695、GSE248685、GSE248693、GSE270165。
- 公开输入研究 accession：Supplementary Table 1 Sheet 1，来自 GEO/SRA、ArrayExpress、DUOS、Synapse、GSA、CELLxGENE、10x。
- 全流程代码：[GitHub](https://github.com/Single-Cell-Genomics-Group-CNAG-CRG/Inflammation-PBMCs-Atlas)。
- 复用时按 `study、patient、disease、treatment、age、sex、chemistry` 分层；不得将 630 万细胞当 630 万独立重复。

## 6. 推荐图版

- **Fig. 1**：650 万级数据设计、主/验证集、64 类注释；数据规模页首选。
- **炎症因子图**：展示 119 个 cell-type-specific factors；讲“状态程序跨疾病复用”。
- **患者投影/分类图**：讲 atlas-to-patient translation；保留 unseen-study 验证结果。

## 7. PPT 单页格式

**标题**：630 万 PBMC 建立跨 19 种疾病的循环炎症坐标

**正文**：1,047 人；6,340,934 个细胞；64 个免疫群体；119 个细胞类型特异炎症因子；新患者可投影到统一参考。

**配图**：Fig. 1 + 炎症 factor heatmap。

**页脚引用**：Nature Medicine 2026, Jiménez-Gracia。

## 8. 局限性

- 只有外周血，无法直接描述组织位置、TCR 克隆或肿瘤抗原。
- 疾病严重度、治疗、采样时点和研究平台高度异质。
- 公共矩阵并非全部从 FASTQ 统一处理。
- 分类性能不等于已可临床诊断，仍需前瞻性多中心验证。

## 9. 可直接用于综述

> 超过 630 万个 PBMC 的炎症图谱表明，跨疾病共享的不是单一“炎症细胞”，而是被部署在不同免疫谱系中的可复用分子程序；患者级分析必须同时建模细胞组成和状态内活性（Nature Medicine 2026, Jiménez-Gracia）。
