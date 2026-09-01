# 《Integrated drug profiling and CRISPR screening identify essential pathways for CAR T-cell cytotoxicity》精读

## 论文信息

- **题目**：Integrated drug profiling and CRISPR screening identify essential pathways for CAR T-cell cytotoxicity
- **作者**：Dufva et al.
- **期刊与年份**：Blood，2020
- **DOI**：[10.1182/blood.2019002121](https://doi.org/10.1182/blood.2019002121)
- **PMID / PMC**：[31830245](https://pubmed.ncbi.nlm.nih.gov/31830245/) / [PMC7098811](https://pmc.ncbi.nlm.nih.gov/articles/PMC7098811/)
- **主要数据入口**：论文 Supplementary Data；正文未给出 GEO/SRA accession
- **研究类型**：526-compound 高通量药物筛选 + TCR 转录因子 reporter 筛选 + NALM6 靶细胞 genome-wide CRISPR knockout 筛选 + 多模型验证

## 一句话结论

本文把药物敏感性、TCR 信号 reporter 与靶细胞 CRISPR 筛选整合起来，发现 **SMAC mimetics 可通过死亡受体/FADD/RIPK1 相关通路增强 CAR-T 杀伤**，同时系统展示了药物既可作用于效应 T 细胞，也可通过改变肿瘤靶细胞的死亡易感性重塑治疗结局。

## 数据护照（先看这一表）

| 项目 | 内容 |
|---|---|
| 药物库 | 526 种药物：165 种 FDA/EMA-approved，361 种 investigational/preclinical |
| 剂量设计 | 每种药物 5 个浓度，覆盖约 10,000-fold 浓度范围 |
| 主药筛体系 | NALM6-luc 单独培养及其与 CD19 CAR-T 共培养，384-well、24 h |
| TCR reporter | Jurkat NFAT、NF-κB、AP-1 reporter；TCS 刺激 ± CD80 |
| 临床样本验证 | 18 例 primary B-ALL samples |
| 细胞系扩展验证 | B-ALL 与 DLBCL 多细胞系 panel |
| CRISPR screen | NALM6-Cas9 靶细胞中的 GeCKO v2 genome-wide knockout screen；不是 CAR-T 内筛选 |
| CRISPR 条件 | NALM6 alone；CAR-T E:T 1:35；CAR-T + 10 nM birinapant E:T 1:70 |
| 公共数据 | 主要为 6 个大型 Supplementary `.xlsx` 和 1 个较小表；未发现 raw FASTQ repository accession |
| 关键限制 | 化合物筛选和 CRISPR 筛选作用对象不同；不能把靶细胞 resistance genes 写成 T-cell intrinsic regulators |

## 1. 研究要解决的问题

CAR-T 的杀伤结果由至少三部分共同决定：

1. CAR-T 本身的受体信号和效应状态；
2. 肿瘤细胞对凋亡/坏死性凋亡等死亡程序的敏感性；
3. 同时影响两类细胞的药物和微环境因素。

单独进行 CAR-T 表型分析无法区分这些层次。本文通过药物谱、TCR reporter 和靶细胞 CRISPR screening 三条证据线，寻找既能增强 CAR-T 活性、又能降低肿瘤抗性的组合策略。

## 2. 方法框架：药物—信号—靶细胞遗传三层整合

### 2.1 526-compound 共培养筛选

- 384-well 格式。
- NALM6-luc 作为 CD19 阳性靶细胞。
- 比较药物对 NALM6 单独培养和 NALM6 + CD19 CAR-T 共培养的影响。
- 每种药物 5 个浓度，跨度约 10,000 倍。
- 24 h 后通过发光/活性读出计算 dose-response 和 drug sensitivity score（DSS）。
- 通过共培养与单培养的差异 DSS 区分“直接杀肿瘤”与“CAR-T 协同增敏”。

### 2.2 TCR 转录因子 reporter 筛选

作者使用 Jurkat reporter 系统分别读出 NFAT、NF-κB 和 AP-1，在 T cell stimulator cells（TCS）刺激、以及 TCS + CD80 共刺激条件下测试同一药物库。该层用于判断药物是否直接增强或抑制 TCR downstream signaling。

### 2.3 NALM6 靶细胞 CRISPR screen

- 在 NALM6-Cas9 中转入 human GeCKO v2 library。
- puromycin selection 从转导后约 48 h 开始，持续约 6 d。
- 第 8 d 分成三种条件：
  - NALM6 only；
  - CD19 CAR-T + NALM6，E:T 1:35；
  - CD19 CAR-T + 10 nM birinapant + NALM6，E:T 1:70。
- 9–12 d 后收集 surviving NALM6，PCR 扩增 guide cassette，HiSeq 2000 测序。
- 使用 MAGeCK 做 gene-level enrichment/depletion。

低 E:T 设计使部分肿瘤细胞存活，从而观察哪些 knockout 造成 CAR-T resistance 或 sensitization。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

本文不是单细胞图谱，而是一套 **多维 perturbation-response 数据**：

- `drug × dose × culture context` 的反应曲线；
- `drug × TCR reporter pathway` 的信号调节矩阵；
- `sgRNA/gene × selection condition` 的肿瘤细胞存活富集矩阵；
- primary B-ALL 与多细胞系验证；
- 对公开表达数据集的辅助分析。

三层数据的实验单位不同：药物共培养是 well，reporter 是 Jurkat readout，CRISPR 是 NALM6 靶细胞 guide abundance。整合时应通过 pathway 或 drug mechanism 连接，不能把原始矩阵直接拼接成一个样本表。

### 3.2 药物筛选规模

- 化合物总数：526。
- 已批准药物：165。
- investigational/preclinical：361。
- 每个化合物：5 个浓度。
- 浓度跨度：约 10,000-fold。
- 至少包含两个主 context：NALM6 alone 与 NALM6 + CAR-T。
- 理论最低药物条件数为 `526 × 5 × 2 = 5,260`，尚未计入重复、vehicle controls 和不同 E:T/验证设置。
- primary B-ALL 验证：18 例。

论文的主要定量单位是完整 dose-response curve 和 DSS，而不是单一浓度的生存率。因此二次分析时应优先使用 Supplementary Table 中的曲线/DSS 数据。

### 3.3 reporter 数据的组成

核心 reporter 包括：

- NFAT；
- NF-κB；
- AP-1。

刺激 context 包括 TCS 和 TCS + CD80。这个层次可区分某药物增强 CAR-T 共培养杀伤是否源于直接增强 TCR/costimulation signal。若药物在共培养中增效但不提高 reporter，则更可能通过靶细胞死亡敏感性起作用。

### 3.4 CRISPR screen 的规模与实验结构

#### library

作者使用标准 human GeCKO v2 genome-wide library。论文重心是 gene-level pathway 结果，并未在正文清楚重新定义完整 guide 数；因此精读报告不宜脱离原始 library annotation 强行给出精确 guide 总数。重分析时应从 GeCKO v2 官方 annotation 与 Supplementary Table 5 进行交叉核对。

#### 三种 selection 条件

| 条件 | 目的 |
|---|---|
| NALM6 only | 控制一般增殖/存活必需基因 |
| CAR-T, E:T 1:35 | 找到改变 CAR-T killing resistance/sensitivity 的肿瘤内源基因 |
| CAR-T + 10 nM birinapant, E:T 1:70 | 找到 SMAC mimetic 联合压力下的必需通路 |

关键 hits 包括 **FADD**、**TNFRSF10B**，以及联合 birinapant 条件中的 RIPK1/TNFRSF1A 等死亡通路成员。它们反映的是靶细胞对杀伤程序的反应节点。

### 3.5 Supplementary Data 的公开组成

PMC 页面列出 7 个表格型附件：

| 附件 | 大致大小 | 论文中对应数据内容 |
|---|---:|---|
| Supplementary file 2 | 约 5.0 MB | 大型药物筛选结果表之一 |
| Supplementary file 3 | 约 10.3 MB | reporter/扩展药物矩阵之一 |
| Supplementary file 4 | 约 10.4 MB | 多细胞系或 primary sample 验证数据之一 |
| Supplementary file 5 | 约 3.3 MB | CRISPR/MAGeCK 结果相关表之一 |
| Supplementary file 6 | 约 10.7 MB | 公开表达数据/扩展分析之一 |
| Supplementary file 7 | 约 65.7 KB | 较小资源或引物表 |
| Supplementary PDF | 约 7.1 MB | Supplementary Methods、Figures、Legends |

论文正文明确提到的关键表包括：

- Supplementary Table 1：526-drug coculture response curves；
- Supplementary Table 4：跨 cell-line panel 的验证；
- Supplementary Table 5：CRISPR screen 的 MAGeCK 结果；
- Supplementary Table 6：公开表达数据集分析。

由于 PMC 下载文件名与论文内部 Table 编号可能不是一一同名，整理时必须打开 workbook 查看 sheet name，不能仅按 `supplement-3.xlsx` 猜内容。

### 3.6 是否存在 GEO/SRA 原始数据

本论文 Data availability 主要指向论文及 supplementary materials。当前可核验页面未给出专门的 GEO、SRA 或 ArrayExpress accession 来存放 CRISPR screen FASTQ。

因此可公开复用的核心是：

- drug dose-response/DSS tables；
- reporter matrices；
- gene-level MAGeCK outputs；
- validation tables。

如果研究目标是从 raw FASTQ 重新计数 sgRNA，需要联系作者确认原始测序是否可提供；不能把 Supplementary Table 5 误称为 raw sequencing。

### 3.7 下载方式

#### 论文与附件

从 [PMC7098811](https://pmc.ncbi.nlm.nih.gov/articles/PMC7098811/) 页面进入 **Supplementary Materials**，逐个下载 PDF 和 `.xlsx`。

下载后先列出 workbook：

```python
from openpyxl import load_workbook

wb = load_workbook("supplement.xlsx", read_only=True, data_only=False)
print(wb.sheetnames)
for ws in wb.worksheets:
    print(ws.title, ws.max_row, ws.max_column)
```

大型矩阵建议逐 sheet 导出成压缩 TSV，并保留第一行原字段：

```python
import pandas as pd
xls = pd.ExcelFile("supplement.xlsx")
for sheet in xls.sheet_names:
    df = pd.read_excel(xls, sheet_name=sheet)
    df.to_csv(f"{sheet}.tsv.gz", sep="\t", index=False)
```

### 3.8 下载后建议整理

```text
63_Dufva_2020/
├── drug_screen/
│   ├── dose_response/
│   ├── DSS/
│   └── compound_annotation/
├── tcr_reporters/
├── crispr_nalm6/
│   ├── gene_level_mageck/
│   └── library_annotation/
├── validation/
│   ├── primary_B_ALL/
│   └── cell_line_panel/
├── supplementary_methods/
└── metadata/data_dictionary.tsv
```

`data_dictionary.tsv` 应记录每个表的实验对象（T cell、NALM6、co-culture）、readout、单位、剂量、时间和重复。本文最容易出现的错误就是把不同对象的 readout 混为同一种 CAR-T 状态。

## 4. 主要生物学发现

### 4.1 SMAC mimetics 是突出的 CAR-T 增敏药物

SMAC mimetics 在共培养中增强杀伤，其作用并非简单等同于直接提升 TCR reporter，而是显著涉及肿瘤细胞死亡程序的解除抑制。

### 4.2 死亡受体/FADD 轴决定靶细胞易感性

NALM6 CRISPR screen 将 FADD、TNFRSF10B 等定位为 CAR-T cytotoxicity 的关键靶细胞内源节点。联合 birinapant 后，RIPK1/TNFRSF1A 等通路的重要性进一步显现。

### 4.3 药物可同时作用于效应细胞和靶细胞

同一药物可能增强 TCR signaling、直接毒杀肿瘤、或改变死亡受体通路。三层 screening 的价值在于分解这些机制。

## 5. 状态—功能—驱动因子的连接

本文建立的不是一个单一 T-cell intrinsic 机制，而是一个双细胞系统：

```text
drug exposure
├── T-cell signaling state: NFAT/NF-κB/AP-1
└── target-cell death state: FADD/TNFRSF10B/RIPK1/TNFRSF1A
        ↓
CAR-T–tumor coculture killing
```

这对“navigation”概念很重要：有时最佳状态不是只改造 T 细胞，而是同时把靶细胞推向更容易被杀伤的状态。

## 6. 对细胞治疗状态导航的启示

- 组合治疗的优化需要把 effector state 与 target susceptibility 同时纳入。
- 药物筛选应使用 coculture differential score，避免把一般性细胞毒药物误写成免疫协同剂。
- 靶细胞 CRISPR screen 是解释 CAR-T resistance 的必要补充。
- 可将 reporter 与高通量 killing readout 组合，建立多目标优化：杀伤提高，同时避免强烈抑制 TCR signaling。

## 7. 可复用的分析思路

1. 对五点浓度曲线拟合 DSS，而非挑选单浓度。
2. 计算 coculture DSS 减去 tumor-alone DSS 的差值。
3. 用 reporter 数据标注 T-cell intrinsic agonist/antagonist。
4. 用 NALM6-only screen 扣除一般 essential genes，再解释 CAR-T-specific selection。
5. 对 birinapant 联合条件做 interaction，而不是只比较各自 fold change。

## 8. 推荐图版

- 526-drug integrated screening workflow。
- coculture differential DSS 图，标出 SMAC mimetics。
- NFAT/NF-κB/AP-1 reporter heatmap。
- NALM6 CRISPR volcano/rank plot，标出 FADD、TNFRSF10B。
- birinapant + CAR-T 机制模型。

综述重绘时建议明确画出两类被扰动的细胞，避免读者误以为所有 hits 都来自 CAR-T。

## 9. 创新价值

- 将 pharmacological screening 与 genetic resistance screening 在同一 CAR-T–tumor 体系中整合。
- 用 reporter layer 区分 T-cell signaling 与 target-cell sensitization。
- 发现 SMAC mimetics 与 CAR-T 的可操作组合逻辑。
- 提供多目标、双细胞状态优化的实验范式。

## 10. 局限性

1. CRISPR screen 在 NALM6 靶细胞中完成，不提供 CAR-T intrinsic genome-wide map。
2. 24 h drug screen 偏向急性作用，不能充分预测长期耗竭、记忆和毒性。
3. Jurkat reporter 与原代 CAR-T 信号并不完全等价。
4. 公共材料主要是 processed supplementary matrices，缺少明确 raw FASTQ accession。
5. 低 E:T 和特定 CD19/NALM6 模型的依赖性限制向实体瘤外推。
6. SMAC mimetics 联合治疗的全身毒性和临床给药窗口需独立评估。

## 11. 对本章节的作用

| 综述模块 | 本文贡献 |
|---|---|
| Quantitatively characterize phenotype/function | dose-response DSS、reporter activity、co-culture killing 三种定量 readout |
| Techniques to perturb/manipulate states | 526 compounds + target-cell genome-wide CRISPR KO |
| Link transitions with drivers | death receptor/FADD/RIPK1 pathway 连接到 CAR-T killing susceptibility |
| Optimize conditions | 药物浓度、E:T 和组合治疗的多维优化 |
| Real-time optimization | 提供 plate-based functional loop 的基础，但不是在线闭环控制 |

## 12. 可直接用于综述的观点

> The efficacy of engineered T cells is jointly determined by the state of the effector cell and the death susceptibility of the target cell.

> Integrating coculture pharmacology with pathway reporters and target-cell CRISPR screening can distinguish immune potentiation from direct tumor toxicity.

> Drug-response curves and genetic interaction conditions provide a practical route toward multi-objective optimization of cell-therapy combinations.

## 13. 避免误读

- **CRISPR screen 在 NALM6，不在 CAR-T。**
- **526 是药物数量，每种还有 5 个浓度**，不是 526 个单点测量。
- **SMAC mimetic 的增效机制不能只归因于 TCR activation**，靶细胞 death pathway 是重要组成。
- **GeCKO v2 的精确 guide 数应以作者使用的 library annotation 为准**，不要从标准库版本未经核对地硬套。
- **supplementary gene-level 表不是 raw FASTQ**；当前未核验到独立 GEO/SRA accession。
- **短期 killing 改善不自动等于长期 CAR-T persistence 改善。**

## 数据与资源链接

- 论文全文：[PMC7098811](https://pmc.ncbi.nlm.nih.gov/articles/PMC7098811/)
- Blood 页面：[DOI landing page](https://doi.org/10.1182/blood.2019002121)
