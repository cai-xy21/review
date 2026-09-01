# 《TOX transcriptionally and epigenetically programs CD8+ T cell exhaustion》精读

## 论文信息

- 作者：Omar Khan、Jason R. Giles、Stephanie McDonald 等
- 期刊：Nature
- 年份：2019；571: 211–218
- DOI：10.1038/s41586-019-1325-x
- 原文：[Nature](https://www.nature.com/articles/s41586-019-1325-x)
- PubMed：[PMID 31207603](https://pubmed.ncbi.nlm.nih.gov/31207603/)
- 全文：[PMC6713202](https://pmc.ncbi.nlm.nih.gov/articles/PMC6713202/)
- GEO SuperSeries：[GSE131871](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE131871)

## 一句话结论

在 LCMV Clone 13 慢性感染中，持续 TCR—calcineurin—NFAT2 信号诱导 TOX；TOX 随后通过转录和染色质重塑建立耗竭程序并支持耗竭 CD8 T 细胞存续，说明 TOX 并非单纯抑制因子，而是慢性刺激下的命运适应调控器。

## 数据护照（先看这一表）

| 维度 | 内容 | 分析提醒 |
|---|---|---|
| 模型 | P14 CD8；LCMV Cl13 day 8 | 小鼠慢性感染 |
| 核心比较 | WT vs Tox KO；control vs TOX overexpression | 含体内必要性和体外充分性测试 |
| 数据层 | bulk RNA-seq、bulk ATAC-seq | 不是 scRNA/scATAC |
| SuperSeries | GSE131871，29 samples | 5 个 subseries |
| processed 包 | GSE131871_RAW.tar，约 20.7 MB | 多为 TXT 计数/peak |
| 每个体内重复 | 常由 10 只小鼠脾细胞汇池 | 单个文库不等于单只小鼠 |
| 细胞输入 | 约 100,000 个分选细胞/重复 | 群体平均，不能看内部异质性 |
| 外部复用 | GSE41867 等 | 不计入本文新生成 29 个样本 |

## 1. 研究要解决的问题

TOX 在耗竭 T 细胞中高表达，但关键问题是：

1. TOX 是耗竭的伴随 marker，还是因果调控器；
2. 什么上游信号诱导 TOX；
3. TOX 如何同时影响转录和染色质；
4. 删除 TOX 是否会把耗竭细胞变成正常 effector/memory，还是导致其消失。

## 2. 方法框架

### 2.1 体内必要性

在 LCMV Cl13 慢性感染中比较 WT 与 Tox-deficient P14 CD8 T cells，检测抑制受体、细胞因子、增殖、存续、bulk RNA 和 bulk ATAC。

### 2.2 体外充分性

将 TOX ectopic expression 与 control 比较：

- 原代 CD8 T cells 的 RNA/ATAC；
- NIH3T3 的 RNA；
- 用于判断 TOX 是否可直接驱动部分分子程序。

### 2.3 上游与蛋白互作

作者用 calcineurin/NFAT2 干预、蛋白质组/互作实验等解析 TOX 诱导，并提出 HBO1/KAT7 等染色质调控伙伴。

## 3. 数据规模与图谱组成

### 3.1 GSE131871 SuperSeries 总览

[GSE131871](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE131871) 当前含 29 个 bulk 样本，分为五个子系列：

| 子系列 | 技术/系统 | 样本数 | 主要比较 |
|---|---|---:|---|
| GSE132983 | ATAC，原代 CD8 | 6 | control vs TOX overexpression |
| GSE132984 | RNA，原代 CD8 | 5 | control vs TOX overexpression |
| GSE132985 | RNA，NIH3T3 | 6 | control vs TOX overexpression |
| GSE132986 | ATAC，Cl13 day 8 P14 | 6 | WT vs Tox KO |
| GSE132987 | RNA，Cl13 day 8 P14 | 6 | WT vs Tox KO |
| 合计 | bulk RNA/ATAC | 29 | 体外充分性 + 体内必要性 |

GSE131871_RAW.tar 当前约 20.7 MB，收集处理后 TXT 文件。原始 reads 分散在各 subseries 的 SRA 链接。

### 3.2 GSE132983：TOX overexpression ATAC

6 个 bulk ATAC 样本比较原代 CD8 T cells 中 control 与 TOX overexpression，通常每组 3 个重复。用于测量 TOX 充分表达后可及性改变。

TOX ectopic expression 改变约 378 个可及区域。这个数说明 TOX 可影响染色质，但远小于完整耗竭景观，提示持续抗原和其他因子共同参与。

### 3.3 GSE132984：TOX overexpression RNA

5 个原代 CD8 bulk RNA 样本：

- control 2；
- TOX overexpression 3。

GEO 处理后包约 600 KB。样本数不平衡，差异分析应使用实际 replicate 设计。

### 3.4 GSE132985：NIH3T3 RNA

6 个 bulk RNA 样本，在 NIH3T3 中比较 control 与 TOX expression。该数据测试 TOX 在非 T 细胞背景的转录作用，有助于区分一般转录效应与 T 细胞上下文依赖。

它不是 T 细胞数据，不能并入 CD8 状态图谱。

### 3.5 GSE132986：WT vs Tox KO ATAC

6 个体内 bulk ATAC 样本，比较 LCMV Cl13 day 8 的 WT 与 Tox-deficient P14 cells，通常每组 3 个重复。

论文方法指出每个体内重复可汇池约 10 只小鼠脾脏，并分选约 100,000 个细胞建库。因此统计层级应写：

- 6 个 ATAC libraries；
- 每个 library 是一个 pooled biological replicate；
- 每个 replicate 的小鼠输入约 10 只；
- 不是 60 个独立测序样本。

### 3.6 GSE132987：WT vs Tox KO RNA

6 个体内 bulk RNA 样本，与 GSE132986 对应的主要比较相同：Cl13 day 8 WT vs Tox KO P14。RNA 和 ATAC 是相同生物设计，但是否为同一汇池拆分需按 GSM 元数据核实，不应自动假设严格配对。

### 3.7 下载方式

下载 SuperSeries 全部处理后文件：

~~~bash
wget -c \
  https://ftp.ncbi.nlm.nih.gov/geo/series/GSE131nnn/GSE131871/suppl/GSE131871_RAW.tar
~~~

单独下载 subseries：

~~~bash
wget -c \
  https://ftp.ncbi.nlm.nih.gov/geo/series/GSE132nnn/GSE132984/suppl/GSE132984_RAW.tar
~~~

原始 reads：

1. 进入每个 subseries 页面；
2. 打开 SRA Run Selector；
3. 导出 RunInfo 与 Accession List；
4. 使用 prefetch/fasterq-dump 或 ENA 镜像；
5. RNA 与 ATAC 分别建立独立 pipeline。

### 3.8 处理后文件如何读

GEO RAW.tar 的 TXT 可能为：

- gene-level counts/normalized expression；
- peak counts；
- BED/差异 peak；
- coverage summary。

下载后先检查表头和样本名：

~~~r
files <- list.files("GSE131871_RAW", full.names=TRUE)
print(files)
x <- read.delim(files[1], check.names=FALSE)
str(x)
~~~

若只复现图，处理后文件足够；若需要统一 RNA/ATAC 定量，应回到原始 reads。不要把 normalized expression 当 raw counts，也不要把 peak table 当 fragment file。

### 3.9 本文未新生成但被复用的数据

作者还利用既有耗竭表达资源，如 GSE41867，进行比较或签名分析。这些外部数据不能计入 GSE131871 的 29 个新样本，引用时应分别列 accession。

## 4. 核心机制

### 4.1 上游：慢性 TCR—calcineurin—NFAT

持续 TCR 信号通过 calcineurin/NFAT2 诱导 TOX。TOX 随后形成正反馈或维持网络，使耗竭程序在慢性抗原环境中稳定。

### 4.2 TOX 建立转录与表观遗传程序

Tox 缺失显著改变抑制受体、转录因子和染色质可及性。TOX 不是只控制一个 PD-1 基因，而是作用于整个命运程序。

### 4.3 耗竭程序同时支持存续

Tox KO cells 没有简单转为功能完好的 effector/memory；它们在慢性感染中难以持久存在。这说明耗竭是对持续刺激的适应状态，部分“抑制程序”具有保护作用。

## 5. 关键图表怎么读

- WT/KO RNA 与 ATAC：支持 TOX 必要性，但 bulk 平均可能混合存活偏倚。
- overexpression：支持部分充分性，不代表单一 TOX 可重建全部耗竭。
- KO 功能：短期功能增强与长期存续下降必须一起解释。
- protein interaction：候选互作需进一步做位点/复合物因果验证。

## 6. 创新点

1. 将 TOX 从 marker 提升为耗竭命运调控器。
2. 同时做 KO 和 overexpression。
3. 同时测 RNA 与 ATAC。
4. 揭示“解除抑制”与“维持细胞存续”之间的权衡。

## 7. 局限性

1. 全部测序为 bulk，不能解析 progenitor 与 terminal Tex。
2. KO 会改变细胞存活和群体组成，部分组学差异可能是选择效应。
3. 体内重复为多鼠汇池，独立样本数仅每组约 3。
4. NIH3T3 结果不等同 T 细胞染色质背景。
5. day 8 是较早阶段，长期耗竭仍需时间序列。
6. 小鼠慢性感染不能直接等同人肿瘤。

## 8. 对本综述的作用

这篇论文是“link cell state/function transitions with molecular drivers”的核心机制文献：

- 上游可导航变量：TCR 强度/持续时间、calcineurin/NFAT；
- 关键命运节点：TOX；
- 下游状态：耗竭转录、染色质和存续；
- 设计权衡：完全抑制 TOX 可能提高短期功能却损害持久性。

它提示细胞治疗优化不应简单追求最低 TOX，而应调节 TOX 的时间、幅度和与记忆程序的平衡。

## 9. 可直接写进综述的表述

> 慢性 TCR—calcineurin—NFAT 信号诱导 TOX，后者协同重塑转录与染色质并维持耗竭 CD8 T 细胞存续；Tox 缺失虽削弱耗竭表型，却未生成稳定的优质效应细胞，揭示状态导航中的功能—持久性权衡。

## 10. 最容易误读的地方

- 29 是 bulk libraries，不是单细胞数。
- 每个体内 replicate 可由约 10 只小鼠汇池。
- GSE132985 是 NIH3T3，不是 T cells。
- TOX KO 不等于把 exhausted cells 恢复为 memory。
- GSE131871 主要是 RNA/ATAC；不要把论文的互作实验误写成 GEO 中独立 ChIP 数据集。
