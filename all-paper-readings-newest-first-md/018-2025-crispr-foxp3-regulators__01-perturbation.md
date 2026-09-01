# 《Genome-wide CRISPR screen in human T cells reveals regulators of FOXP3》精读

## 论文信息

- **作者**：Kelvin Y. Chen、Tatsuya Kibayashi、Ambre Giguelay 等
- **期刊与年份**：*Nature*, 2025；642: 191–202
- **DOI**：10.1038/s41586-025-08795-5
- **本地原文**：[PDF](<D:/research/review/perturbation33references/26-Genome-wide CRISPR screen in human T cells reveals regulators of FOXP3.pdf>)
- **测序数据**：[DDBJ BioProject PRJDB16517](https://ddbj.nig.ac.jp/search/entry/bioproject/PRJDB16517)
- **Perturb-icCITE-seq 代码**：[GitHub](https://github.com/agiguelay/Perturb-icCITEseq)

## 一句话结论

原代人 naive CD4⁺ T 细胞的全基因组 CRISPR KO 筛选和 Perturb-icCITE-seq 共同识别 RBPJ–NCOR 抑制复合物为 FOXP3 诱导的情境特异负调控器；RBPJ 删除增强 iTreg 形成、功能和反复刺激后的稳定性，并促进 FOXP3 CNS2 去甲基化。

## 数据护照

| 模块 | 规模/组成 | 获取位置 |
|---|---|---|
| 全基因组 KO 筛选 | 19,114 个基因；77,441 个 library sgRNA + 50 个 FOXP3 control sgRNA，即 77,491；约 4 guides/gene | Supplementary Table 1 + PRJDB16517 |
| 分选 readout | FOXP3 表达 top 15% 与 bottom 15%；2 次筛选重复 | screen sequencing |
| 筛选覆盖度 | >1 亿 RFP⁺细胞；目标 ≥1,000 cells/sgRNA | Methods |
| Perturb-icCITE-seq | 295 个靶基因；约 905–907 个 gRNA；277 个表面抗体 + 46 个胞内抗体 | PRJDB16517 + code |
| 单细胞响应矩阵 | 转录组 + guide + hashtag + 表面/胞内蛋白；分析到 2,192 个受影响基因 | Perturb-icCITE-seq |
| RBPJ dense mutagenesis | 151 个 RBPJ exon-targeting gRNA + 20 NTC；3 位独立供体 | Supplementary Table 4 / PRJDB16517 |

## 1. 研究问题

体外 TGFβ + IL-2 可诱导 FOXP3，但人 iTreg 往往不稳定。作者希望系统识别决定 FOXP3 诱导和稳定性的基因网络，并区分“改变 FOXP3 蛋白”与“广泛改变 T 细胞状态”的扰动。

## 2. 实验设计与方法框架

作者以 cord-blood naive CD4⁺CD25⁻ T 细胞为起点，使用 SLICE 流程：低 MOI 慢病毒导入 sgRNA、Cas9 蛋白电转、iTreg 诱导、按胞内 FOXP3 高低分选并测序 guide。随后用阵列式 RNP 验证，并把精选命中放入 Perturb-icCITE-seq，同时读出 RNA、表面蛋白和胞内转录因子。最后聚焦 RBPJ 的结构域、复合物、CNS2 甲基化与体内功能。

## 3. 数据规模与图谱组成

### 3.1 全基因组筛选：77,491 条 guide、19,114 个基因

Methods 记录的主 library 为 77,441 条、覆盖 19,114 个基因，并另加 50 条靶向 FOXP3 编码区的 control guide；图中因此报告总计 **77,491 gRNAs**。两种数字分别是“基础 library”和“实际总 guide”，不矛盾。

实验以 MOI 0.3 导入 guide，扩增后收集超过 1 亿个 RFP⁺ 细胞，以维持至少 1,000 cells/sgRNA。iTreg 诱导 72 h 后分选 FOXP3 top 15% 和 bottom 15%，用 MAGeCK 比较富集。筛选做两次重复；Supplementary Table 1 提供 gene/guide-level 结果。

### 3.2 全基因组筛选的统计单位

guide read count 是技术测量单位，独立筛选重复才是筛选层面的重复。后续阵列验证使用 4–5 位供体，但不能用这些供体数反推 pooled screen 具有 4–5 个独立供体重复。报告应明确区分：

- discovery screen：2 次筛选重复；
- arrayed validation：不同命中使用 4 或 5 位独立供体；
- mechanistic assays：供体数依图而异。

### 3.3 Perturb-icCITE-seq：多模态单细胞扰动图谱

该模块将约 295 个候选靶基因组成 CROP-seq library。正文图示 905 个不同 gRNA，Methods 写 907 个；最稳妥的表述是“约 905–907 条 guide”，并以下载后 library definition 为准。

单细胞层同时测量：

1. 3′ gene expression；
2. 每个细胞携带的 guide；
3. donor/sample hashtag；
4. 277 个表面蛋白；
5. 46 个胞内蛋白，包括 FOXP3 等转录因子。

作者由此把靶基因聚成 11 个共功能 perturbation modules，并将响应基因聚成激活、naive-like、IL-2–STAT5、氧化磷酸化、糖酵解等 program。主矩阵分析到 2,192 个显著受影响基因。

### 3.4 RBPJ dense mutagenesis

聚焦 library 包含 151 条覆盖 RBPJ 外显子的 guide 和 20 条 NTC，以 3×10⁶ 细胞、MOI 0.3 在 3 位独立供体中分别筛选。Supplementary Table 4 将 guide enrichment 映射到蛋白坐标，用来定位与 FOXP3 调控相关的 RBPJ 结构域。

### 3.5 PRJDB16517 包含什么、如何下载

论文声明所有本文新生成的 NGS 数据统一归档于 DDBJ Sequence Read Archive project `PRJDB16517`，参考基因组 GRCh38。项目应包括 pooled screen amplicon、Perturb-icCITE-seq 的 GEX/feature-barcode/guide 文库，以及机制实验相关测序。

推荐流程：

1. 在 DDBJ Search 打开 BioProject，导出 SRA XML/run table；
2. 用 sample title 和 library strategy 分开 screen amplicon、GEX、antibody-derived tag 与 guide capture；
3. 从 Supplementary Tables 1、4 获取已经聚合的筛选结果；
4. 复现单细胞处理时按 GitHub 脚本所需文件名组织 FASTQ 和 feature definitions。

DDBJ 项目页是原始数据入口；全基因组命中表明确由 Supplementary Tables 1 和 4 发布，因此不能仅检查 DDBJ 是否有一个“gene score matrix”来判断数据完整性。

### 3.6 下载后的首要核验

逐文库确认 donor、screen replicate、FOXP3 bin、guide library version 和 read structure。Perturb-icCITE-seq 必须先做 hashtag 去多重、guide assignment 和蛋白背景校正，再比较扰动；把 multiplet 或多 guide 细胞直接作为单一 KO 会产生错配。

## 4. 主要结果

筛选重现 TGFBR1、SMAD3/4、FOXP3、UHRF1 等已知调控器，并发现 RBPJ、HDAC3、GPS2、TBL1XR1、NCOR1/2 等负调控网络。RBPJ KO 提高 FOXP3、增强抑制功能，并在反复刺激后保持更稳定的 Treg 表型。

## 5. 机制理解

RBPJ 在这里主要以转录抑制复合物成员发挥作用，而非经典 Notch 激活路径。删除 RBPJ 解除 NCOR/HDAC3 相关抑制，并促进 FOXP3 CNS2 去甲基化，使诱导性 FOXP3 更接近稳定谱系程序。

## 6. 推荐重点阅读的图

- 全基因组 FOXP3 high/low volcano 与通路富集。
- Perturb-icCITE-seq 多模态网络图。
- RBPJ–NCOR/HDAC3 机制与 dense mutagenesis。
- 反复刺激、CNS2 去甲基化和功能稳定性图。

## 7. 创新性

把胞内 FOXP3 蛋白分选扩展到全基因组原代人 T 细胞筛选，再用同时测胞内蛋白的 Perturb-icCITE-seq 解析命中对细胞状态的多维影响。

## 8. 局限性

发现体系是 cord-blood naive CD4⁺ T 细胞的体外 iTreg 诱导，不完全等同于成人外周或组织 Treg。不同模块的供体和重复层级不一致。RBPJ 删除可能有长期、组织特异或肿瘤免疫副作用。

## 9. 在综述中的定位

适合作为“多模态 CRISPR 筛选解析 Treg 命运稳定性”的代表，并提醒 CD8⁺ 抗肿瘤增强与 Treg 工程可能追求相反方向的免疫调控。

## 10. 可直接写入综述的表述

> 全基因组 FOXP3 蛋白分选筛选与 Perturb-icCITE-seq 将 RBPJ–NCOR 复合物识别为人 iTreg 分化的情境特异制动器；RBPJ 删除通过加强 FOXP3 稳定表达和 CNS2 去甲基化改善 iTreg 持久性。

## 11. 数据复用建议

可用 Supplementary Table 1 做通路级稳健排名，再以 Perturb-icCITE-seq 区分“选择性提高 FOXP3”与“造成广泛激活/代谢改变”的命中。单细胞差异分析应按 donor pseudobulk 或混合模型处理，不能把同一供体数千细胞当独立重复。

## 12. 转化与安全性关注

面向自身免疫细胞治疗时需验证成人外周 T 细胞、长期反复刺激、炎症细胞因子和体内组织归巢；同时评估 RBPJ/NCOR 广泛转录作用引起的异常分化与致瘤风险。

## 13. 避免误读

- 77,441 是基础 library guide 数；加 50 条 FOXP3 control 后总计 77,491。
- Perturb-icCITE-seq 约 905–907 guides 的轻微差异应以实际 library 文件为准。
- 2 次 pooled screen 重复不等于所有验证实验的 4–5 位供体。
- PRJDB16517 提供原始测序，命中汇总表主要在论文补充表。

