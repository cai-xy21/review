# 《A high-density microfluidic bioreactor for the automated manufacturing of CAR T cells》精读

## 论文信息

- 作者：Sin WX, Jagannathan NS, Teo DBL, Kairi F, Fong SY, Tan JHL, Sandikin D, Cheung KW, Luah YH, Wu X, Raymond JJ, Lim FLWI, Lee YH, Seng MS, Soh SY, Chen Q, Ram RJ, Tucker-Kellogg L, Birnbaum ME
- 期刊：*Nature Biomedical Engineering* 8:1571–1591；在线发表于 2024 年 6 月 4 日
- DOI：[10.1038/s41551-024-01219-1](https://doi.org/10.1038/s41551-024-01219-1)
- PubMed：[PMID 38834752](https://pubmed.ncbi.nlm.nih.gov/38834752/)
- 原文、Supplementary Information 与 Source Data：[Nature Biomedical Engineering](https://www.nature.com/articles/s41551-024-01219-1)
- RNA-seq：[GEO GSE261103](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE261103)
- 模型代码：[GitHub perfReactorCarT](https://github.com/narendrasuhas/perfReactorCarT)

## 一句话结论

2 ml 闭合式灌流微流控生物反应器可从 2×10^6 起始 T 细胞完成激活、慢病毒转导和 12 天扩增，健康供者产出超过 2×10^8 个 CAR⁺ viable T cells、淋巴瘤患者供者超过 6×10^7，并通过在线溶氧/pH、培养液代谢物和动力学模型估计 donor-specific growth/metabolic rates，构成高密度制造与在线状态监测的工程原型。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 生物反应器 | 2 ml automated closed-system microbioreactor（Breez） | 对照为 8 ml gas-permeable well；介质用量不等同于工作体积 |
| 工艺 | D0 激活、D1 转导、D12 收获 | continuous perfusion；约 D2 起 1 vvd，逐步至 D8 后 4 vvd |
| 健康供者 | 3 位 | 每供者 3 个 CAR 技术重复 + 1 个 non-transduced，共 12 runs/系统设计层面 |
| 患者供者 | 3 位成人淋巴瘤患者 | 每供者每系统单 run；来自废弃 leukapheresis tubing material |
| 起始细胞 | 2×10^6 purified T cells/run | 健康与患者工艺均采用有限起始量 |
| 终点产量 | 健康 >2×10^8 CAR⁺ viable；患者 >6×10^7，文中患者微反应器可 >7.5×10^7 | 剂量比较是工程参照，不等于可直接临床放行 |
| RNA-seq | GSE261103，6 个 bulk 样本 | MBR vs GWP，各 3 位健康供者；非单细胞 |
| 处理文件 | gene count 2.3 MB；FPKM 3.4 MB | raw reads 在 SRA；处理表适合快速比较 |
| 其他数据 | flow、cytokine、metabolites、sensor、mouse、Source Data | 部分需论文 Source Data；完整 raw/analysed data可向作者申请 |

## 1. 研究要解决的问题

自体 CAR-T 制造需要小批量、多患者并行，但传统 fed-batch 缺乏实时环境控制，大型反应器又不适合 scale-out。研究目标是：

1. 在 2 ml 封闭系统中完成全部上游步骤；
2. 通过灌流和气体控制提高细胞密度与每批产量；
3. 证明产品表型和功能不劣于 gas-permeable well；
4. 把在线传感和代谢建模变成非破坏性的过程分析工具。

## 2. 工程系统框架

### 2.1 微反应器

系统具备细胞接种、试剂加入、取样、废液和 sterile collection 端口，连续 medium perfusion，并通过 headspace O2 调节溶氧。在线 pH 和 dissolved oxygen sensor 提供过程曲线。

### 2.2 对照工艺

gas-permeable well 使用约 8 ml 培养体积并每两天交换 6 ml 培养基；微反应器总 medium usage 约 59 ml，对照约 33 ml。微反应器的优势主要是高密度、自动化和闭环可测，而不是培养基总量更低。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

这是一套**设备—过程—产品—功能**多层制造数据：

1. 在线 pH/DO 与灌流设置；
2. 细胞计数、viability、CAR transduction、CD4/CD8 和分化/耗竭流式；
3. spent-media glucose、lactate、glutamine、ammonia 等代谢物；
4. bulk RNA-seq 比较 MBR 与 gas-permeable well 终产品；
5. cytokine、短期杀伤、连续 rechallenge/persistence；
6. 患者来源产品的 NSG anti-leukaemia efficacy；
7. 动力学模型估计 donor-specific growth、glucose consumption、lactate production 等速率。

只有 bulk RNA-seq 被专门存入 GEO。其他 figure-level 数据在 Nature Source Data，部分完整 raw/analysed 数据需合理请求。

### 3.2 多大规模、覆盖哪些生物情境

| 层级 | 规模/设计 | 主要用途 |
|---|---|---|
| 健康供者工艺 | 3 donors；每 donor 3 CAR + 1 NT runs；MBR 与 GWP 对照 | 重复性、扩增、表型、功能、RNA |
| 患者工艺 | 3 lymphoma donors；每 donor 每系统单 run | 有限原料下的可行性与临床相关产量 |
| 患者原料 | 40–60 ml 稀释 tubing blood，约 50–125 million PBMC；7–11 million T cells/patient | 不是标准完整 leukapheresis product |
| 终点 | D12 | healthy final VCC >300 million、CAR 60–70%；patient CAR约 50% |
| 细胞密度 | 接近 2×10^8 cells/ml | 比 gas-permeable well 高约一个数量级 |
| 体内 | 一位患者供者产品进入 NALM6 NSG 功能验证 | 不能视为 3 位患者均有独立 in-vivo 验证 |

健康供者 MBR 中 cumulative expansion >400-fold，对照 <200-fold；D12 MBR viable cell count 约为 GWP 的 2.5 倍。超过半数终产品仍为 Tn/Tscm + Tcm，但 MBR 有轻微更高的分化/衰老 marker，且存在明显 donor-specific feature。

### 3.3 GSE261103 具体有什么

| 项目 | 内容 |
|---|---|
| 平台 | Illumina NovaSeq 6000，Homo sapiens |
| 样本 | 6：MBR Donor A/B/C + GWP Donor A/B/C |
| 处理文件 | `GSE261103_gene_count.xls.gz`（约 2.3 MB） |
| 处理文件 | `GSE261103_gene_fpkm.xls.gz`（约 3.4 MB） |
| 原始数据 | SRA raw reads，从 GEO 的 SRA Run Selector 进入 |
| 设计 | 同一 3 位健康供者配对比较 MBR vs GWP end products |

RNA 数据显示同一供者跨系统的整体转录相关性 R²>0.96，但仍有 1,204 个差异基因：752 在 MBR 上调、452 下调，富集激活/增殖和 cytokine/chemokine 相关过程。配对 donor 设计非常重要，应在统计模型中使用 donor block。

### 3.4 其他公开数据与代码

- Nature 页面提供主图与 Extended Data 的 Source Data；
- Supplementary Information 包括设备、工艺、流式、功能和模型细节；
- GitHub `perfReactorCarT` 提供 computational modelling code；
- Data availability 说明 raw/analysed datasets 可出于研究目的向通讯作者合理请求。

因此公开层级可概括为：**RNA raw/processed 完全公开；图级过程和功能数值公开；完整过程原始文件部分需申请。**

### 3.5 如何获取：按目的选择

#### 路线 A：重做终产品转录比较

直接下载 gene count 表，建立 `donor + system` metadata，用 paired design：

```r
design <- model.matrix(~ donor + system, data = meta)
```

只有 3 个 donor，差异表达应重点报告 effect size、方向一致性和 pathway，不宜依赖大量小 P 值。

#### 路线 B：重做过程动力学

下载 Source Data 和 GitHub code，确认 sensor、cell count、glucose/lactate 的时间戳与单位。若 Source Data 不含完整采样频率，向作者申请原始传感器导出和 spent-media assay 表。

#### 路线 C：复用患者制造数据

从 Fig. 5 Source Data 获取每位患者的 D12 expansion、viability、CAR%、CAR⁺ cell yield 与功能。患者数据为单 run，不可估计同一 donor 的过程重复性。

#### 路线 D：设备复现

Supplementary Methods 和代码只覆盖部分系统。需要 CAD、材料、传感器校准、无菌验证、控制软件和泵参数时，需联系作者/设备方；数据包本身不能完成 GMP 设备复制。

### 3.6 下载后先做什么

```python
import pandas as pd

counts = pd.read_excel("GSE261103_gene_count.xls.gz", index_col=0)
print(counts.shape)
print(counts.columns)
```

若 `read_excel` 不能直接读取 gzip 包装，先安全解压到 `.xls`。分析前确认 counts 是否整数、gene identifier 类型，以及 donor A/B/C 的列顺序。

## 4. 制造与产品发现

- 2 ml MBR 在 D6–12 的扩增优势最明显，可能来自更好的 nutrient exchange 和 gas transfer；
- healthy donor 可在一个 cassette 产出 >2×10^8 CAR⁺ viable cells；
- patient donor 也可达到 tisa-cel 最低剂量量级；
- MBR 产品短期细胞因子、杀伤与长期 rechallenge 不劣于 GWP；
- MBR 终产品略更分化，但 triple-positive PD-1/LAG3/TIM3 总体很低；
- donor A/B/C 的长期 persistence、分化与 CD57 表型不同，在线模型需 donor-aware；
- 患者来源 MBR CAR-T 在 NALM6 小鼠中与 GWP 产品有相当抗白血病活性。

## 5. 在线传感如何连接细胞状态

pH/DO 不是 T 细胞状态的直接分子 marker，但与 glucose/lactate、细胞密度和增殖模型联合后，可估计 donor-specific metabolic rate。论文最重要的系统观点是：

> 在线环境信号 + 稀疏离线生物测量 + mechanistic model，可以形成无菌、非破坏性的 state estimator。

这为未来根据估计的 growth/metabolic state 自动调整 perfusion、O2 或 harvest time 提供基础。

## 6. 距离闭环优化还有多远

当前系统主要执行预设 perfusion ramp，并事后/并行估计状态。尚未展示模型根据实时估计自动改变 flow/O2 并改善 CQA。因此它是**sensor-enabled automated manufacturing**，还不是完整 model-predictive control。

## 7. 推荐图版

- **Fig. 1**：设备与工艺流程，适合制造章节开场。
- **Fig. 2**：细胞密度、扩增和 CAR yield，是 scale-out 主图。
- **Fig. 5–6**：患者来源与体内功能，支撑临床相关性。
- **Fig. 7**：传感器和代谢模型，最适合“real-time optimization systems”。

如果只能选一组，选 Fig. 1 + Fig. 7。

## 8. 创新价值

1. 在 2 ml 封闭系统内集成激活、转导和扩增。
2. 以高密度灌流实现单 cassette 临床剂量量级产出。
3. 同时验证健康和淋巴瘤患者来源原料。
4. 开放 bulk RNA、图源数据和模型代码。
5. 把在线 pH/DO 与细胞/代谢动力学连接为状态估计器。

## 9. 局限性

1. 只有 3 位健康和 3 位患者供者，外部制造场地未验证。
2. 患者工艺为单 run，不能量化 process repeatability。
3. 终产品 bulk RNA 仅 6 样本，统计功效有限。
4. MBR 使用更多总培养基，成本优势需完整 techno-economic analysis。
5. 在线信号仍是间接状态代理，未展示实时闭环控制改善 outcome。
6. NSG 功能验证规模有限，不能替代临床 potency 和长期安全性。
7. 完整 raw sensor/flow/metabolite 数据并未全部在公共仓库一键下载。

## 10. 对本章节的作用

该文适合作为“**从离线状态测量到制造过程状态估计**”的代表。它同时覆盖设备 miniaturization、patient-specific variability、在线传感、代谢模型与最终功能，是构建实时优化系统的关键工程案例。

## 11. 可直接用于综述的观点

> 2 ml 灌流微反应器可从 2×10^6 起始 T 细胞在 12 天内制造临床剂量量级的 CAR-T，并通过 pH/DO、代谢物和细胞动力学模型估计 donor-specific growth/metabolic rates，说明高密度 scale-out 与在线状态估计可以在同一制造平台中实现（Nature Biomedical Engineering 2024, Sin）。

## 12. 避免误读

- 不要把“临床剂量量级”写成已完成临床放行或输注。
- 不要把 12 runs 写成 12 位健康供者。
- 不要把 6 个 RNA 样本写成 6 位独立患者。
- 不要写成微反应器培养基用量更少；该工艺总 medium usage 更高。
- 不要把在线状态估计写成已验证的闭环控制。

## 13. 数据复用优先级

优先下载 GSE261103 counts、Nature Source Data 和 GitHub code。若目标是建立数字孪生/实时控制 benchmark，应进一步向作者申请原始 pH/DO time series、flow-rate log、cell-count/metabolite timestamps 和 batch metadata；只有这些数据对齐后才能严谨训练 state-space model。
