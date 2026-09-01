# 《Dysfunctional CD8 T cells form a proliferative, dynamically regulated compartment within human melanoma》精读

## 论文信息

- **作者**：Li H, van der Leun AM, Yofe I, et al.
- **期刊与年份**：Cell, 2019（2018 年在线发表）
- **DOI**：[10.1016/j.cell.2018.11.043](https://doi.org/10.1016/j.cell.2018.11.043)
- **原文**：[Cell](https://www.cell.com/cell/fulltext/S0092-8674(18)31570-8)
- **PubMed**：[PMID 30595452](https://pubmed.ncbi.nlm.nih.gov/30595452/)
- **开放全文**：[PMC7253294](https://pmc.ncbi.nlm.nih.gov/articles/PMC7253294/)
- **开放处理数据**：[GSE123139](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE123139)
- **受控原始数据**：[EGA EGAS00001003363](https://ega-archive.org/studies/EGAS00001003363)
- **代码**：[MetaCell](https://bitbucket.org/tanaylab/metacell/src/default/)
- **本报告核对日期**：2026-08-23

## 一句话结论

本文在 25 例人黑色素瘤中用 MARS-seq 联合 FACS 蛋白索引分析 47,772 个细胞，并以耦合单细胞 TCR 测序连接克隆与状态，发现“dysfunctional”CD8 T 细胞不是静止终点，而是一个可增殖、动态调控且具有连续内部异质性的肿瘤内 compartment。

## 数据护照

| 项目 | 内容 |
|---|---|
| 队列 | 25 名黑色素瘤患者，覆盖 stage II/III/IV 和多种既往治疗；其中 9 例为未治疗 stage III 皮下病灶 |
| 总细胞 | 47,772：46,612 个免疫细胞 + 1,160 个恶性细胞 |
| 技术 | MARS-seq 3′ plate-based scRNA；FACS 蛋白 index；部分采用耦合 scTCR 变体 |
| 聚合表示 | 324 个 metacells |
| T/NK | 29,825 个细胞，218 个 metacells，分为 9 个主要 group |
| CD8 | 145 个 CD8 metacells；重点比较 GZMK transitional、GZMH cytotoxic、checkpoint-high dysfunctional |
| 血液配对 | 3 名患者（p13、p17、p27）共 17,989 个肿瘤/血液细胞，用于 compartment 比较 |
| 开放数据 | GSE123139：分文库处理文件、TCRβ 表 |
| 原始数据 | EGAS00001003363，受控访问 |

## 1. 研究问题

人肿瘤中的 CD8“dysfunction/exhaustion”究竟是不可逆、低增殖的终末状态，还是包含持续更新和动态变化的细胞群？细胞毒、过渡和 dysfunctional 状态如何与 TCR 克隆大小、增殖和肿瘤特异性相关？

## 2. 方法框架

1. 对肿瘤细胞悬液进行多标志 FACS，并保存 index sorting 的表面蛋白信息。
2. 用 MARS-seq 获取大量细胞 3′ 转录组。
3. 用 MetaCell 将相似细胞聚合成稳健 metacells，减少稀疏性对状态解释的影响。
4. 在 T/NK compartment 中定义 transitional、cytotoxic、dysfunctional 等连续程序。
5. 用耦合 scTCR 分析克隆扩增、克隆内状态和增殖；用血液配对区分循环与肿瘤富集状态。

## 3. 数据规模与图谱组成

### 3.1 细胞、metacell 与患者三个层级

| 层级 | 规模 | 说明 |
|---|---:|---|
| 患者 | 25 | 临床分期、病灶和既往治疗异质 |
| 单细胞总数 | 47,772 | 46,612 immune + 1,160 malignant |
| 全图 metacells | 324 | 模型聚合单位，不是额外测得的细胞类型 |
| T/NK 单细胞 | 29,825 | 核心 T 细胞状态分析 |
| T/NK metacells | 218 | 分为 9 个主要 group |
| CD8 metacells | 145 | 聚焦 transitional/cytotoxic/dysfunctional 轴 |
| 血液配对子集 | 17,989 cells，3 名患者 | 肿瘤与 PBMC 比较，不代表全部 25 名患者均有配对血液 |

MetaCell 把局部相似细胞聚为相对稳定的表达单元；因此论文图上的一个点/列有时代表 metacell 而不是单细胞。复用时必须区分单细胞矩阵和 metacell 聚合结果。

### 3.2 T/NK 图谱的 9 个 group

图谱覆盖初始/记忆、GZMK+ transitional、GZMH+ cytotoxic、checkpoint-high dysfunctional、增殖 T、CD4 Treg/Tfh、NK 等主要状态。文章最关键的结构不是固定的 9 标签，而是 CD8 沿 **transitional—cytotoxic—dysfunctional** 相关程序的连续变化。dysfunctional 群内部仍有增殖和状态差异，并非统一静止终点。

### 3.3 GSE123139 的组织方式

GEO 系列含约 **204 个 GSM 文库/分选板记录**。204 不是患者数，而是板、文库或实验批次层面的记录。系列级文件包括：

- `GSE123139_RAW.tar`，约 **146.76 MB**：每个 GSM 的处理后表达/注释文件集合；解包后需依据文件名和元数据重组细胞矩阵。
- `GSE123139_T_cells_tcrb_v2.txt.gz`，约 **0.59 MB**：T 细胞 TCRβ 信息及其细胞映射，是开放克隆分析的直接入口。

GEO 没有提供一个类似 10x `filtered_feature_bc_matrix.h5` 的整合对象，也不能从 GSM 数量直接推断生物学样本数。原始 reads 和受保护患者信息位于 **EGAS00001003363**。

### 3.4 公开数据能回答什么

| 目标 | 文件/入口 | 可复现程度 |
|---|---|---|
| 重建表达 atlas | GSE123139 RAW.tar | 需合并多个板/文库并恢复 index/细胞元数据 |
| 复现 metacells | 表达数据 + MetaCell 代码 | 参数和 QC 选择会影响聚合结果 |
| TCRβ 克隆大小 | `T_cells_tcrb_v2` | 开放可用；需与表达 cell ID 对齐 |
| 配对 α/β 或从 reads 重建 | EGA 原始数据 | GEO 的系列级 TCRβ 表不等同于完整 5′ 配对 VDJ |
| FACS 蛋白 index | GEO/补充元数据 | 检查逐文库文件；不是所有细胞均有同样面板 |

### 3.5 下载与读取示例

```powershell
Invoke-WebRequest `
  -Uri "https://ftp.ncbi.nlm.nih.gov/geo/series/GSE123nnn/GSE123139/suppl/GSE123139_T_cells_tcrb_v2.txt.gz" `
  -OutFile "GSE123139_T_cells_tcrb_v2.txt.gz"
```

```python
import pandas as pd
tcr = pd.read_csv(
    "GSE123139_T_cells_tcrb_v2.txt.gz",
    sep="\t", compression="gzip"
)
print(tcr.columns)
print(tcr.head())
```

下载 `RAW.tar` 后应先列出归档内容，再按 GSM/板映射合并；不建议在未检查路径时批量覆盖同名文件。

## 4. Dysfunctional compartment 的定义

该状态由多个 checkpoint、组织适应和效应相关基因共同定义，而不是 PD-1 单阳性。作者发现 dysfunctional program 内部仍存在连续程度和增殖细胞，挑战“耗竭=完全不能增殖”的简化观点。术语 dysfunctional 与 exhausted 部分重叠，但不能在不同论文间不加说明地完全互换。

## 5. 克隆扩增与动态性

较大的 TCR 克隆更常出现在 dysfunctional compartment，且可包含增殖细胞；同克隆细胞也可跨相邻状态分布。这说明慢性抗原刺激可能同时驱动克隆扩增和功能重塑。然而，静态 TCR 共享不能单独证明细胞由某状态转到另一状态，动态性是多证据推断。

## 6. 血液—肿瘤比较

三名患者的 17,989 细胞配对子集显示，dysfunctional program 更偏肿瘤，而细胞毒/循环状态可在血液中出现。这个结果有助于区分“体内肿瘤状态”和可从外周采集的制造起始状态，但病例数只有 3，不能直接确定最佳细胞治疗采样来源。

## 7. 主要生物学发现

- dysfunctional CD8 compartment 可增殖且内部状态连续。
- 大克隆与 dysfunctional 状态富集相关，但克隆内仍有表型差异。
- GZMK transitional、GZMH cytotoxic 与 checkpoint-high dysfunctional 构成有联系但非简单直线的景观。
- 结合转录、FACS protein index 和 TCR 可更准确描述功能状态。

## 8. 推荐精读图

- 全部 47,772 细胞及 324 metacells 总览。
- 29,825 个 T/NK 细胞的 218 metacells 与 9 groups。
- CD8 transitional/cytotoxic/dysfunctional 连续程序图。
- TCR 克隆大小、增殖与状态的关联图。

## 9. 方法学创新

1. MetaCell 在大规模稀疏数据中构建可解释的局部状态单元。
2. 同时利用转录、FACS index protein 和耦合 scTCR。
3. 把耗竭从静态终点重新表述为动态、可增殖 compartment。

## 10. 局限性

- 队列临床异质性较大，不同治疗史可能影响状态。
- MARS-seq 与现代 10x 5′ VDJ 的数据结构不同。
- metacell 聚合提高稳定性，但可能遮蔽稀有细胞和细胞内变异。
- 血液配对仅 3 名患者。
- 横断面数据没有直接记录单细胞状态随时间变化。

## 11. 在综述架构中的位置

适合用于“**T cell is at the start point and the frontier of cell therapy**”和“**optimize conditions for navigating T cell states**”：工程化制造不应只避免所有 checkpoint 表达，而应区分可扩增、可塑的抗原经历状态与真正稳定的终末功能障碍。

## 12. 可直接写入综述的表述

> Li 等在 25 例黑色素瘤的 47,772 个单细胞中，以 29,825 个 T/NK 细胞和耦合 TCR 解析 CD8 状态，发现 checkpoint-high dysfunctional compartment 仍包含增殖细胞并与大型克隆富集相关，提示耗竭并非单一静止终点，而是可动态调控的状态集合。

## 13. 避免误读

- 324 和 218 是 metacell 数，不是细胞类型数。
- GSE123139 的 204 个 GSM 是实验文库/板记录，不是 204 名患者。
- 开放文件是 TCRβ 表，不应描述为所有细胞均有完整配对 αβ VDJ。
- “动态”来自克隆、增殖和连续状态证据，不是活细胞时间序列。
