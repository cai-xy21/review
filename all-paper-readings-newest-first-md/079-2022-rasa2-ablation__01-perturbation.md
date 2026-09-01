# 《RASA2 ablation in T cells boosts antigen sensitivity and long-term function》精读

## 论文信息

- **作者**：Julia Carnevale, Eric Shifrut, Nupura Kale 等
- **期刊与年份**：*Nature*, 2022
- **DOI**：10.1038/s41586-022-05126-w
- **本地原文**：[PDF](<D:/research/review/perturbation33references/22-RASA2 ablation in T cells boosts antigen sensitivity and long-term function.pdf>)
- **RNA-seq 数据**：[GEO GSE204862](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE204862)
- **代码**：[Zenodo 6808407](https://doi.org/10.5281/zenodo.6808407)

## 一句话结论

在多种免疫抑制条件下的原代人 T 细胞 CRISPR 筛选共同指向 RASA2；删除这一 RAS-GAP 可提高低抗原条件下的敏感性，并在反复刺激中维持长期功能。

## 数据护照

| 模块 | 规模/组成 | 获取位置 |
|---|---|---|
| 全基因组 CRISPR KO 筛选 | 6 种条件；供体数随条件为 1–4 | Supplementary Tables 1–2 |
| 重复刺激 RNA 轨迹 | 3 供体 × TCR/CAR × input+Stim1–5 | GSE204862，36 样本 |
| RASA2KO Stim1 | 4 供体 × control/KO | GSE204862，8 样本 |
| RASA2KO Stim5 | 3 供体 × control/KO | GSE204862，6 样本 |
| **GSE204862 合计** |  | **50 个 bulk RNA-seq样本** |
| 代码 | 论文分析代码 | Zenodo 6808407 |

## 1. 研究问题

实体瘤中的低抗原密度与多种抑制因子会共同削弱 T 细胞。作者不是只在一种压力下筛选，而是问：是否存在能跨多种抑制环境、同时提高抗原灵敏度和长期功能的通用细胞内在制动器？

## 2. 实验设计与方法框架

作者在人原代 T 细胞中开展六种全基因组 CRISPR KO 筛选：基础刺激、腺苷、环孢素、他克莫司、TGF-β 和 Treg 共培养。跨筛选整合后聚焦 RASA2，再以阵列式编辑、低抗原刺激、重复刺激、bulk RNA-seq和体内模型验证。

## 3. 数据规模与图谱组成

### 3.1 六套 CRISPR 筛选

六种环境及供体重复为：

| 筛选环境 | 健康供体数 |
|---|---:|
| 基础/刺激条件 | **4** |
| 腺苷抑制 | **2** |
| 环孢素抑制 | **2** |
| 他克莫司抑制 | **2** |
| TGF-β 抑制 | **1** |
| Treg 共培养 | **4** |

因此各筛选的证据权重不相同，尤其 TGF-β 只有 1 位供体。筛选级结果主要发布在 **Supplementary Tables 1–2**，而不是 GSE204862；阵列式候选验证见 Supplementary Table 3，差异表达结果见 Table 4。不能在 GEO 中寻找六套筛选的 FASTQ 并据此认定“数据缺失”。

### 3.2 GSE204862：50 个 bulk RNA-seq样本

GEO 数据由三个清晰模块构成。

#### A. 重复刺激轨迹：36 个样本

- 供体：donor 1、2、4，共 **3 位**；
- 受体体系：TCR 与 CAR 两种；
- 时间/轮次：input、Stim1、Stim2、Stim3、Stim4、Stim5，共 **6 个状态**；
- 总数：**3 × 2 × 6 = 36**。

这是同一供体跨刺激轮次的纵向条件展开，不是 36 位供体。TCR 与 CAR 轨迹可用于定义共同和受体体系特异的慢性刺激程序。

#### B. RASA2KO vs control，Stim1：8 个样本

**4 位供体 × 2 个编辑状态 = 8**。这一模块反映早期/首次刺激下 RASA2 删除的效应，统计应做供体配对。

#### C. RASA2KO vs control，Stim5：6 个样本

**3 位供体 × 2 个编辑状态 = 6**。它反映反复刺激后的长期效应。Stim1 与 Stim5 的供体数不同，不能默认所有 4 位供体都具有完整终点配对。

合计为 **36 + 8 + 6 = 50 个文库**。

### 3.3 公开文件内容

GEO 提供三个与模块一一对应的 raw-count 矩阵：

- `GSE204862_RepStim_CARnTCR_countmatrix.tsv.gz`，约 970.4 KB；
- `GSE204862_RASA2KO_Stim1_countmatrix_fix.tsv.gz`，约 238.6 KB；
- `GSE204862_RASA2KO_Stim5_countmatrix_fix.tsv.gz`，约 137.7 KB。

原始 reads 在 SRA / PRJNA842576，平台为 NovaSeq 6000。代码在 Zenodo 6808407。外部数据 GSE119450（CROP-seq）以及 GSE89307、GSE86881、GSE138459 等用于背景验证，不属于本文新 50 样本。

### 3.4 推荐下载方式

1. 从 GSE204862 直接下载三个 count matrix 和 sample annotation；矩阵很小，足以重做大多数表达/通路分析。
2. 若需重新比对，进入 SRA Run Selector 导出 50 条 accession 与 `RunInfo.csv`，再用：

```bash
prefetch --option-file SraAccList.txt
fasterq-dump --split-files --threads 8 --outdir fastq --option-file SraAccList.txt
```

3. 从论文补充材料下载 Tables 1–4；六套筛选的 guide/gene-level 结果以这些表为主。
4. 从 Zenodo 6808407 下载代码并保留版本信息，用代码中的样本映射与 GEO 列名交叉核验。
5. 三个 RNA 矩阵分别分析；确认 donor 对齐后才可构建 Stim1/Stim5 的交互模型。

### 3.5 下载后的统计设计

重复刺激轨迹可用 `~ donor + receptor + stimulation + receptor:stimulation`，或在 TCR/CAR 内分别做纵向变化。RASA2 效应在 Stim1 和 Stim5 分别用供体配对比较，再测试 KO 效应是否随轮次增大。供体是统计重复，基因和时间点不是重复。

## 4. 主要结果

RASA2 在多个抑制性筛选中表现为共同负调控因子。其删除增强 RAS–MAPK 信号和低抗原敏感性，并使 T 细胞在反复刺激后保持更强的增殖、细胞因子和杀伤能力，改善体内肿瘤控制。

## 5. 机制理解

RASA2 是 RAS GTPase-activating protein，通常促进 RAS-GTP 水解并提高激活阈值。删除 RASA2 延长/增强 RAS–MAPK 信号，使低密度抗原也能触发反应；长期功能提升提示该调节并非只产生一次性过度激活。

## 6. 推荐重点阅读的图

- 六套筛选的跨条件命中整合和 RASA2 排名。
- 低抗原密度下 RASA2KO 的信号和功能曲线。
- Stim1–Stim5 RNA 轨迹及 RASA2KO/对照比较。
- 体内肿瘤控制、持久性和安全观察图。

## 7. 创新性

多压力环境并行筛选提高了命中的泛化价值；RASA2 将“抗抑制”和“提高抗原灵敏度”连接到同一可编辑信号阈值节点。

## 8. 局限性

不同筛选供体数不均，TGF-β 仅 1 位供体；RNA-seq 是 bulk；长期刺激终点只有 3 位供体。提高抗原灵敏度也可能增加正常组织低抗原识别和过度激活风险。

## 9. 在综述中的定位

适合作为“跨微环境筛选发现信号阈值制动器”的代表，也可与 ADORA2A、SNX9 和 NR4A 编辑比较其作用层级。

## 10. 可直接写入综述的表述

> 跨六种抑制环境的原代人 T 细胞 CRISPR 筛选识别出 RASA2；其删除通过解除 RAS 信号制动，提高低抗原敏感性并维持反复刺激后的长期功能。

## 11. 数据复用建议

可用 Supplementary Tables 1–2 计算跨筛选稳健排名，再用 36 样本轨迹定义慢性刺激签名，并在 Stim1/Stim5 的配对样本中检验 RASA2KO 对该签名的轮次依赖救援。不要把筛选表与 RNA count matrix当作同一 assay。

## 12. 转化与安全性关注

最重要风险是抗原阈值降低后对正常低表达组织的识别。需在多抗原密度、正常组织模型和长期刺激下绘制治疗窗，并考虑可调编辑、逻辑门或安全开关。

## 13. 避免误读

- **GSE204862 的 50 个样本是 RNA-seq，不包含六套 CRISPR 筛选原始测序。**
- 36 个轨迹样本来自 3 位供体，不是 36 位供体。
- Stim1 为 4 位供体，Stim5 为 3 位，不能假定完整 4 人纵向配对。
- TGF-β 筛选只有 1 位供体，单独证据强度有限。
