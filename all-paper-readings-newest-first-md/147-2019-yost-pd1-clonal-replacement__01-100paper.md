# 《Clonal replacement of tumor-specific T cells following PD-1 blockade》精读

## 论文信息

- **作者**：Yost KE, Satpathy AT, Wells DK, et al.
- **期刊与年份**：Nature Medicine, 2019
- **DOI**：[10.1038/s41591-019-0522-3](https://doi.org/10.1038/s41591-019-0522-3)
- **原文**：[Nature Medicine](https://www.nature.com/articles/s41591-019-0522-3)
- **PubMed**：[PMID 31359002](https://pubmed.ncbi.nlm.nih.gov/31359002/)
- **开放全文**：[PMC6689255](https://pmc.ncbi.nlm.nih.gov/articles/PMC6689255/)
- **GEO SuperSeries**：[GSE123814](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE123814)
- **单细胞 RNA/TCR**：[GSE123813](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE123813)
- **bulk RNA**：[GSE123812](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE123812)
- **WES/SRA**：[PRJNA533341](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA533341)
- **bulk TCR**：[ImmuneACCESS, DOI 10.21417/KY2019NM](https://doi.org/10.21417/KY2019NM)
- **本报告核对日期**：2026-08-23

## 一句话结论

本文以基底细胞癌和鳞状细胞癌的治疗前后配对活检为对象，对 79,046 个高质量单细胞进行 10x 5′ RNA+TCR 分析，发现 PD-1 阻断后扩增的耗竭 T 细胞克隆多数在治疗前活检中未被检测到，提出疗效相关 T 细胞更新可由“clonal replacement”而非仅由原位耗竭克隆复苏完成。

## 数据护照

| 项目 | 内容 |
|---|---|
| 癌种 | 基底细胞癌（BCC）和皮肤鳞状细胞癌（SCC） |
| 设计 | 部位匹配的 anti-PD-1 治疗前/后活检；SCC 为短间隔连续活检 |
| 技术 | 10x 5′ scRNA-seq + V(D)J；配套 bulk RNA、WES、外周血 bulk TCR |
| QC | 83,583 个捕获/测序细胞中 79,046 个通过 QC（约 95%） |
| BCC | 11 名患者；53,030 个恶性、免疫和基质细胞；其中 33,106 个 TIL |
| BCC TCR | 28,371/33,106 个 T 细胞获得配对/可用 TCR（约 86%） |
| SCC | 4 名患者；26,016 个 TIL；治疗后平均约 31 天 |
| 单细胞仓库 | GSE123813；BCC 与 SCC 各自提供 counts、metadata、TCR 表 |
| 其他仓库 | GSE123812 bulk RNA；PRJNA533341 WES；ImmuneACCESS bulk TCR |

## 1. 研究问题

PD-1 阻断究竟主要“重新激活”肿瘤内既有的耗竭 T 细胞克隆，还是招募/扩增治疗前未检测到的新克隆？治疗前后同一克隆的状态如何变化？单次活检对克隆缺失的判定有多可靠？

## 2. 方法框架

1. 获取部位匹配的治疗前和治疗后 BCC/SCC 活检。
2. 用 10x 5′ 同时测量转录组与 V(D)J，使每个 barcode 的状态和 clonotype 配对。
3. 建立 BCC 全肿瘤生态图谱和 BCC/SCC TIL 图谱。
4. 将治疗后扩增的耗竭克隆分为 pre-existing 与 novel（治疗前活检未检测到）。
5. 用外周血 bulk TCR、WES 和 bulk RNA 辅助追踪克隆来源及治疗变化。

## 3. 数据规模与图谱组成

### 3.1 总体与癌种分层

| 层级 | 规模 | 图谱组成 |
|---|---:|---|
| 初始细胞 | 83,583 | 两癌种、多个时间点的所有单细胞 |
| QC 后细胞 | 79,046 | 摘要所称 scRNA+TCR atlas 的总规模 |
| BCC 全生态 | 53,030 | 恶性、免疫、基质细胞；11 名患者 |
| BCC TIL | 33,106 | 形成 9 个 T cell cluster；整体 BCC atlas 为 19 个 cluster |
| BCC 有 TCR 的 T 细胞 | 28,371（86%） | 用于克隆追踪 |
| SCC TIL | 26,016 | 4 名患者的连续活检；集中验证 clonal replacement |

BCC 和 SCC 的分析层级不同：BCC 数据既包含肿瘤生态又包含 TIL，SCC 主要是富集 TIL。不能把 53,030 与 26,016 简单解释为同一取样策略的两组。

### 3.2 GSE123813 公开文件

GSE123813 含约 **86 个 GSM 文库记录**。系列级文件已按癌种和数据类型整理，是这 10 篇中可复用性较高的数据包之一：

| 文件 | 约大小 | 内容 |
|---|---:|---|
| `GSE123813_bcc_scRNA_counts.txt.gz` | 100.35 MB | BCC 单细胞计数矩阵 |
| `GSE123813_bcc_all_metadata.txt.gz` | 1.32 MB | BCC 全部细胞元数据 |
| `GSE123813_bcc_tcell_metadata.txt.gz` | 0.81 MB | BCC T 细胞精细注释 |
| `GSE123813_bcc_tcr.txt.gz` | 0.76 MB | BCC clonotype/TCR 映射 |
| `GSE123813_scc_scRNA_counts.txt.gz` | 45.31 MB | SCC TIL 计数矩阵 |
| `GSE123813_scc_metadata.txt.gz` | 0.62 MB | SCC 细胞、患者、时间点等元数据 |
| `GSE123813_scc_tcr.txt.gz` | 0.52 MB | SCC clonotype/TCR 映射 |

下载时最关键的是把 counts、metadata 和 TCR 三类文件按 cell barcode 取交集，不能只按行顺序拼接。

### 3.3 其他数据包

- **GSE123812**：38 个 bulk RNA 文库记录，用于治疗前后/组织层面的表达对照。
- **PRJNA533341**：配套 WES 原始数据在 SRA/BioProject，可用于突变与潜在抗原分析。
- **ImmuneACCESS 10.21417/KY2019NM**：5 名患者 10 份血液样本的 bulk TCR 数据，用于判断某些“肿瘤内新出现”克隆是否可在外周血发现。
- 论文还复用了 GSE118165、GSE113590、GSE58377、E-MTAB-5678 等外部参考数据；这些不是本队列新生成的 79,046 个单细胞。

### 3.4 按目的下载

| 目的 | 必需文件 | 注意事项 |
|---|---|---|
| BCC 生态重建 | BCC counts + all metadata | 包含非 T 细胞 |
| BCC T 细胞状态/克隆 | BCC counts + tcell metadata + tcr | 用 barcode 合并，保留时间点/病灶信息 |
| SCC clonal replacement | SCC counts + metadata + tcr | 患者内比较；低频克隆受采样深度影响 |
| 外周来源线索 | ImmuneACCESS bulk TCR | 血液检出不等于已证明迁移进入肿瘤 |
| 突变背景 | PRJNA533341 | WES 体积较大，先核对 Run 表 |

### 3.5 下载与合并示例

```powershell
Invoke-WebRequest `
  -Uri "https://ftp.ncbi.nlm.nih.gov/geo/series/GSE123nnn/GSE123813/suppl/GSE123813_bcc_tcr.txt.gz" `
  -OutFile "GSE123813_bcc_tcr.txt.gz"
```

```python
import pandas as pd
meta = pd.read_csv("GSE123813_bcc_tcell_metadata.txt.gz", sep="\t")
tcr = pd.read_csv("GSE123813_bcc_tcr.txt.gz", sep="\t")

# 先检查实际列名，再用 cell barcode 显式连接
print(meta.columns)
print(tcr.columns)
```

## 4. Clonal replacement 的定义

作者将治疗后出现/扩增、但在匹配治疗前活检中未检测到的 clonotype 称为 novel clonotype。耗竭状态中大量治疗后扩增来自这类 novel clones，而不只是原有耗竭克隆的数目增加，因而提出 clonal replacement。

## 5. “Novel”不等于体内从不存在

这是本文最重要的解释边界。治疗前一小块活检未检测到某克隆，可能意味着它当时存在于其他肿瘤区域、引流淋巴结、血液或低于测序检测限。外周血 bulk TCR 可为来源提供线索，但仍不能确定克隆迁移的精确组织路线。因此应写“pre-treatment biopsy-undetected”，而不是绝对的“newly generated”。

## 6. 状态与克隆的联合变化

同一个 clonotype 在治疗前后可呈现不同转录状态，而不同 clonotype 对 PD-1 阻断的扩增贡献差异很大。治疗效应因此包含两层：已有细胞的状态重编程，以及群体克隆组成替换。对细胞治疗而言，这提示仅对终末耗竭细胞做状态逆转可能不够，补充具有不同克隆来源和前体潜力的细胞同样重要。

## 7. 主要生物学发现

- PD-1 阻断后扩增的耗竭 T 细胞很大部分来自治疗前活检未检测到的克隆。
- BCC 和 SCC 两个队列均支持 clonal replacement 概念。
- 配对 RNA+TCR 可区分“状态变化”和“克隆组成变化”。
- 外周血可能含部分后续进入/扩增于肿瘤的克隆，但来源仍未被直接追踪。

## 8. 推荐精读图

- BCC 全生态及 9 个 T 细胞群图。
- 治疗前后 clonotype 扩增瀑布/共享图。
- novel 与 pre-existing 克隆在耗竭群中的比例图。
- SCC 连续活检验证图。

## 9. 方法学创新

1. 10x 5′ RNA+VDJ 使状态与克隆一一配对。
2. 部位匹配、治疗前后设计将静态 atlas 推进到干预响应。
3. 明确区分状态重编程与克隆替换两个治疗机制。

## 10. 局限性

- 治疗前活检只采样局部，novel 判定受空间欠采样影响。
- 采样间隔和肿瘤类型不同，不能精确比较速度。
- 并非所有细胞都获得 TCR，低频克隆存在漏检。
- RNA/TCR 仍是离散时间点测量，不是连续活细胞追踪。
- 外周血检出只能提示来源，不能证明迁移方向。

## 11. 在综述架构中的位置

非常适合“**link cell state/function transitions with molecular drivers**”和“**techniques for live cell tracking**”之间的过渡：TCR barcode 是天然谱系标签，可追踪群体克隆命运，但缺少连续时空分辨率；未来需与实时成像、可记录 lineage tracing 或高频纵向采样结合。

## 12. 可直接写入综述的表述

> Yost 等在 BCC/SCC 治疗前后配对活检的 79,046 个高质量单细胞中联合测量 RNA 和 TCR，发现 PD-1 阻断后扩增的耗竭 T 细胞多数来自治疗前活检未检出的 clonotype，说明免疫治疗可通过克隆替换而非仅复苏原位耗竭克隆重塑 T 细胞群体。

## 13. 避免误读

- “novel clone”指治疗前活检未检测到，不等于治疗前全身不存在。
- 79,046 是 BCC 与 SCC 合计 QC 后细胞；两者取样策略不同。
- 53,030 个 BCC 细胞包含恶性和基质，不全是 TIL。
- TCR 是天然谱系标签，但不是实时位置追踪器。
