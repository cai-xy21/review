# 《CRISPR/Cas9 mediated deletion of the adenosine A2A receptor enhances CAR T cell efficacy》精读

## 论文信息

- **作者**：Lauren Giuffrida, Kevin Sek, Melissa A. Henderson 等
- **期刊与年份**：*Nature Communications*, 2021
- **DOI**：10.1038/s41467-021-23331-5
- **本地原文**：[PDF](<D:/research/review/perturbation33references/12-CRISPRCas9 mediated deletion of the adenosine A2A receptor enhances CAR T cell efficacy.pdf>)
- **核心数据入口**：[GEO SuperSeries GSE156192](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE156192)

## 一句话结论

在 CAR-T 细胞中永久敲除腺苷 A2A 受体（ADORA2A/A2AR），可降低腺苷丰富的肿瘤微环境造成的免疫抑制，恢复细胞因子、JAK–STAT 信号与抗肿瘤活性；其优势在于把短效药理阻断转化为细胞内在、持续的工程化抗抑制能力。

## 数据护照

| 项目 | 内容 |
|---|---|
| 数据类型 | 人和小鼠 CAR-T bulk RNA-seq；体外功能实验；小鼠肿瘤模型 |
| 新产生的公开组学数据 | GSE156192，共 **60 个 GEO 样本/测序文库记录** |
| 子系列 | GSE156189、GSE156190、GSE156191、GSE166807 |
| 物种 | Homo sapiens；Mus musculus |
| 原始数据 | NCBI SRA，与各 GEO 子系列关联 |
| 处理后数据 | 各子系列的 raw-count 矩阵 |
| 关键计量单位 | 供体/生物学重复，而非 GEO 样本记录数 |

## 1. 研究问题

肿瘤微环境中的胞外腺苷通过 A2AR 抑制 TCR/CAR 信号、细胞因子产生和细胞毒性。小分子拮抗剂需要持续暴露且可能存在系统性作用，因此本文追问：能否用 CRISPR/Cas9 直接删除 CAR-T 的 ADORA2A，使抑制抵抗成为稳定的细胞内在性状？

## 2. 实验设计与方法框架

研究同时使用小鼠抗 HER2 CAR-T 与人抗 Lewis Y CAR-T。作者以 CRISPR/Cas9 构建 A2AR 缺失细胞，并与 mock 编辑对照比较；用 NECA 模拟广谱腺苷受体激动环境，用 SCH58261 作 A2AR 药理拮抗对照。功能读出包括细胞因子、增殖、杀伤、信号通路和体内肿瘤控制，bulk RNA-seq 用于解释转录层面的抑制与救援机制。

## 3. 数据规模与图谱组成

### 3.1 这里的“图谱”是什么

本文不是单细胞细胞图谱，而是一组围绕“物种 × CAR 构型 × A2AR 状态 × 腺苷刺激”的干预型转录组。理解数据时应把每个子系列看作一张独立实验切片，不能仅按 60 个样本把人、小鼠、CD4/CD8 和不同刺激时长直接合并。

### 3.2 四个子系列的精确构成

| 子系列 | GEO记录数 | 实验组成 | 生物学重复与注意事项 |
|---|---:|---|---|
| **GSE156189** | 12 | 小鼠抗 HER2 CAR-T；NECA 与/或 A2AR 拮抗剂 SCH58261 的药理干预 | 四种处理组合，各 3 个生物学重复；用于比较腺苷抑制与药理救援 |
| **GSE156190** | 12 | 人抗 Lewis Y CAR-T；mock 或 ADORA2A-KO，分别加/不加 10 μM NECA，刺激 8 h | **3 位健康供体 × 4 条件**；供体是配对统计单位，不是 12 位供体 |
| **GSE156191** | 12 | 小鼠抗 HER2 CAR-T；mock 或 A2AR-KO，分别加/不加 1 μM NECA，刺激 8 h | 4 条件 × 3 生物学重复；3′ RNA-seq |
| **GSE166807** | 24 | 小鼠 CAR-T；CD4/CD8 × mock/A2AR-KO × ±NECA，刺激 5 h | **2 亚群 × 2 基因型 × 2 刺激 × 3 重复**；可分析谱系依赖的救援效应 |

合计为 **12 + 12 + 12 + 24 = 60 个文库记录**。其中只有 GSE156190 明确是 3 位人供体；其余记录包含小鼠生物学重复和条件展开。因此“60”不能写成“60 个患者/供体”。

### 3.3 公开数据包中有什么

