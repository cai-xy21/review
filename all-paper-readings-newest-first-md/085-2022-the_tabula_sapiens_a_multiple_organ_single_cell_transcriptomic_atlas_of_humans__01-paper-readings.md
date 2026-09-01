# 《The Tabula Sapiens: A multiple-organ, single-cell transcriptomic atlas of humans》精读（T 细胞视角）

## 论文信息

- 作者：Tabula Sapiens Consortium
- 期刊：*Science*，2022；376(6594): eabl4896
- DOI：10.1126/science.abl4896
- 原文：[Science](https://www.science.org/doi/10.1126/science.abl4896)
- 开放全文：[PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC9812260/)
- 数据：[Figshare collection](https://figshare.com/projects/Tabula_Sapiens/100973)；[GEO GSE201333](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE201333)；[AWS Open Data](https://registry.opendata.aws/tabula-sapiens/)
- 代码：[GitHub—czbiohub/tabula-sapiens](https://github.com/czbiohub/tabula-sapiens)

## 一句话结论

Tabula Sapiens 以 15 名供者的 59 份标本建立 24 个器官、483,152 个细胞的全身参考图谱；其 T 细胞价值在于同一供者多器官设计，使跨组织克隆共享可与组织特异表达直接比较。

## 1. 数据护照

| 维度 | 规模/内容 | 含义 |
|---|---:|---|
| 供者 | 15 | 多组织来自同一供者；平均年龄约 51 岁，性别较平衡 |
| 标本 | 59 | 不是每名供者都有全部器官 |
| 器官/组织 | 24 | 血、骨髓、脾、淋巴结、胸腺及多种实质器官 |
| QC 后细胞 | 483,152 | 其中免疫细胞 264,824 |
| 外周常规 T 细胞 | 82,018 | 汇总 generic T、CD4/CD8、helper、effector、memory、naive、Treg、Tfh；不含 NKT 与胸腺发育细胞 |
| NKT | 15,653 | `mature NK T cell` 10,339 + `type I NK T cell` 5,314；因定义有争议，建议单列 |
| 胸腺 T 系细胞 | 3,364 | DN1、DN3、DN4、双阳性 thymocyte 及未细分 thymocyte |
| 全部 T-lineage | 101,035 | 外周常规 T + NKT + 胸腺 T 系细胞；占全图谱约 20.9% |
| 细胞类型 | 475 | 专家使用统一 ontology 注释 |
| 主技术 | Smart-seq2 + 10x scRNA-seq | 全长与 droplet 数据并存 |
| TCR/BCR | 重点在供者 TSP2 的受体序列组装 | 不是全体 15 名供者的标准化 10x VDJ 图谱 |

### T 细胞数量的口径说明

- 原始对象中，名称**恰好等于** `T cell` 的细胞只有 14,682 个；这只是未进一步细分的通用标签，不能当作全部 T 细胞。
- 用于图谱横向比较，建议写 **82,018 个外周常规 T 细胞**；若研究对象覆盖 NKT 和胸腺发育细胞，则写 **101,035 个 T-lineage cells**，并在脚注注明纳入范围。
- 上述数字按 CELLxGENE Census `2023-12-15` 快照中的原始 Tabula Sapiens Immune 对象逐类汇总，以对应 2022 年论文。CELLxGENE 当前 collection 可能包含后续版本，细胞数不可与原论文直接混用。

## 2. 对 T 细胞图谱的独特价值

### 2.1 同一供者跨组织比较

血液、淋巴器官和实质器官来自同一人，降低遗传背景和暴露史的混杂。它适合回答“同一类 T 细胞在不同组织如何改写表达”。

### 2.2 跨器官克隆共享

作者从 TSP2 组装 TCR 序列，发现大克隆常分布于多个器官，并展示多个 T 细胞亚型之间的克隆关系。该结果说明驻留状态并不意味着克隆只存在于单一器官。

### 2.3 它是全身背景，不是 T 细胞精细图谱

475 种全身细胞类型保证广度，却牺牲 T 细胞状态深度。Tex、Tpex、TRM 等精细命名应映射到专门 T 细胞参考图谱，而不是只用 Tabula Sapiens 原注释。

## 3. TCR 数据边界

- 论文的受体/克隆展示主要聚焦 TSP2，不能把 483,152 细胞理解为都有 TCR。
- 从 scRNA reads 组装 TCR 与专门 10x VDJ 的敏感度不同，缺失序列不代表没有 TCR。
- 跨组织相同 clonotype 支持共同克隆来源，但不能确定迁移方向或组织间交通顺序。

## 4. 数据获取

- **处理矩阵/metadata**：[Figshare](https://figshare.com/projects/Tabula_Sapiens/100973)；常见 `.h5ad`、`.rds` 与分组织对象。
- **GEO**：[GSE201333](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE201333)。
- **原始大文件**：[AWS Open Data](https://registry.opendata.aws/tabula-sapiens/)。
- **在线浏览**：可从论文或 CELLxGENE collection 进入。
- T 细胞复用先筛 `compartment == immune` 与 T lineage，再保留 `donor、tissue、method、sample`；Smart-seq2 和 10x 不宜直接比较原始 counts。

## 5. 推荐图版

- **Fig. 1**：15 名供者、24 器官、近 50 万细胞的数据设计。
- **Fig. 3A**：T 细胞克隆跨组织分布；本章节首选。
- 全局 cell ontology/organ composition 图：适合说明该资源提供全身背景。

## 6. PPT 单页格式

**标题**：同一供者多器官图谱揭示 T 细胞克隆的全身分布

**正文**：15 名供者、24 个器官、483,152 个细胞；其中外周常规 T 细胞 82,018 个，若纳入 NKT 与胸腺发育细胞则共有 101,035 个 T-lineage cells；TSP2 受体组装显示大 T 细胞克隆可跨器官存在。

**配图**：Fig. 1 + Fig. 3A。

**页脚引用**：Science 2022, Tabula Sapiens Consortium。

## 7. 局限性

- 供者数有限且器官覆盖不平衡，部分组织仅 1–2 名供者。
- 围手术期/器官供者背景不等于完全健康普通人群。
- TCR 覆盖集中于特定供者，跨人群 repertoire 结论有限。
- 细胞类型广而不深，需专门 T 细胞图谱做二次精注释。

## 8. 可直接用于综述

> Tabula Sapiens 的同一供者多器官设计将 T 细胞状态置于全身组织背景中，并显示大克隆可跨多个器官分布；它提供的是组织生态坐标，而非高分辨率 T 细胞状态终表（Science 2022, Tabula Sapiens Consortium）。
