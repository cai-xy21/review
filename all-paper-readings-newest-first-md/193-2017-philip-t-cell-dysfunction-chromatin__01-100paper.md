# 《Chromatin states define tumour-specific T cell dysfunction and reprogramming》精读

## 论文信息

- 作者：Mary Philip、Laura Fairchild、Li Sun 等
- 期刊：Nature
- 年份：2017；545: 452–456
- DOI：10.1038/nature22367
- 原文：[Nature](https://www.nature.com/articles/nature22367)
- PubMed：[PMID 28514453](https://pubmed.ncbi.nlm.nih.gov/28514453/)
- 全文：[PMC5693219](https://pmc.ncbi.nlm.nih.gov/articles/PMC5693219/)
- SuperSeries：[GEO GSE89309](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE89309)
- bulk RNA-seq：[GEO GSE89307](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE89307)
- bulk ATAC-seq：[GEO GSE89308](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE89308)

## 一句话结论

在自发性肝癌模型中，肿瘤特异 CD8 T 细胞经历从可逆的早期 dysfunction state 1 到难以重编程的固定 state 2；bulk RNA-seq 与 ATAC-seq 显示这一转变伴随大规模染色质重塑，并可用 CD38、CD101 和 CD5 表型区分可塑性窗口。

## 数据护照（先看这一表）

| 维度 | 内容 | 分析提醒 |
|---|---|---|
| 模型 | 自发性肝肿瘤 TCRTAG；急性 LmTAG 对照 | 小鼠固定 TCR/抗原模型 |
| 核心问题 | dysfunction state 1 → state 2 | 状态来自群体分选与时间序列，不是单细胞轨迹 |
| SuperSeries | GSE89309，97 samples | 含两个 subseries 的全部样本 |
| RNA-seq | GSE89307，38 bulk samples | 多时间点、转移/重编程条件 |
| ATAC-seq | GSE89308，59 bulk samples | 含 mouse 与 human 分选细胞 |
| 处理后包 | RNA 约 5.2 MB；ATAC 约 91.6 MB | 多为 TXT 信号/peak 文件 |
| 原始量级 | SuperSeries umbrella 约 0.25 TB | 当前 BioProject 统计，归档可能变化 |
| 技术纠错 | bulk RNA/ATAC | 不是 scRNA/scATAC |

## 1. 研究要解决的问题

肿瘤特异 T 细胞 dysfunction 是否是单一终点，还是存在可逆与固定阶段？作者关注：

1. T 细胞进入肿瘤后多快出现 dysfunction；
2. 哪个阶段仍能通过移出肿瘤或改变环境恢复；
3. 染色质状态何时固定；
4. 能否用表面 marker 识别仍可重编程的细胞。

## 2. 方法框架

### 2.1 抗原匹配的急性与肿瘤模型

使用识别 SV40 large T antigen 的 TCRTAG CD8 T cells：

- LmTAG 急性感染：形成正常 effector/memory 参照；
- 自发性肝肿瘤：持续抗原驱动 dysfunction；
- 多时间点分选肿瘤特异 T cells；
- 过继转移到无肿瘤或急性感染环境测试可逆性。

固定 TCR 和抗原减少特异性混杂，但也限制受体多样性和人类外推。

### 2.2 分子与功能测量

- bulk RNA-seq；
- bulk ATAC-seq；
- 流式表型和功能；
- 过继转移/再挑战；
- 人 melanoma 与 NSCLC PD-1 高表达 TIL 的 bulk ATAC 验证。

## 3. 数据规模与图谱组成

### 3.1 GSE89309 SuperSeries

[GSE89309](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE89309) 汇总：

| 子系列 | 技术 | 样本数 |
|---|---|---:|
| GSE89307 | bulk RNA-seq | 38 |
| GSE89308 | bulk ATAC-seq | 59 |
| 合计 | 两种 bulk assay | 97 |

GSE89309_RAW.tar 当前约 96.8 MB，主要是处理后 TXT 文件合集。它不是 97 个单细胞，也不是 97 名动物；每个样本是一个分选细胞群体文库。

### 3.2 GSE89307 bulk RNA-seq

[GSE89307](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE89307) 含 38 个样本，覆盖：

- naive TCRTAG；
- LmTAG 急性感染 day 5、day 7、memory；
- 肝肿瘤 TCRTAG day 5、7、14、21、28、35、60；
- 从不同肿瘤阶段取出后转移/再挑战的重编程实验；
- 其他对照和重复。

处理后 GSE89307_RAW.tar 约 5.2 MB。原始 reads 由 SRA/BioProject 下载；对应 BioProject 当前报告 38 个 SRA experiments、约 193 Gbases、96,971 MB 归档量，约 94.7 GiB。

下载：

~~~bash
wget -c \
  https://ftp.ncbi.nlm.nih.gov/geo/series/GSE89nnn/GSE89307/suppl/GSE89307_RAW.tar
~~~

38 个样本含多个实验批次和时间点。统计分析需从 GSM characteristics 重建 experiment、day、condition、transfer 和 replicate，不能把所有时间点当成同一纵向动物。

### 3.3 GSE89308 bulk ATAC-seq

[GSE89308](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE89308) 含 59 个样本，处理后 GSE89308_RAW.tar 约 91.6 MB。组成包括：

- 小鼠 naive、acute effector/memory；
- 小鼠肿瘤特异 TCRTAG 多阶段；
- 重编程/转移条件；
- 人健康 naive/central-memory CD8 T cells；
- 人 melanoma 或 NSCLC 中 PD-1hi TIL。

人数据用于检验 tumour dysfunction 染色质模式的保守性，但并非一个大规模患者图谱。下载处理后文件：

~~~bash
wget -c \
  https://ftp.ncbi.nlm.nih.gov/geo/series/GSE89nnn/GSE89308/suppl/GSE89308_RAW.tar
~~~

ATAC 处理后 TXT 可能是 peaks、normalized coverage 或差异结果，而不是可直接用于 ArchR/Signac 的 fragments.tsv。若需统一 peak calling，必须下载原始 reads。

### 3.4 原始数据总量

GSE89309 对应 umbrella BioProject 当前统计约：

- 548 Gbases；
- 约 0.25 TB archive。

这是当前数据库统计，不是论文正文固定数字；不同下载格式会产生不同磁盘占用。完整重处理建议预留至少 0.5 TB，并记录 SRA run table 和校验和。

### 3.5 数据的时间—状态结构

论文不是只比较“正常 vs 肿瘤”，而是沿时间和可塑性构造参照：

| 轴 | 代表条件 | 解释 |
|---|---|---|
| 正常分化 | naive → acute effector → memory | 无持续抗原的参照 |
| 肿瘤 dysfunction | day 5 → 60 | 持续肿瘤抗原下的时间进程 |
| 可逆性 | 早/晚期细胞移出肿瘤 | 检验环境改变后能否恢复 |
| 人体对应 | 健康 CD8 vs PD-1hi TIL | 检验染色质特征保守性 |

state 1 和 state 2 是综合时间、功能、RNA 和 ATAC 定义的群体阶段。不能从横断面 UMAP 或单一 marker 直接等同。

### 3.6 下载后如何整理

建议建立一张样本级 manifest，至少含：

- GSM；
- assay；
- species；
- tissue/tumour；
- T-cell receptor/model；
- day；
- state；
- transfer/rechallenge；
- replicate；
- processed file；
- SRA accession。

快速复图使用 GEO 处理后 TXT；重新定量则从 SRA 下载：

~~~bash
curl -L \
  "https://trace.ncbi.nlm.nih.gov/Traces/sra-db-be/runinfo?acc=GSE89309" \
  -o GSE89309_runinfo.csv
~~~

对 bulk RNA 使用 sample-level counts；对 ATAC 统一比对、过滤和 consensus peaks。RNA 和 ATAC 的 38/59 样本并非全都一一配对。

## 4. 两个 dysfunction chromatin states

### 4.1 State 1：早期、仍可重编程

肿瘤进入早期的 TCRTAG 细胞已失去部分效应功能并表达抑制受体，但染色质仍具可塑性。移出肿瘤或在急性环境中再刺激可恢复部分功能和分化。

### 4.2 State 2：晚期、固定

长期肿瘤抗原暴露后形成 state 2，染色质景观重排并难以在环境改变后恢复。这定义了“固定 dysfunction”，而不只是更高的 PD-1 表达。

### 4.3 表面标志

CD38 和 CD101 高、CD5 低与固定 state 2 相关，可用于分选不同可塑性细胞。checkpoint receptors 单独不足以区分早期可逆与晚期固定状态。

## 5. 关键图表怎么读

- 时间序列 RNA/ATAC：多数样本是不同动物的横断面群体，不是同一细胞连续跟踪。
- 转移实验：是可逆性的强功能证据，但环境变化同时改变多种信号。
- 人 bulk ATAC：支持保守染色质模式，不能给出患者内亚群比例。
- marker flow：CD38/CD101/CD5 是组合富集，不是绝对门控真值。

## 6. 创新点

1. 把 dysfunction 分为可逆与固定染色质阶段。
2. 用功能重编程实验验证表观状态，而非只做组学关联。
3. 提供可用于分选可塑性窗口的表面 marker。
4. 加入人肿瘤 bulk ATAC 验证。

## 7. 局限性

1. 全部测序为 bulk，无法解析 state 内部异质性。
2. 固定 TCR/单一抗原模型不能覆盖天然肿瘤受体多样性。
3. 时间点多，但每个群体重复数和配对关系有限。
4. 人体验证样本规模小且也是 bulk。
5. ATAC 状态与增强子因果需要编辑实验验证。
6. state 1/2 的门槛在不同癌种和治疗中可能移动。

## 8. 对本综述的作用

本论文直接回答“何时还能导航”：

- 早期 state 1 是可塑性窗口；
- 晚期 state 2 代表表观遗传锁定；
- 状态监测应加入 CD38/CD101/CD5 或相应分子模块；
- 干预应在锁定前实施，或同时引入染色质重编程。

它也是“实时优化系统”的概念依据：系统不仅预测目标状态，还应估计当前细胞离不可逆边界有多远。

## 9. 可直接写进综述的表述

> 肿瘤特异 CD8 T 细胞在持续抗原下由可逆的 dysfunction state 1 过渡到染色质固定的 state 2；CD38、CD101 和 CD5 的组合可富集不同可塑性阶段，提示细胞状态导航存在有限时间窗口。

## 10. 最容易误读的地方

- GSE89309 的 97 是 bulk 样本数，不是细胞数或供者数。
- GSE89307 和 GSE89308 分别是 bulk RNA 与 bulk ATAC。
- state 1/2 不是单细胞聚类标签。
- 人数据是小规模 bulk ATAC 验证。
- 时间相关性加转移实验支持可塑性变化，但不等于直接活细胞谱系追踪。
