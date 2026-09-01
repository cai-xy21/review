# 《A spatial human thymus cell atlas mapped to a continuous tissue axis》精读

## 论文信息

- 第一作者：Nadav Yayon、Veronika R. Kedlian、Lena Boehme（共同第一作者）
- 期刊：*Nature*，2024；635: 708–718
- DOI：10.1038/s41586-024-07944-6
- 原文：[Nature](https://www.nature.com/articles/s41586-024-07944-6)
- 开放全文：[PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC11578893/)
- 图谱对象：[CELLxGENE](https://cellxgene.cziscience.com/collections/fc19ae6c-d7c1-4dce-b703-62c5d52061b4)
- 代码：[thymus_spatial_atlas](https://github.com/Teichlab/thymus_spatial_atlas)；[TissueTag](https://github.com/Teichlab/TissueTag)

## 一句话结论

研究整合约 482,651 个单细胞、28 张 Visium 切片和超过 110 万个 IBEX 分割细胞/细胞核，建立从胎儿到幼儿的人胸腺多模态空间图谱，并以连续皮质—髓质轴将不同年龄、切片和技术映射到统一解剖坐标。

## 1. 研究问题

胸腺 T 细胞发育同时是“分子状态变化”和“在皮质—髓质间迁移”。离散组织标签无法精确比较不同大小、形态和年龄的胸腺。作者因此建立可量化的 Cortico-Medullary Axis（CMA/OrganAxis）。

## 2. 数据护照

| 模态 | 规模 | 关键用途 |
|---|---:|---|
| 公开 scRNA 数据 | 20 名供者，266,551 cells | 扩大胎儿/幼儿发育覆盖 |
| 新 CITE-seq | 5 名供者，146,352 cells；143 个表面蛋白 | RNA + 蛋白 + 部分 αβ TCR |
| 新基质富集 scRNA | 4 名供者，69,748 cells | 增强 TEC、成纤维和血管细胞 |
| 整合单细胞总量 | 482,651 cells | 12 名胎儿 + 17 名幼儿供者，共 29 名 |
| T cell / thymocyte subset | **391,462 cells** | 约占整合单细胞的 81.1%；主要是不同发育阶段的胸腺 T 系细胞，并非 39 万个成熟外周 T 细胞 |
| Visium | 28 张：胎儿 12、幼儿 16 | 其中新生成胎儿 9、幼儿 16，另整合公开胎儿 3 |
| IBEX | 44-plex，8 名供者/样本，1,101,631 个分割核 | 高维蛋白空间验证 |
| 其他空间验证 | 14-plex RareCyte、RNAscope | 验证细胞和分子定位 |

注意：三类单细胞数据供者有重叠，不能把 20+5+4 写成 29 个完全独立新增供者；论文整合口径为 29 名供者。

## 3. 方法核心

### 3.1 TissueTag

用 H&E 或虚拟组织图像半自动标注皮质、髓质、包膜/隔、血管、Hassall 小体等结构。

### 3.2 OrganAxis/CMA

把每个 spot 或细胞到解剖边界的距离转为连续坐标，使不同切片、年龄和模态可以在同一“皮质—髓质位置”上比较。

### 3.3 多模态映射

- cell2location 将单细胞参考状态反卷积到 Visium spots。
- CITE-seq 的 RNA/ADT 共同构建 T 细胞发育 WNN 空间。
- Slingshot 给出 DP 正选择到 CD4/CD8 成熟端点的伪时间。
- IBEX 以蛋白定位验证 TEC 与胸腺微区。

## 4. 关键发现

1. 连续 CMA 能跨胎儿和幼儿胸腺捕捉主要空间变异，优于只用离散皮质/髓质标签。
2. αβ T 细胞不同发育阶段沿 CMA 呈连续位置变化，发育伪时间与空间轴相互印证。
3. 胎儿和幼儿胸腺在趋化因子、细胞因子和上皮亚群定位上存在系统差异。
4. 某些 TEC 亚群和祖细胞生态位只有在连续位置与高维成像中才清楚。

## 5. TCR 数据

CITE-seq 采用 10x 5′ GEX + Feature Barcode + Human TCR kit；有黑圈标记的样本进行了 αβ TCR-seq。TCR 主要用于界定重排状态和胸腺发育阶段，而不是做百万级外周克隆扩增图谱。下载后应按样本确认 TCR 可用性，不能用整合单细胞总量作 TCR 分母。

## 6. 数据获取

| 数据 | 入口 |
|---|---|
| 注释 scRNA 与 Visium 对象 | [CELLxGENE collection](https://cellxgene.cziscience.com/collections/fc19ae6c-d7c1-4dce-b703-62c5d52061b4) |
| 新 scRNA/Visium 原始测序 | [ENA PRJEB77091](https://www.ebi.ac.uk/ena/browser/view/PRJEB77091) |
| 受控样本 | [EGA EGAD00001015384](https://ega-archive.org/datasets/EGAD00001015384) |
| CITE-seq | [GEO GSE271304](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE271304) |
| Visium/IBEX/RNAscope/RareCyte 图像 | [BioImage Archive S-BIAD1257](https://www.ebi.ac.uk/biostudies/bioimages/studies/S-BIAD1257) |
| 公开输入数据 | E-MTAB-8581、GSE206710、GSE159745、GSE147520、E-MTAB-11341 |
| 代码与教程 | [GitHub](https://github.com/Teichlab/thymus_spatial_atlas)、[OrganAxis tutorial](https://organ-axis-tutorial.readthedocs.io/) |

分析 AnnData 时重点检查 `age、donor、technology、cell_type levels、CMA、sample`；Visium 另查 `spatial` 坐标和图像比例；CITE-seq 查 RNA/ADT/TCR 的共同条码交集。

## 7. 推荐图版

- **Fig. 1**：所有模态、样本和细胞规模；数据页首选。
- **Fig. 2**：TissueTag + CMA/OrganAxis；方法创新首选。
- **Fig. 3**：T 谱系阶段沿 CMA 的空间分布；本章节生物学首选。
- **Fig. 4**：TEC 亚群的年龄与空间差异；讲生态位时使用。

若只能放一张：选 **Fig. 3**；若需要解释“空间图谱如何跨切片比较”，选 **Fig. 2**。

## 8. PPT 单页格式

**标题**：连续解剖轴把 T 细胞发育状态放回胸腺空间

**正文**：482,651 个单细胞，其中 391,462 个 T 系/胸腺细胞；另有 28 张 Visium 和 110 万 IBEX 分割核。CMA 统一胎儿与幼儿胸腺，显示 T 细胞发育伪时间与皮质—髓质迁移相互对应。

**配图**：Fig. 2 方法示意 + Fig. 3 T lineage spatial mapping。

**页脚引用**：Nature 2024, Yayon。

## 9. 局限性

- Visium spot 不是单细胞，反卷积依赖单细胞参考和注释精度。
- CMA 将复杂 2D/3D 结构压缩为一条主轴，会损失血管、隔与局部微区信息。
- 伪时间与空间共变仍不是直接活体迁移追踪。
- 样本集中在胎儿和 0–3 岁幼儿，不能代表成年和老年胸腺。

## 10. 可直接用于综述

> 多模态空间胸腺图谱通过连续皮质—髓质轴对齐约 48 万个单细胞、28 张 Visium 切片和高维蛋白成像，使 T 细胞发育状态能够在跨年龄、跨切片的共同解剖坐标中比较（Nature 2024, Yayon）。
