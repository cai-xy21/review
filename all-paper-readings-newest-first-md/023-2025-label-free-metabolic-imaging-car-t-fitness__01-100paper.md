# 《Label-free metabolic imaging monitors the fitness of chimeric antigen receptor T cells》精读

## 论文信息

- 作者：Pham DL, Cappabianca D, Forsberg MH, Weaver C, Mueller KP, Tommasi A, Vidugiriene J, Lauer A, Sylvester K, Lika J, Bugel M, Fan J, Capitini CM, Saha K, Skala MC
- 期刊：*Nature Biomedical Engineering*；在线发表于 2025 年 9 月 16 日
- DOI：[10.1038/s41551-025-01504-7](https://doi.org/10.1038/s41551-025-01504-7)
- PubMed：[PMID 40958004](https://pubmed.ncbi.nlm.nih.gov/40958004/)
- 开放全文：[PMC12445593](https://pmc.ncbi.nlm.nih.gov/articles/PMC12445593/)
- 全部公开数据与代码：[Zenodo 15628321](https://zenodo.org/records/15628321)；DOI [10.5281/zenodo.15628321](https://doi.org/10.5281/zenodo.15628321)

## 一句话结论

无标记 optical metabolic imaging（OMI）通过单细胞 NAD(P)H/FAD 强度、寿命和形态参数，能够监测培养基驱动的激活/细胞周期、预测最佳 CRISPR CAR knock-in 时间窗，并在终产品中识别与更低糖酵解、较高 CCR7 和更强体内抗肿瘤效力相关的代谢亚群；Zenodo 开放图级数据和分类/UMAP 代码，但未提供完整 raw FLIM image archive。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 核心技术 | NAD(P)H/FAD fluorescence lifetime OMI | 无外源染料的代谢 readout，但仪器和拟合依赖强 |
| OMI 特征 | 14 个参数 | intensity、lifetime components、redox ratio、cell/cytoplasm size 等 |
| 激活比较 | 4,923 cells，3 donors | ImmunoCult XF vs TexMACS；两种 activator；24–72 h |
| 细胞周期时序 | Imm 2,601 cells；Tex 3,442 cells，3 donors | 0/12/24/36/48/60/72 h；与 Hoechst/cell-cycle 对照 |
| 编辑预测 | 36 个样本、3 donors | activation 12–72 h；预测扩增后 CAR positivity |
| 终产品 OMI | 648 cells、2 donors | Imm、Tex、Tex→Imm；518 train + 130 test |
| 体内关联 | 9,569 spleen T cells、11 mice、2 donors；效力组每条件 2–5 mice | 流式/OMI/肿瘤结局层级不同 |
| Zenodo | 16 files，37,903,801 bytes（约 36.15 MiB） | 图级 XLS/XLSX/CSV + 2 个 Python 脚本；非原始显微全库 |

## 1. 研究要解决的问题

CAR-T 代谢适应性会影响编辑、扩增、memory 和体内 persistence，但常用 ATP、Seahorse、代谢组或流式 assay 需要破坏性取样。作者检验 OMI 是否可以：

1. 在单细胞、无标记条件下实时/重复观察代谢；
2. 预测最佳 transduction/editing 时间；
3. 检测终产品内代谢异质性；
4. 把 ex-vivo 代谢表型与 in-vivo potency 连接起来。

## 2. OMI 方法框架

### 2.1 测量原理

NAD(P)H 和 FAD 是内源荧光代谢辅因子。作者采集 fluorescence intensity 与 lifetime，并用双指数衰减模型估计自由/结合组分比例和平均寿命。高 NAD(P)H α1、低 NAD(P)H mean lifetime 常与更强 glycolytic state 一致。

### 2.2 制造扰动

- **Imm**：ImmunoCult XF 培养基/激活体系，glucose/glutamine 较高；
- **Tex**：TexMACS 体系；
- **Tex→Imm**：先 Tex 激活，再转入 Imm 扩增；
- 另有 Imm→Tex、Tex→Tex-high 等营养成分验证；
- 激活 12–72 h 后进行 virus-free CRISPR/Cas9 anti-GD2 CAR knock-in；
- 终产品进入 CHLA-20 neuroblastoma、M21 melanoma 和 MG63 osteosarcoma 功能实验。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

该数据是**单细胞 OMI 参数 + 平行生化/流式 + 编辑/功能结局**的多尺度表：

1. 每个细胞的 14 个 OMI/形态特征；
2. 每个 donor/condition/time 的 Hoechst cell-cycle、Ki-67、ATP、NAD(H)/NADP(H)、酶活和 Seahorse；
3. 每个制造样本扩增后的 CAR positivity；
4. 终产品 CCR7/CD62L、耗竭 marker 和体内 tumor flux；
5. UMAP 和 classification 代码。

OMI 是单细胞数据，但多数 biological outcome 在 sample 或 mouse 层级。模型训练若按细胞随机拆分，会共享 donor/condition 背景，不能自动等价为跨供者预测。

### 3.2 多大规模、覆盖哪些生物情境

| 实验模块 | 规模 | 主要结论 |
|---|---:|---|
| Media/activator screen | 4,923 activated cells，3 donors；3 个时间点 | 培养基而非 activator 主导代谢转变速度 |
| 激活时序 | Imm: 390/386/354/420/314/346/391 cells；Tex: 686/535/469/480/412/410/450 cells | Imm 在 36–60 h 更早进入高代谢/细胞周期，Tex 在 60–72 h |
| 编辑时间窗 | 36 samples，3 donors；每条件约 4–5 replicates | Imm 36–60 h、Tex 48–72 h 编辑效率最高；可提升 3–4 倍 |
| 扩增产品异质性 | 143 Imm、345 Tex→Imm、160 Tex cells，2 donors | Tex→Imm 中 high-lifetime 亚群约 30–40%，Imm约10%，Tex<5% |
| CCR7 共成像 | 519 CCR7−、371 CCR7+ cells，4 donors | CCR7+ 细胞 NAD(P)H lifetime 更高、α1 更低 |
| potency classifier | 648 cells，2 donors；518 training、130 testing | 区分 Tex→Imm 高效力与 Imm/Tex 低效力，AUC>0.91 |
| 小鼠功能 | 每条件 2–5 mice、2 donors | Tex→Imm 在部分鼠中回归并更好控瘤 |
| 脾细胞流式 | 9,569 cells、11 mice、2 donors | Tex→Imm 保留更多 memory、较少 exhaustion |

以上细胞数来自各 figure panel，不能相加成为一个统一训练集；不同实验的 donor 重叠关系应从源数据 sheet 核对。

### 3.3 Zenodo 公共数据包有什么

Zenodo 记录包含 16 个文件，总计 37,903,801 bytes（约 36.15 MiB）：

| 文件 | 大小 | 内容/用途 |
|---|---:|---|
| `Fig_1.xlsx` | 1.56 MB | 激活介质/activator OMI 与分类数据 |
| `Fig_2_Fig_3.xlsx` | 1.22 MB | 细胞周期时序、编辑时间窗和相关性 |
| `Fig_4.xlsx` | 11.05 MB | 扩增条件、代谢表型及相关数据 |
| `Fig_5.xlsx` | 0.31 MB | 终产品代谢异质性/CCR7 等 |
| `Fig_6.xls` | 1.59 MB | 体内 potency、产品分类与相关数据 |
| `Supp_Fig_1.xlsx` | 0.90 MB | 额外激活/代谢验证 |
| `Supp_Fig_2.xlsx` | 18.63 MB | 最大文件；扩展时序/单细胞数据 |
| `Supp_Fig_3.xlsx` | 0.17 MB | 编辑/transduction 补充数据 |
| `Supp_Fig_4.csv` | 0.25 MB | 补充图 4 的表格数据 |
| `Supp_Fig_5.xlsx` | 0.03 MB | 扩增产品补充数据 |
| `Supp_Fig_6.xlsx` | 0.34 MB | media switch 与代谢酶/ATP 验证 |
| `Supp_Fig_7.xlsx` | 1.66 MB | 小鼠/体内补充数据 |
| `Supp_Fig_8.xlsx` | 0.17 MB | 额外癌种杀伤数据 |
| `Heatmaps_code_Fig4.R` | 4.2 KB | Fig. 4 heatmap 代码 |
| `classification.py` | 7.7 KB | 分类模型代码 |
| `UMAP.py` | 3.8 KB | UMAP 代码 |

这是一套**figure-organized source-data release**。没有 TIFF/OME-TIFF/FLIM raw photon data、segmentation masks、trained model checkpoint 或统一 image manifest；因此可重做统计和分类，但不能从 raw photon decay 完整复现 OMI feature extraction。

### 3.4 如何获取：按目的选择

#### 路线 A：快速复现论文图

从 Zenodo 下载对应 figure XLSX/XLS。每个 workbook 先列出 sheet names、维度和表头，再按图注选择。不要一次性把所有 sheet 合并，因为很多是不同实验、不同 n 层级。

#### 路线 B：重做 UMAP/classification

下载 `UMAP.py`、`classification.py` 及 Fig. 1/6 数据。固定随机种子，并尝试 donor-held-out split；原论文 70/30 cell split 的高 AUC 不等于新 donor 泛化。

#### 路线 C：研究编辑最佳时机

使用 `Fig_2_Fig_3.xlsx`，把 OMI feature 与扩增后 `%CAR+` 按 sample ID 连接。分析单位应是 36 个 manufacturing samples，不是其中全部 individual cells。

#### 路线 D：从 raw image 重做 feature extraction

Zenodo 不足以完成。需向作者申请 raw FLIM data、instrument metadata、SPCImage fitting configuration、segmentation masks 和 image-to-sample mapping。

### 3.5 下载后先做什么

```python
import pandas as pd

book = pd.ExcelFile("Fig_2_Fig_3.xlsx")
print(book.sheet_names)
for sheet in book.sheet_names:
    df = pd.read_excel(book, sheet_name=sheet)
    print(sheet, df.shape, df.columns.tolist()[:8])
```

先识别 cell-level 与 sample-level sheet，再决定统计单位。若一个 sample 内有数百 cells，应以 donor/sample 聚合或 mixed model 处理，避免 pseudoreplication。

## 4. 激活与细胞周期发现

Imm 条件在约 48 h 出现 glycolytic shift 峰，Tex 在约 72 h；activator 类型影响较小。NAD(P)H lifetime、free fraction 和 cytoplasm size 与 Hoechst S/G2/M、Ki-67 相关，说明 OMI 可无标记读取激活—增殖进程。

## 5. 预测最佳 CAR 编辑窗口

低 NAD(P)H lifetime、较高 free fraction 和更大 cytoplasm 对应 cycling state，也对应更高 CRISPR knock-in efficiency。Imm 条件最佳约 36–60 h，Tex 约 48–72 h；按 OMI 选择时间可带来约 3–4 倍编辑效率差异。

这比固定“激活后 48 h 电转”更精确：最佳时点是培养体系依赖的状态，而不是绝对时钟。

## 6. 终产品异质性与体内效力

Tex→Imm 产生一个 high NAD(P)H-lifetime、低糖酵解/较氧化的亚群，并与 CCR7⁺ memory-like 状态一致。该产品在 CHLA-20 小鼠中更好控瘤，脾中 T 细胞更保留 CCR7/CD62L、较少 PD-1/TIGIT。

基于 pre-infusion OMI 的分类器区分 Tex→Imm 与 Imm/Tex 产品 AUC>0.91，但“高效力”标签实质上由培养条件代理，样本只有 2 donors；不能直接解释为预测任意患者体内疗效。

## 7. 推荐图版

- **Fig. 1–2**：OMI 监测激活与细胞周期。
- **Fig. 3**：OMI 预测 CRISPR 编辑窗口，最适合“优化条件”。
- **Fig. 5**：终产品代谢亚群与 CCR7。
- **Fig. 6**：ex-vivo OMI 连接体内 potency，最适合“实时优化系统”。

如果只能选一组，选 Fig. 3 + Fig. 6。

## 8. 创新价值

1. 以无标记单细胞代谢成像连接制造早期和体内效力。
2. 将编辑时机从固定时间转成状态依赖决策。
3. 揭示终产品内无法由均值看到的代谢亚群。
4. 开放完整图级 source data 和分析脚本。
5. 把培养基 switch 作为可操作的状态导航输入。

## 9. 局限性

1. 多数实验仅 2–4 donors，potency classifier 只有 2 donors。
2. cell-level 随机拆分可能高估跨供者泛化。
3. 高效力标签与培养条件绑定，未用大规模独立疗效 cohort 验证。
4. Zenodo 没有 raw FLIM images/decay curves 和 segmentation pipeline。
5. OMI 仪器复杂、吞吐量和 GMP at-line 集成仍需评估。
6. NAD(P)H/FAD readout 不能唯一分解具体代谢通路。
7. NSG 小鼠和少量肿瘤模型不能替代临床效力。

## 10. 对本章节的作用

该文是整篇综述最直接的“**实时观察状态—预测操作窗口—改变培养条件—连接体内功能**”案例之一。它可把 live-cell tracking、quantitative phenotype、navigation condition 和 real-time optimization 四部分串起来。

## 11. 可直接用于综述的观点

> 单细胞 OMI 显示 CAR-T 编辑的最佳时机不是固定培养时钟，而是由培养体系决定的代谢/细胞周期状态；ex-vivo high-lifetime、低糖酵解亚群还与 CCR7⁺ memory-like 表型和更强体内控瘤相关，提示无标记代谢成像可成为状态导航的反馈传感器（Nature Biomedical Engineering 2025, Pham）。

## 12. 避免误读

- 不要把 OMI 当作特异性代谢通路测定。
- 不要把 AUC>0.91 写成患者疗效预测已验证。
- 不要把数千 cells 当作数千独立生物样本。
- 不要说 Zenodo 包含完整 raw image archive。
- 不要把 Tex→Imm 的优势外推到所有 CAR、靶点和培养平台。

## 13. 数据复用优先级

优先下载全部 Zenodo 文件；从 Fig. 2/3 做 sample-level 编辑窗口模型，从 Fig. 6 做 donor-held-out 敏感性分析。若计划开发新 OMI pipeline，再向作者申请 raw FLIM 与 image manifest；公开包最适合图级复现和二次统计，不适合端到端成像复现。
