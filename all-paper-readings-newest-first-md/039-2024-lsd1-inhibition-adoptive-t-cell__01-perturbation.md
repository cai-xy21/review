# 《LSD1 inhibition improves efficacy of adoptive T cell therapy by enhancing CD8+ T cell responsiveness》精读

## 论文信息

- **作者**：Isabella Pallavicini, Teresa Maria Frasconi, Carlotta Catozzi 等
- **期刊与年份**：*Nature Communications*, 2024
- **DOI**：10.1038/s41467-024-51500-9
- **本地原文**：[PDF](<D:/research/review/perturbation33references/16-LSD1 inhibition improves efficacy of adoptive T cell therapy by enhancing CD8+ T cell responsiveness.pdf>)
- **新数据**：[GSE272770](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE272770)
- **复用数据**：[GSE147130](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE147130)

## 一句话结论

LSD1 抑制能提高 CD8 T 细胞对激活信号的响应并增强过继治疗效力；药理处理的体外转录组与既往 Lsd1 遗传缺失的肿瘤浸润 T 细胞数据共同支持这一方向。

## 数据护照

| 数据集 | 新/复用 | 规模 | 生物学场景 |
|---|---|---:|---|
| GSE272770 | 本文新产生 | 9 个小鼠 bulk RNA-seq样本：NT 5、LSD1i 4 | 体外激活 72 h，药理抑制 |
| GSE147130 | 既往公开、本文复用 | 8 个样本：WT 4、Lsd1-KO 4 | MC38 肿瘤内 CD8+CD44+PD1+ TIL，遗传缺失 |

## 1. 研究问题

表观遗传酶 LSD1/KDM1A 可能限制 T 细胞对抗原刺激的响应。本文检验短暂药理抑制 LSD1 是否可在不进行永久编辑的情况下提高 CD8 T 细胞激活和过继细胞治疗效果。

## 2. 实验设计与方法框架

作者在小鼠 CD8 T 细胞激活/扩增过程中使用 LSD1 抑制剂，检测早期响应、分化、细胞因子、杀伤和体内治疗效果。新产生的 72 h bulk RNA-seq刻画药理效应，并与公开的 Lsd1-KO MC38 TIL 数据交叉验证。

## 3. 数据规模与图谱组成

### 3.1 新产生数据 GSE272770

GSE272770 是 **9 个小鼠 CD8 T 细胞 bulk RNA-seq文库**：

- 未处理/载体对照（NT）：**n=5**；
- LSD1 抑制剂处理：**n=4**；
- 取样时间：激活后 **72 h**；
- 平台：Illumina NovaSeq 6000；
- SRA/BioProject：PRJNA1138703。

GEO 提供三类实用处理文件：差异表达结果表（约 726 KB）、batch-corrected counts（约 353 KB）和 raw counts（约 354 KB）。9 是生物学样本/文库数，不是 9 个时间点。

### 3.2 复用数据 GSE147130

该数据来自既往研究中的 MC38 肿瘤浸润 **CD8+CD44+PD1+ T 细胞**：

- WT：**n=4**；
- T 细胞 Lsd1 遗传缺失：**n=4**；
- 合计 **8 个 bulk RNA-seq样本**；
- 75 bp paired-end；SRA SRP253103 / PRJNA613073；
- GEO 提供约 761 KB 的 count matrix。

它代表“体内肿瘤 TIL + 遗传缺失”，而 GSE272770 代表“体外 72 h + 药理抑制”。两者可用于方向一致性验证，但不能直接合并成 17 个同质样本。

### 3.3 图谱组成的核心对照

本文数据可抽象为两条证据轴：

1. **短期、可逆药理轴**：NT vs LSD1i，体外 72 h，5 vs 4；
2. **长期、细胞内在遗传轴**：WT vs Lsd1-KO，肿瘤内 TIL，4 vs 4。

若同一通路在两条轴上方向一致，支持 LSD1 是稳健调控节点；若不一致，可能来自剂量、时长、细胞状态或微环境差异，不应简单解释为“数据冲突”。

### 3.4 推荐下载方式

1. 对 GSE272770，先下载 raw-count 表、batch-corrected 表和 GEO 样本注释。差异分析应优先从 raw counts 重做；校正矩阵适合可视化，不宜无条件用于计数模型。
2. 对 GSE147130，同样下载 count matrix 与 series/sample annotation，并标记为 external/reused。
3. 如需重新比对，分别通过两个系列的 SRA Run Selector 导出 accession list，再用 `prefetch` 和 `fasterq-dump --split-files` 下载。
4. 两个项目使用独立目录、各自参考基因组/注释版本；不要把 FASTQ 或 count matrix直接拼接后做普通两组比较。

### 3.5 下载后的分析方案

先在每个数据集内部做差异表达与 GSEA，再比较 log2FC 方向、rank-rank overlap 或通路 NES；元分析的单位应是“两个独立实验的效应量”，而非把 17 个样本视为同批实验。保留 `dataset`、`context`、`perturbation_type`、`replicate`、`time` 等字段。

## 4. 主要结果

LSD1 抑制提高 CD8 T 细胞对激活刺激的响应，并改善过继治疗的抗肿瘤活性。新旧转录组在若干激活/效应程序上形成互证，支持该表型不是单一实验批次的偶然结果。

## 5. 机制理解

LSD1 是组蛋白去甲基化及转录复合体调控因子。抑制 LSD1 改变 T 细胞激活相关染色质/转录阈值，使 CD8 T 细胞更易启动效应程序。本文主要证明功能和转录后果，完整的位点级因果链仍需更多染色质结合数据。

## 6. 推荐重点阅读的图

- LSD1i 对早期激活、细胞因子和杀伤的剂量/时间结果。
- GSE272770 的 PCA、差异表达和通路富集图。
- 与 GSE147130 Lsd1-KO TIL 的签名一致性图。
- 过继治疗肿瘤控制与生存曲线。

## 7. 创新性

把可工艺化的 LSD1 药理预处理与遗传缺失数据相互校验，在无需永久改造的前提下提出表观遗传增强策略。

## 8. 局限性

两套 RNA-seq 的场景差异很大；新数据只有一个 72 h 终点且 n=4–5；均为小鼠体系。LSD1 抑制剂可能影响扩增、分化及非 T 细胞，剂量窗口和离靶效应需要进一步定义。

## 9. 在综述中的定位

可归入“制造期表观遗传药理重编程”，并与 G9a/GLP 抑制研究并列；其特点是用遗传 TIL 数据为药理效应提供外部参照。

## 10. 可直接写入综述的表述

> 体外 LSD1 抑制提高 CD8 T 细胞激活响应和过继治疗效力，其转录效应与既往 Lsd1 缺失肿瘤浸润 T 细胞数据具有一致性，提示 LSD1 是可药理利用的表观遗传制动点。

## 11. 数据复用建议

适合构建“药理—遗传一致性”基准：分别计算 GSE272770 和 GSE147130 的基因排序，再评估效应/记忆/耗竭模块的一致性。跨数据集整合宜用效应量或通路层面，不宜直接 batch correction 后声称形成统一队列。

## 12. 转化与安全性关注

制造期用药需要验证残留、洗脱、扩增产量和长期命运。LSD1 在造血和多组织中有广泛功能，细胞外暴露或残留可能带来额外风险。

## 13. 避免误读

- GSE272770 是 9 个样本（5 vs 4），GSE147130 是 8 个样本（4 vs 4）。
- 后者是复用数据，不是本文新测序。
- 药理抑制与遗传敲除、体外与肿瘤内并不等价。
- batch-corrected counts 更适合展示；正式计数差异分析应从 raw counts 开始。