- GSE156190：`GSE156190_human_A2AKO_rawcounts.txt.gz`，人 CAR-T 原始计数矩阵；原始 FASTQ 位于 SRA（SRP277394/PRJNA657021）。
- GSE156191：`GSE156191_murine_A2AKO_raw_counts_table.txt.gz`，小鼠 A2AR-KO 实验计数矩阵；原始数据位于 SRP277395/PRJNA657024。
- GSE166807：子系列补充文件中的 raw-count 表；原始数据位于 SRP306572/PRJNA702055。
- GSE156189：GEO 页面提供该药理实验的处理后文件及 SRA 关联入口。
- 论文的功能学数值、流式图和体内实验数据主要在 Source Data/补充材料，不应期待在 GEO 中找到。

### 3.4 推荐下载方式

1. 从 GSE156192 进入四个子系列，先下载 supplementary file 中的 count matrix 与 series matrix/样本注释。
2. 若只做差异表达和通路复核，处理后计数矩阵已足够，且最不容易混淆样本。
3. 若需重新比对，在每个子系列点击 **SRA Run Selector**，导出 `RunInfo.csv` 和 `Accession List`，再运行：

```bash
prefetch --option-file SraAccList.txt
fasterq-dump --split-files --threads 8 --outdir fastq --option-file SraAccList.txt
```

4. 人、小鼠及各子系列分别建目录；不要在缺少样本表的情况下把四个 count matrix 直接拼接。

### 3.5 下载后如何整理

建议建立至少这些元数据列：`species`、`subseries`、`donor_or_rep`、`car_target`、`cell_subset`、`edit`、`NECA`、`antagonist`、`stim_hours`、`library_protocol`。GSE156190 应使用供体配对设计；GSE166807 可使用 `subset × edit × NECA` 交互项。跨物种比较应先在各物种内求效应量，再做同源基因层面的方向比较。

## 4. 主要结果

ADORA2A 删除提高了 CAR-T 在腺苷受体激动条件下的细胞因子产生和效应功能，并改善肿瘤模型中的控制。转录组显示，A2AR 信号压低 T 细胞激活及 JAK–STAT 相关程序，而基因敲除可部分恢复这些程序。

## 5. 机制链条

肿瘤代谢/缺氧促使胞外腺苷累积 → A2AR–cAMP 轴抑制 CAR/TCR 下游信号 → 效应因子和细胞因子下降；ADORA2A-KO 切断受体入口 → 在 NECA 存在时维持激活与细胞毒程序 → 提升 CAR-T 抗肿瘤效力。

## 6. 推荐重点阅读的图

- CRISPR 编辑效率与 A2AR 功能缺失验证图。
- ±NECA 条件下 mock 与 A2AR-KO 的细胞因子/杀伤对比。
- RNA-seq 的差异表达、通路富集和 JAK–STAT 相关图。
- 体内肿瘤控制与生存曲线，用于判断体外救援是否转化为疗效。

## 7. 创新性

论文把可逆的受体药理阻断改造为 CAR-T 固有的抗抑制模块，并在人、小鼠与药理/遗传对照中形成相互支持的证据链。其可复用价值还在于完整的 2×2 扰动设计，可直接估计“编辑能否特异抵消 NECA 效应”。

## 8. 局限性

bulk RNA-seq 无法拆分细胞状态异质性；健康供体数量有限；不同子系列的物种、刺激时间、建库方法并不一致。永久删除 A2AR 也可能改变长期稳态、迁移或安全性，不能仅凭短时 RNA-seq 推断临床净获益。

## 9. 在综述中的定位

适合作为“代谢性免疫抑制受体编辑”的代表工作，与代谢底物工程、转录因子重编程和负调控因子敲除形成对照。

## 10. 可直接写入综述的表述

> ADORA2A 的 CRISPR 删除使 CAR-T 获得对腺苷抑制的细胞内在抵抗，并在转录、功能和体内层面恢复效应程序，提示肿瘤微环境感知受体可作为装甲化细胞治疗的工程位点。

## 11. 数据复用建议

优先在 GSE156190 和 GSE156191 中做相同的 `edit × NECA` 交互分析，提取跨物种一致的救援基因集；再用 GSE166807 判断该基因集在 CD4 与 CD8 亚群中的差异。可构建“腺苷抑制分数”和“基因编辑救援分数”，用于与其他 CAR-T 扰动数据比较。

## 12. 转化与安全性关注

A2AR 是系统性生理调节受体。工程细胞虽限制了作用范围，但仍需评估过度炎症、长期持续性、组织归巢和慢性抗原环境下的耗竭风险；治疗优势不等于受体在所有场景均可无条件删除。

## 13. 避免误读

- **60 是 GEO 样本/文库记录，不是 60 位供体。**
- GSE156192 包含人和小鼠、不同刺激时长及不同建库设计，不能直接作为一个同质队列分析。
- GEO 主要提供转录组；流式、杀伤和体内数据需要从 Source Data/补充材料获取。
- NECA 是实验性腺苷受体激动条件，不等同于完整肿瘤微环境。
