# 《Multi-phenotype CRISPR-Cas9 Screen Identifies p38 Kinase as a Target for Adoptive Immunotherapies》精读

## 论文信息

- 作者：Devikala Gurusamy、Amanda N. Henning、Tori N. Yamamoto 等
- 期刊：*Cancer Cell*
- 年份：2020；37: 818–833.e9
- DOI：10.1016/j.ccell.2020.05.004
- 原文：[Cancer Cell](https://doi.org/10.1016/j.ccell.2020.05.004)
- PubMed：[PMID 32516591](https://pubmed.ncbi.nlm.nih.gov/32516591/)
- RNA-seq：[GEO GSE114087](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE114087)；BioProject PRJNA464286；SRA SRP144714

## 一句话结论

针对 TCR 驱动激酶的多表型 CRISPR-Cas9 筛选显示，p38/MAPK14 同时限制 T 细胞扩增、记忆样表型、氧化还原稳态和基因组稳态；p38 抑制可改善小鼠和人 TIL、TCR-T 与 CAR-T 的代谢质量和抗肿瘤效力。

## 数据护照（先看这一表）

| 数据层 | 内容 | 公开位置 | 分析提醒 |
|---|---|---|---|
| 多表型 CRISPR screen | 25 个 TCR 驱动激酶，每基因 3 条 sgRNA；读出扩增、CD62L、ROS、γH2AX | 论文补充表/图 | 属于阵列式多表型筛选，不是全基因组 pooled screen |
| RNA-seq | 小鼠 Pmel CD8⁺ T 细胞，vehicle vs p38 inhibitor BIRB796，第 5/10 天 | GSE114087，共 14 个样本 | 第 5 天每组 4 个、第 10 天每组 3 个；时间点需分层 |
| 处理后 RNA | 每样本 TXT 汇总于 RAW TAR | GEO TAR 约 2.8 MB | 可快速做表达/GSEA；原始 reads 走 SRA |
| 代谢组 | p38 抑制前后全局代谢物 | Supplementary Table S3 | 不在 GSE114087；需从期刊补充文件下载 |
| TCR/功能/流式 | 人 TIL、TCR-T、CAR-T 的表型和功能 | 正文/补充材料 | 未作为独立公共原始数据集提供 |

## 1. 研究要解决的问题

过继细胞治疗所需的“好 T 细胞”同时具有扩增、记忆、低氧化压力和低 DNA 损伤等多种性质。单终点筛选容易找到只改善一个指标却损害其他指标的靶点。本文用多表型设计寻找能同时改善多个产品质量属性的 TCR 下游激酶。

## 2. 筛选设计

作者选择 25 个 TCR 驱动激酶，每基因使用 3 条 sgRNA，在 Cas9 T 细胞中分别测量：

1. 细胞扩增；
2. CD62L 记忆样表型；
3. 细胞内/线粒体 ROS；
4. γH2AX 基因组压力。

将多个读出整合后，MAPK14/p38α 是兼具多方面收益的突出命中。随后以 BIRB796 等 p38 抑制策略在小鼠 Pmel、人黑色素瘤/乳腺癌 TIL、NY-ESO-1 TCR-T 和 CD19 CAR-T 中验证。

## 3. GSE114087 数据详解

该系列为小鼠 Pmel CD8⁺ T 细胞 RNA-seq，共 14 个样本：

- 第 5 天 vehicle 4 个、BIRB796 4 个；
- 第 10 天 vehicle 3 个、BIRB796 3 个。

时间点和药物构成 2×2 设计，但重复数不完全相同。分析时可分别比较每个时间点，也可建立包含 `time + treatment + time:treatment` 的模型；不能直接把第 5/10 天合并成 7 vs 7 而忽略培养时间。

## 4. 数据下载方式

### 4.1 GEO 处理后数据

```text
https://ftp.ncbi.nlm.nih.gov/geo/series/GSE114nnn/GSE114087/suppl/GSE114087_RAW.tar
```

TAR 约 2.8 MB，包含每个 GSM 的 TXT 表，适合快速复现热图、通路富集和时间分层药物效应。

### 4.2 原始 reads

从 GSE114087 的 SRA 链接进入 SRP144714，导出 RunInfo.csv，再使用：

```bash
prefetch --option-file SraAccList.txt
fasterq-dump --split-files --threads 8 SRRxxxxxxx
```

### 4.3 筛选和代谢组

CRISPR 靶点/sgRNA 与多表型结果、全局代谢组应从期刊 Supplementary Tables 下载；它们不包含在 GEO RNA-seq accession 中。下载后应单独记录表格版本和字段说明。

## 5. 主要发现

1. 有效抗肿瘤 T 细胞质量不能由单一扩增或杀伤指标定义。
2. p38 同时调节记忆、氧化还原稳态、DNA 损伤和代谢适能。
3. p38 抑制提高小鼠和人 TIL/TCR-T/CAR-T 的治疗相关表型与功能。
4. RNA-seq 和代谢组支持 p38 抑制带来的持续状态重塑，而非仅短时信号变化。

## 6. 转化意义

p38 抑制的吸引力在于可作为 ex vivo 制造添加剂，避免永久基因编辑。但药物加入时间、撤药、残留和不同 T 细胞产品的适用性需要单独优化；广谱 p38 抑制也可能影响多个亚型和非 T 细胞过程。

## 7. 推荐图版

- 多表型筛选矩阵/综合排名图。
- p38 抑制对 CD62L、ROS、γH2AX 和扩增的并列图。
- RNA-seq/代谢组机制图。
- TIL、TCR-T、CAR-T 的体内疗效验证。

## 8. 创新价值

1. 以多个产品质量属性而非单终点筛选 ACT 靶点。
2. 将基因扰动、药理抑制、转录组和代谢组串联。
3. 横跨小鼠与多类人源细胞产品验证 p38。

## 9. 局限性

1. 仅筛选预选的 25 个 TCR 驱动激酶，不是无偏全基因组筛选。
2. 阵列式 screen 的重复和综合评分会影响命中排名。
3. BIRB796 并非只作用于 MAPK14，药理和遗传证据需分别解读。
4. GSE114087 只有小鼠 Pmel RNA-seq，人源产品的底层组学未同等公开。
5. 临床制造中的剂量、残留和安全性仍未解决。

## 10. 对综述的作用

适合放在“多表型 CRISPR 筛选优化细胞产品”和“p38 作为制造期可药物靶点”部分。它是从筛选指标设计角度很有代表性的论文。

## 11. 可直接用于综述的观点

> 多表型 CRISPR 筛选表明，p38/MAPK14 同时限制 T 细胞扩增、记忆样状态、氧化还原和基因组稳态；在扩增过程中抑制 p38 可跨 TIL、TCR-T 和 CAR-T 平台改善治疗效力（Cancer Cell 2020, Gurusamy）。

## 12. 数据复用建议

- 用 GSE114087 分时间点重做差异表达，并显式测试 treatment×time 交互。
- 筛选结果和代谢组从 Supplementary Tables 独立下载、独立引用。
- 对人源外推应优先把论文功能图作为证据，不要声称 GEO 含人 TIL RNA-seq。

## 13. 避免误读

- 这是 25 激酶的定向多表型 screen，不是 genome-wide screen。
- GSE114087 的 14 个样本均为小鼠 Pmel 体系。
- GEO 不包含全部流式、代谢组和人 TIL 原始数据。
- p38 抑制的药物效果不能完全等同于 MAPK14 单基因缺失。
