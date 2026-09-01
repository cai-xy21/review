# 《Guide Swap enables genome-scale pooled CRISPR–Cas9 screening in human primary cells》精读

## 论文信息

- **题目**：Guide Swap enables genome-scale pooled CRISPR–Cas9 screening in human primary cells
- **作者**：Ting et al.
- **期刊与年份**：Nature Methods，2018
- **DOI**：[10.1038/s41592-018-0149-1](https://doi.org/10.1038/s41592-018-0149-1)
- **PMID**：[30297964](https://pubmed.ncbi.nlm.nih.gov/30297964/)
- **主要数据入口**：Nature Supplementary Data 1–2 与 Source Data；论文未提供 GEO/SRA accession
- **研究类型**：Guide Swap pooled CRISPR–Cas9 平台；人原代 CD4 T 细胞 12,996-element screen + 人 cord-blood CD34+ HSPC 64,401-element screen

## 一句话结论

本文提出 Guide Swap：先用慢病毒把 pooled sgRNA library 稳定导入原代细胞，再电转预装 non-targeting guide 的 Cas9 RNP，使胞内 guide 与 Cas9 重新配对，从而在不构建 Cas9 稳定细胞株的情况下完成人原代 CD4 T 细胞和 CD34+ HSPC 的大规模 pooled knockout screening。

## 数据护照（先看这一表）

| 项目 | CD4 T-cell screen | CD34+ HSPC screen |
|---|---:|---:|
| library规模 | 12,996 elements | 64,401 guide rows，约 13,243 genes，分 5 pools |
| 靶基因 | 2,585 genes | 13,243 genes |
| 平均 guides/gene | 约 5 | 约 5 |
| controls | 73 controls；另对 CD4/CD45/CXCR4 各 spike-in 10 guides | AHR positive/control elements 等 |
| 起始细胞 | 16.25 million/replicate | 每 pool 约 3.9 million CD34+ cells |
| 起始覆盖 | 约 1,250 cells/guide | 约 300 cells/guide |
| 重复 | 2 technical replicates | 五个 library pools，screen replication/批次以表和 Methods 为准 |
| readout | day 0；day 6 triple-positive 与 CD4−/CD45−/CXCR4− populations | day 10–11 CD34+ 与 CD34− |
| 主要结果 | 68 个 essential genes；50 个已有已知依据 | CD34 differentiation/maintenance regulators |
| 公共文件 | Supplementary Data 1：约 1.934 MB，含 6 sheets | Supplementary Data 2：约 10.415 MB，含 3 sheets |
| raw FASTQ | 无 GEO/SRA accession；未公开成独立 raw repository | 同左 |

## 1. 研究要解决的问题

多数 pooled CRISPR screens 依赖稳定表达 Cas9 的细胞系。人原代 T 细胞和 HSPC 难以建立稳定 Cas9 细胞株；直接电转 Cas9 protein 虽安全、短暂，但如何让它与已由慢病毒导入的数万种 guide 在每个细胞内正确配对，是关键技术障碍。

本文提出“Guide Swap”：

1. 先让每个原代细胞通过低 MOI 慢病毒获得一条 library guide；
2. 再电转已经携带 non-targeting guide 的 Cas9 RNP；
3. 胞内 guide 交换/占据 Cas9，形成目标特异复合体；
4. 保留 pooled library 的可追踪性，同时避免长期 Cas9 表达。

## 2. 方法框架：先条码化，再短暂提供 Cas9

### 2.1 Guide Swap 机制

- lentiviral sgRNA 先进入细胞并稳定表达。
- Cas9 以 RNP 形式电转，预装一个 non-targeting guide 以提高蛋白稳定性/递送表现。
- 进入细胞后，library guide 与 Cas9 结合，完成目标切割。
- guide identity 仍由整合的 lentiviral cassette 保存，可在 selection 后扩增测序。

### 2.2 CD4 T-cell screen

- 原代人 CD4 T cells。
- library 12,996 elements，靶向 2,585 genes，约 5 guides/gene。
- 73 controls，另加入针对 CD4、CD45、CXCR4 各 10 条 guide 作为表型阳性 controls。
- 每个 replicate 16.25 million cells，约 1,250 cells/guide。
- 慢病毒转导约 65%。
- 两个 technical replicates。
- day 0 收集约 15 million cells。
- nucleofection 后 6 d 分选：
  - RFP-positive、CD4/CD45/CXCR4 triple-positive population；
  - CD4-negative；
  - CD45-negative；
  - CXCR4-negative。

### 2.3 HSPC screen

- 人 cord-blood CD34+ HSPCs。
- 13,243 genes，平均约 5 guides/gene。
- 为维持覆盖和可操作性，将 library 分为 5 个 pools，每 pool 约 13,000 guides。
- 每 pool 起始约 3.9 million cells，约 300 cells/guide。
- day 10–11 根据 CD34-positive 与 CD34-negative 分选，寻找控制 stem/progenitor maintenance 和 differentiation 的 genes。

Methods 中另有约 15 million cells 在某个 electroporation stage 的描述；其与“每 pool 3.9 million starting cells”的层级不同。引用时应保留步骤上下文，不应相加为一个总样本数。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

本文公共数据是两个 **processed CRISPR-screen workbooks**：

1. T-cell screen：每条 guide 在 day 0、triple-positive、CD4−、CD45−、CXCR4− 等群体中的 RPM/count 与 gene-level RSA statistics。
2. HSPC screen：五个 library pools 的 guide-level CD34+/CD34− readout 和 gene-level ranking。

这不是单细胞数据，也不是转录组。每一行是 guide 或 gene，不是细胞。表型分选的细胞总量用于保证 library coverage，但最后公开矩阵是聚合的 guide abundance。

### 3.2 CD4 T-cell library 与覆盖

| 项目 | 数值 |
|---|---:|
| library elements | 12,996 |
| target genes | 2,585 |
| average guides/gene | ~5 |
| controls | 73 + 30 spike-in phenotype guides |
| cells/replicate | 16.25 million |
| estimated coverage | ~1,250 cells/guide |
| transduction | ~65% |
| technical replicates | 2 |
| day-0 cells | ~15 million |
| selection time | 6 days after nucleofection |

作者在 essential-gene analysis 中报告 68 个 hits，其中 50 个可由既往 essentiality evidence 支持。CD4/CD45/CXCR4 spike-ins 用于证明对应 marker-negative populations 中 guide enrichment 符合预期。

### 3.3 Supplementary Data 1：T-cell screen workbook

Nature 文件：`41592_2018_149_MOESM3_ESM.xlsx`，约 1.934 MB。

| sheet | 规模 | 内容 |
|---|---:|---|
| Data S1a | 2,661 × 9 | essential screen gene-level RSA results |
| Data S1b | 2,661 × 8 | CXCR4 phenotype gene-level results |
| Data S1c | 2,661 × 8 | CD4 phenotype gene-level results |
| Data S1d | 2,661 × 8 | CD45 phenotype gene-level results |
| Data S1e | 2,583 × 8 | second-best-guide based gene ranking/summary |
| Data S1f | 13,006 × 11 | guide-level RPM matrix |

Data S1f 的约 13,006 rows 与 12,996 library elements 相差约 10 行，通常来自 header/legend/额外记录或 workbook 结构。二次分析必须读取 guide ID 后去除空行/legend，而不能直接把 Excel `max_row` 当作 guide 数。

Data S1f 的列覆盖两个 replicates中的 day 0 与多个 day 6 sorted populations。精确列名应原样保留，用于构建 replicate-aware contrasts。

### 3.4 HSPC library 与覆盖

| 项目 | 数值 |
|---|---:|
| target genes | 13,243 |
| average guides/gene | ~5 |
| library pools | 5 |
| guides/pool | ~13,000 |
| guide-level public rows | 64,401 |
| cells/pool | ~3.9 million |
| estimated coverage | ~300 cells/guide |
| selection | day 10–11 CD34+ vs CD34− |

64,401 guide rows略低于 13,243 × 5 的理论值，因为实际每基因 guide 数并非严格相等，且包含/过滤 controls。应以 Data S2a 的 guide ID 为最终 library 实体清单。

### 3.5 Supplementary Data 2：HSPC screen workbook

Nature 文件：`41592_2018_149_MOESM4_ESM.xlsx`，约 10.415 MB。

| sheet | 规模 | 内容 |
|---|---:|---|
| Data S2a | 64,401 × 14 | 五 pools 的 HSPC guide-level screening data |
| Data S2b | 13,030 × 8 | gene-level second-best guide/ranking results |
| Legend | 小表 | 字段和分析说明 |

Data S2a 是全部数据中最大的工作表。建议只读加载或分块导出，不要在电子表格中频繁筛选导致格式/公式误改。

### 3.6 Source Data

Nature 页面还提供：

- Source Data Fig. 1：约 18 KB；
- Source Data Fig. 3：约 15 KB；
- Supplementary Figures/Methods PDF。

Source Data 用于复现图中 summarized measurements；完整 guide matrices 仍以 Supplementary Data 1–2 为主。

### 3.7 是否有 GEO/SRA accession

论文 Data availability 指向文章和 supplementary information，未提供 GEO、SRA 或 ENA accession。因此：

- 可下载 per-guide RPM/processed counts 和 gene-level RSA results；
- 可以重新做 gene ranking、replicate consistency 和 phenotype enrichment；
- 不能从公共 repository 直接获得完整 raw FASTQ，重新验证 guide extraction/zero-read handling 受限。

在综述数据表中应写“processed guide-level matrices publicly available; raw sequencing accession not reported”，而不是写“无数据”。

### 3.8 下载方式

#### T-cell workbook

```text
https://media.springernature.com/original/springer-static/esm/art%3A10.1038%2Fs41592-018-0149-1/MediaObjects/41592_2018_149_MOESM3_ESM.xlsx
```

#### HSPC workbook

```text
https://media.springernature.com/original/springer-static/esm/art%3A10.1038%2Fs41592-018-0149-1/MediaObjects/41592_2018_149_MOESM4_ESM.xlsx
```

#### 读取与导出

```python
from openpyxl import load_workbook

path = "41592_2018_149_MOESM4_ESM.xlsx"
wb = load_workbook(path, read_only=True, data_only=False)
for ws in wb.worksheets:
    print(ws.title, ws.max_row, ws.max_column)
```

对于 64k × 14 的 sheet，推荐使用 pandas 分 sheet 读出并保存 Parquet：

```python
import pandas as pd

df = pd.read_excel(path, sheet_name="Data S2a")
df = df.dropna(how="all")
df.to_parquet("Data_S2a_HSPC_guides.parquet", index=False)
```

### 3.9 下载后建议整理

```text
70_Ting_2018_GuideSwap/
├── tcell_screen/
│   ├── guide_rpm.tsv.gz
│   ├── essential_RSA.tsv
│   ├── CD4_results.tsv
│   ├── CD45_results.tsv
│   └── CXCR4_results.tsv
├── hspc_screen/
│   ├── guide_matrix.parquet
│   ├── gene_ranking.tsv
│   └── pool_annotation.tsv
├── source_data/
├── library_annotations/
└── methods/
```

## 4. 主要技术与生物学发现

### 4.1 Guide Swap 将 pooled screening 扩展到原代人细胞

平台不需要 Cas9 stable line，减少长期 nuclease expression，并保留 pooled library 的大规模优势。

### 4.2 T-cell marker 与 essential-gene controls 验证了有效性

CD4、CD45、CXCR4 阳性 controls 在对应 negative gates 富集；68 个 essential hits 中 50 个有已知依据，说明 Guide Swap 可重现预期生物学。

### 4.3 HSPC 证明方法跨细胞类型

五 pool 设计使超过 64k guides 的筛选在数量稀缺的 CD34+ HSPC 中可行，说明方法并非只适用于 T cells。

## 5. 状态—功能—驱动因子的连接

```text
integrated sgRNA identity
→ transient Cas9 editing
→ surface-marker or fitness phenotype
→ guide enrichment/depletion
→ causal gene ranking
```

T-cell 部分的状态 readout 主要是 CD4/CD45/CXCR4 表达和 survival/essentiality，尚未覆盖 exhaustion、memory、cytotoxicity 等更复杂治疗状态。

## 6. 对细胞治疗状态导航的启示

- 把 Cas9 暴露限制在短时间窗口，适合原代和潜在制造相关细胞。
- library 可以先建立，再通过不同流式 gate 更换状态 readout。
- 未来可与多参数 flow、functional coculture 或单细胞 readout 结合。
- 稀缺原代细胞的关键约束是 cells/guide coverage，library 分池是实用解决方案。

## 7. 可复用的分析思路

1. 先做 day-0 library representation 和 replicate correlation。
2. 对 CD4/CD45/CXCR4 分别计算 negative vs triple-positive enrichment。
3. gene ranking 同时看 RSA 和 second-best-guide，降低单 guide 假阳性。
4. HSPC 五 pools 分别归一化，再在 gene 层汇总。
5. 报告 coverage bottleneck，并用 controls 估计 false-positive threshold。

## 8. 推荐图版

- Guide Swap molecular workflow。
- primary CD4 T-cell screen design 与 spike-in controls。
- 68 essential-gene ranking。
- CD4/CD45/CXCR4 phenotype enrichment。
- five-pool HSPC screen 与 CD34 fate results。

## 9. 创新价值

- 将 Cas9 RNP 的短暂性与 lentiviral pooled guide 的可追踪性结合。
- 在原代人 T cells 中实现近 13k-element screen。
- 在稀缺 CD34+ HSPC 中实现 64,401-guide 大规模筛选。
- 完整 guide-level processed matrices 可公开下载并重分析。

## 10. 局限性

1. T-cell screen 只有 2,585 genes，不是全人基因组。
2. HSPC library 虽覆盖 13,243 genes，但分为五 pools，pool/batch effects 需处理。
3. T-cell phenotypes 主要是 surface markers 与 essentiality，离细胞治疗复杂功能还有距离。
4. raw FASTQ 未以 GEO/SRA accession 公开。
5. 两个 T-cell replicates 是 technical replicates，不能替代多供者生物学重复。
6. guide swap efficiency、editing kinetics 和位点依赖性可能影响 gene ranking。

## 11. 对本章节的作用

| 综述模块 | 本文贡献 |
|---|---|
| Quantify phenotype/function | marker-negative sorting 与 essential fitness readout |
| Perturb/manipulate states | 原代细胞 pooled knockout 的 Guide Swap 递送方案 |
| Link transitions with drivers | guide enrichment 直接连接基因与表面状态/存活 |
| Optimize navigation | library coverage、分池、Cas9 暴露时序的工程优化 |
| Real-time systems | 可与 flow sorting 形成快速反馈，但目前为终点测序 |

## 12. 可直接用于综述的观点

> Guide Swap separates stable perturbation identity from transient nuclease exposure, enabling pooled genetic screening in primary human cells without a Cas9-expressing cell line.

> In scarce primary-cell systems, library size is constrained by cells-per-guide coverage; dividing the library into balanced pools is a practical design strategy.

> Public guide-level matrices can support biological reanalysis even when raw sequencing files are not deposited, but they cannot reproduce guide extraction and preprocessing from first principles.

## 13. 避免误读

- **CD4 T-cell library 是 12,996 elements、2,585 genes**，不是 genome-wide 全基因组覆盖。
- **HSPC 的 64,401 是 guide-level public rows/elements 量级，不是 genes。**
- **16.25 million 是每个 T-cell replicate 的细胞数**，不是测序 reads。
- **两次 T-cell screen 是 technical replicates**，不能写成两个独立 donors。
- **公开的是 processed guide matrices，不是 raw FASTQ。**
- **表中约 13,006 rows 不等于 library 真有 13,006 guides**，工作簿行数包含 header/legend 或额外记录。

## 数据与资源链接

- 论文：[Nature Methods](https://www.nature.com/articles/s41592-018-0149-1)
- PubMed：[30297964](https://pubmed.ncbi.nlm.nih.gov/30297964/)
- T-cell Supplementary Data：[MOESM3](https://media.springernature.com/original/springer-static/esm/art%3A10.1038%2Fs41592-018-0149-1/MediaObjects/41592_2018_149_MOESM3_ESM.xlsx)
- HSPC Supplementary Data：[MOESM4](https://media.springernature.com/original/springer-static/esm/art%3A10.1038%2Fs41592-018-0149-1/MediaObjects/41592_2018_149_MOESM4_ESM.xlsx)
