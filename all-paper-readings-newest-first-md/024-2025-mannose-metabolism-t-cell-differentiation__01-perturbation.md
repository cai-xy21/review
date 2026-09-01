# 《Mannose metabolism reshapes T cell differentiation to enhance anti-tumor immunity》精读

## 论文信息

- 作者：Yajing Qiu、Yapeng Su、Ermei Xie 等
- 期刊：*Cancer Cell*
- 年份：2025；43(1): 103–121.e8
- DOI：10.1016/j.ccell.2024.11.003
- 原文：[Cancer Cell](https://doi.org/10.1016/j.ccell.2024.11.003)
- CITE-seq：[BioProject PRJNA1066390](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA1066390)
- bulk RNA-seq：[BioProject PRJNA1078750](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA1078750)
- CUT&RUN：[BioProject PRJNA1078771](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA1078771)

## 一句话结论

D-mannose 补充通过重塑代谢、O-GlcNAc 修饰和染色质状态，稳定 β-catenin 并维持 TCF1/LEF1 相关干性程序，从而降低 T 细胞向耗竭分化并增强过继细胞治疗的抗肿瘤效力。

## 数据护照（先看这一表）

| 数据 | 比较与样本 | accession | 体量/提醒 |
|---|---|---|---|
| CITE-seq | B16-OVA 中 OT-I TIL，对照 vs D-mannose | SRR27620041、SRR27620042；PRJNA1066390 | 两个大型运行，SRA 归档各约 45–46 GB；样本数仅 2，不能将细胞当生物重复 |
| bulk RNA-seq | 对照 3 个重复、D-mannose 3 个重复 | SRR28027798–SRR28027803；PRJNA1078750 | 6 个 paired-end 运行，各约 2 GB 级 |
| CUT&RUN | IgG、H3K4me3、H3K27me3；对照和 D-mannose | SRR28028215–SRR28028224；PRJNA1078771 | 数据页共 10 个运行；论文正文只写到 222，漏列 223–224 |
| 流式/功能/代谢 | 小鼠和人 T 细胞、ACT 及肿瘤模型 | 正文与补充材料 | 未作为统一机器可读数据集公开 |
| 代码 | 生信处理代码 | 无公开仓库 | 论文明确“不报告原创代码” |

## 1. 研究要解决的问题

耗竭 T 细胞的代谢异常是原因还是结果，以及能否用简单、可转化的代谢补充在扩增阶段保留干性，是本文的核心问题。作者把 mannose 代谢、蛋白 O-GlcNAcylation、β-catenin 稳定和 T 细胞分化连接成一条机制链。

## 2. 实验与分析框架

1. 在肿瘤浸润 T 细胞中比较 mannose 代谢状态。
2. 用 D-mannose 处理小鼠/人 T 细胞，评估 TCF1、PD-1/TIM-3、细胞因子、记忆表型和抗肿瘤能力。
3. 以 CITE-seq 描述 B16-OVA 中 OT-I TIL 的状态组成变化。
4. 用 bulk RNA-seq、代谢实验和 CUT&RUN 追踪转录与 H3K4me3/H3K27me3 改变。
5. 验证 OGT—O-GlcNAc—β-catenin 轴及其对 TCF7/LEF1 干性程序的作用。

## 3. 数据内容详解

### 3.1 CITE-seq

两个运行分别对应对照和 D-mannose 处理的 OT-I 肿瘤浸润 T 细胞，同时包含转录组与抗体标签信息。它适合复现细胞状态比例、TCF1⁺/耗竭样群体和表面蛋白变化，但只有两个处理级样本；无论包含多少细胞，都不能由此得到可靠的组间生物重复方差。

### 3.2 bulk RNA-seq

PRJNA1078750 的数据页显示 6 个运行，即每组 3 个重复。论文 Data availability 中出现 `SRR27027798-28027803`，首个编号的 `270` 是排版/录入错误；数据库实际为 `SRR28027798–SRR28027803`。这组数据比 CITE-seq 更适合处理级差异表达和 GSEA。

### 3.3 CUT&RUN

PRJNA1078771 实际包含 10 个运行：对照 IgG 1 个、对照 H3K4me3 2 个、对照 H3K27me3 2 个、D-mannose IgG 1 个、D-mannose H3K4me3 2 个、D-mannose H3K27me3 2 个。对应 SRR28028215–SRR28028224。论文正文只写 `SRR28028215–SRR28028222`，会漏掉 D-mannose H3K27me3 的两个运行；下载时应以 BioProject 数据页为准。

## 4. 数据下载方式

### 4.1 先获取元数据

分别打开三个 BioProject，在 SRA Run Selector 导出 `RunInfo.csv` 和 accession list。先核对 `BioSample`、处理、抗体和重复，再下载 reads。

### 4.2 SRA Toolkit

```bash
# 示例：bulk RNA-seq
prefetch SRR28027798
fasterq-dump --split-files --threads 8 SRR28027798

# 示例：CITE-seq；文件很大，先设置充足缓存/临时目录
prefetch SRR27620041
fasterq-dump --split-files --threads 16 SRR27620041
```

批量下载时使用 Run Selector 导出的 accession list：

```bash
prefetch --option-file SraAccList.txt
fasterq-dump --split-files --threads 12 SRR28028215
```

CITE-seq 两个 SRA 归档合计约 91 GB，转换后的 FASTQ 往往更大；建议至少准备 250–300 GB 可用空间。若目的只是机制复核，可先下载 6 个 bulk RNA 和 10 个 CUT&RUN 运行，避免直接拉取全部 CITE-seq。

## 5. 主要发现

1. T 细胞功能不良伴随 mannose 代谢降低。
2. D-mannose 限制耗竭分化，增加 TCF1⁺/干性与记忆相关表型。
3. D-mannose 改变代谢和表观遗传景观，并增强体内抗肿瘤效应。
4. OGT 介导的 O-GlcNAcylation 稳定 β-catenin，连接 mannose 代谢与 TCF7/LEF1 程序。

## 6. 机制链条

本文提出的主链是：D-mannose 输入增加 → 蛋白 O-GlcNAcylation 改变 → β-catenin 更稳定 → TCF7/LEF1 干性转录程序增强 → T 细胞较少进入终末耗竭并获得更好的持续性。RNA-seq 和 CUT&RUN 为转录/染色质关联证据，遗传或药理干预用于加强因果解释。

## 7. 推荐图版

- CITE-seq 状态图：展示 D-mannose 改变 TIL 状态组成。
- 代谢与 O-GlcNAc/β-catenin 机制图：最适合综述机制部分。
- CUT&RUN 与 TCF7/LEF1 相关表观遗传图。
- ACT 抗肿瘤曲线：连接机制与治疗价值。

## 8. 创新价值

1. 将单糖代谢与 T 细胞干性/耗竭命运直接连接。
2. 联合 CITE-seq、bulk RNA-seq、CUT&RUN 与功能验证。
3. D-mannose 适合嵌入 ex vivo 扩增流程，转化路径相对直接。

## 9. 局限性

1. CITE-seq 只有处理级两个运行，组间统计必须谨慎。
2. 多数核心证据来自模型系统，临床级产品制造中的剂量、持续时间和安全窗口仍需验证。
3. D-mannose 的作用可能依赖培养基、葡萄糖和细胞激活状态。
4. 无公开分析代码，完整复现细胞注释和图形需要自行重建流程。
5. 数据 accession 在正文中存在两处错误/不完整，若不核对数据页会漏下载或下载失败。

## 10. 对综述的作用

该文适合“代谢干预保持 T 细胞干性”和“扩增工艺重编程”部分。它说明代谢添加物并非仅提升即时细胞毒性，也可通过蛋白修饰和染色质状态改变长期分化轨迹。

## 11. 可直接用于综述的观点

> D-mannose 通过 OGT 介导的 O-GlcNAc 修饰稳定 β-catenin，并维持 TCF7/LEF1 相关干性和表观遗传程序，从而限制 T 细胞耗竭分化并提高过继治疗效力（Cancer Cell 2025, Qiu）。

## 12. 数据复用建议

- 做差异表达：优先 PRJNA1078750 的 3 vs 3 bulk RNA-seq。
- 做细胞状态：下载两个 CITE-seq 运行，但把结果定位为描述性比较。
- 做表观遗传机制：下载 PRJNA1078771 全部 10 个运行，不要按正文截止到 SRR28028222。
- 建议保存 RunInfo.csv 和原始 BioProject 页面快照，形成可审计的样本表。

## 13. 避免误读

- 正确的 bulk RNA-seq 起始号是 SRR28027798，不是 SRR27027798。
- CUT&RUN 数据页到 SRR28028224；只下载到 222 会漏掉两个样本。
- 两个 CITE-seq 运行不等于具有大量生物重复。
- 不要把 D-mannose 的体外扩增效果直接等同于临床疗效或长期安全性。
