# 《Cross-tissue immune cell analysis reveals tissue-specific features in humans》精读

## 论文信息

- 作者：Cecilia Domínguez Conde、Chen Xu、Laura B. Jarvis 等
- 期刊：Science
- 年份：2022；376(6594): eabl5197
- DOI：10.1126/science.abl5197
- 原文：[Science](https://www.science.org/doi/10.1126/science.abl5197)
- PubMed：[PMID 35549406](https://pubmed.ncbi.nlm.nih.gov/35549406/)
- 全文：[PMC7612735](https://pmc.ncbi.nlm.nih.gov/articles/PMC7612735/)
- 原始数据：[ArrayExpress/BioStudies E-MTAB-11536](https://www.ebi.ac.uk/biostudies/arrayexpress/studies/E-MTAB-11536)
- 处理后数据与交互图谱：[Tissue Immune Cell Atlas](https://www.tissueimmunecellatlas.org/)
- 代码归档：[Zenodo 10.5281/zenodo.6334988](https://doi.org/10.5281/zenodo.6334988)

## 一句话结论

作者用 12 名器官供者的 16 类组织建立了约 36 万细胞的人体跨组织免疫图谱，并以 329,762 个免疫细胞及配对 TCR/BCR 为主体，说明免疫细胞状态不仅由谱系决定，还受到组织生态位、迁移和局部适应程序的系统塑造。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 供者 | 12 名死亡器官供者 | 不是 12 名健康志愿者；死亡、取材和缺血过程可能影响转录状态 |
| 组织 | 16 类 | 各供者并非都贡献全部组织，组织和供者高度不平衡 |
| 高质量细胞 | 357,211 | 含免疫与非免疫细胞 |
| 免疫细胞 | 329,762 | 论文核心分析对象 |
| 免疫状态 | CellTypist 最终注释 101 类 | 层级化标签，不能直接当作 101 个稳定谱系 |
| 分子层 | scRNA-seq；αβ/γδ TCR；BCR | V(D)J 只覆盖相应建库样本，并非每个细胞都有受体序列 |
| 原始归档 | E-MTAB-11536；410 runs、820 FASTQ | 当前 ENA 归档约 3.66 TiB FASTQ，完整下载成本很高 |
| 处理后对象 | global、T、B、髓系及 TCR h5ad | T 细胞主对象约 2.80 GiB；含 counts 版本约 5.59 GiB |

## 1. 研究要解决的问题

外周血只代表人体免疫系统的一小部分。论文要回答：

1. 相同免疫谱系进入不同组织后会形成哪些共同或组织特异状态；
2. T、B 和髓系细胞的组织分布、克隆扩增与迁移关系如何；
3. 能否建立一个可被后续疾病、治疗和细胞工程研究复用的人体正常免疫参考。

对本综述而言，它提供的是“状态地图底座”：先说明自然人体中 T 细胞状态如何随组织改变，再讨论如何人工导航这些状态。

## 2. 方法框架

### 2.1 取材与单细胞测序

作者从 12 名器官供者采集血液、骨髓、脾、肺、肝、肠道、肺引流淋巴结、肠系膜淋巴结等组织；在可获得时还包括胸腺、骨骼肌和网膜等。不同组织经机械或酶消化后获得细胞悬液，采用 10x Genomics 3′ 或 5′ 建库。

5′ 文库允许与免疫受体测序配对。数据集同时含：

- αβ TCR；
- γδ TCR；
- BCR/免疫球蛋白；
- 基因表达矩阵。

作者还使用单分子 RNA 原位杂交验证若干组织定位结果。这里的空间证据是靶向 smFISH，不是全转录组空间组学。

### 2.2 整合、注释与验证

作者以 CellTypist 和人工检查进行层级化注释，最终形成 101 类免疫状态。分析包括跨组织丰度比较、差异表达、组织适应模块、细胞通讯、TCR/BCR 克隆扩增和跨组织共享，并使用成像验证部分定位。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

这是一套同一研究设计下产生的跨器官单细胞多组学资源，不是把几十项公开研究拼接而成。核心有五层：

1. 基因表达：每个细胞的 UMI counts、标准化表达和低维表示；
2. 细胞注释：细胞大类、精细状态、组织、供者、样本和技术批次；
3. αβ TCR：支持 T 细胞克隆扩增和跨组织共享分析；
4. γδ TCR 与 BCR：支持 γδ T 和 B 细胞受体分析；
5. 组织位置验证：针对少量 marker 的 smFISH 图像和定量，不是可下载的空间表达矩阵。

论文报告 357,211 个通过质控的细胞，其中 329,762 个为免疫细胞。最终 101 类注释覆盖 T/先天淋巴、B/浆细胞、髓系等多个区室。T 细胞分析不能简单用“全部 32.98 万免疫细胞”作为分母，应在下载对象中按细胞大类和 V(D)J 可用性重新统计。

### 3.2 供者、组织、样本与文库层级

| 层级 | 规模 | 含义 |
|---|---:|---|
| 生物供者 | 12 | 器官供者，是独立生物重复的上限 |
| source/sample 记录 | 108 | ArrayExpress 中的样本来源记录；不能等同供者 |
| 组织类型 | 16 | 每名供者的组织组合不同 |
| experiment | 298 | 不同建库实验 |
| sequencing run | 410 | ENA 中的 run 数 |
| FASTQ | 820 | 通常每个 run 含 R1/R2 两个文件 |

按 E-MTAB-11536 的 SDRF 当前记录，文件行对应的文库类型包括 10x 3′ v3、10x 5′ v1/v2、αβ TCR、γδ TCR 和免疫球蛋白文库。108 个 sample records、298 个 experiments、410 个 runs 与 12 名供者回答的是不同问题，引用时不得互换。

论文正文的重点组织包括：

- 循环：外周血；
- 初级/次级淋巴器官：骨髓、胸腺、脾、肺引流淋巴结、肠系膜淋巴结；
- 屏障与实质器官：肺、肝、小肠/结肠；
- 其他可获得组织：骨骼肌、网膜等。

### 3.3 原始数据页面：E-MTAB-11536

[E-MTAB-11536](https://www.ebi.ac.uk/biostudies/arrayexpress/studies/E-MTAB-11536) 是完整原始数据入口。其元数据可追溯供者、器官、建库策略、experiment 和 ENA run。

当前归档统计为：

- 410 个测序 runs；
- 820 个 FASTQ 文件；
- FASTQ 总量约 3,752.6 GiB，即约 3.66 TiB；
- submitted files 总量约 4,389.9 GiB，即约 4.29 TiB。

归档体积会随数据库重打包或镜像变化，所以报告中应记录访问日期。完整 FASTQ 下载适合重跑比对、VDJ calling 或严格审计；仅做状态分析不建议从数 TiB 原始数据开始。

下载流程：

1. 在 BioStudies 页面下载 SDRF/IDF/ADF 元数据；
2. 由 SDRF 的 ENA_RUN 字段取得 run accession；
3. 在 ENA Portal API 或 Browser 生成 FTP/Aspera 下载清单；
4. 用校验和检查每个 FASTQ。

命令示例：

~~~bash
wget -O E-MTAB-11536.sdrf.txt \
  "https://www.ebi.ac.uk/biostudies/files/E-MTAB-11536/E-MTAB-11536.sdrf.txt"
~~~

大规模 FASTQ 应用 ENA manifest + Aspera/Globus，并预留至少 5 TiB 的下载和中间文件空间。

### 3.4 处理后图谱页面与文件

[Tissue Immune Cell Atlas](https://www.tissueimmunecellatlas.org/) 提供可视化和处理后 AnnData。当前可直接访问的关键文件如下：

| 文件 | 当前字节数 | 约合 | 用途 |
|---|---:|---:|---|
| global.h5ad | 5,131,003,648 | 4.78 GiB | 全图谱处理后对象 |
| CountAdded_PIP_global_object_for_cellxgene.h5ad | 10,233,316,828 | 9.53 GiB | 含 counts、面向 Cellxgene 的全图谱对象 |
| t-cells.h5ad | 3,009,830,730 | 2.80 GiB | T/先天淋巴细胞子集，T 细胞研究首选 |
| CountAdded_PIP_T_object_for_cellxgene.h5ad | 6,000,594,152 | 5.59 GiB | 含 counts 的 T 细胞对象 |
| adata_TILC_TCR_onlyseq.h5ad | 2,970,695,988 | 2.77 GiB | αβ TCR 相关 T/先天淋巴对象 |
| adata_TILC_TCRgd_onlyseq.h5ad | 495,152,536 | 0.46 GiB | γδ TCR 对象 |

文件服务器基础路径为：

https://cellgeni.cog.sanger.ac.uk/pan-immune/

示例：

~~~bash
wget -c \
  https://cellgeni.cog.sanger.ac.uk/pan-immune/t-cells.h5ad
~~~

门户还提供 B 细胞和髓系子集。文件名、字段和对象体积可能随门户更新，正式分析前应保存 URL、下载日期、字节数和哈希。

### 3.5 代码包

[Zenodo 6334988](https://doi.org/10.5281/zenodo.6334988) 当前包含一个 TissueImmuneCellAtlas-v1.1.zip，57,749,438 bytes，约 55.1 MB。它是分析代码归档，不是表达矩阵。

建议把三类资源分开管理：

- E-MTAB-11536：原始 FASTQ 和实验元数据；
- 图谱门户：处理后 h5ad；
- Zenodo：代码快照。

### 3.6 下载后的最低核验

~~~python
import scanpy as sc

adata = sc.read_h5ad("t-cells.h5ad", backed="r")
print(adata)
print(adata.obs.columns.tolist())
print(list(adata.obsm.keys()))
print(list(adata.uns.keys())[:20])
~~~

至少核验：

- 实际细胞数和基因数；
- counts 位于 X 还是 layers；
- donor、tissue、sample、细胞标签字段；
- 哪些细胞具有 αβ 或 γδ TCR；
- 同一 clonotype 的定义是否包含供者 ID。

## 4. 与 T 细胞状态有关的核心发现

1. 相同 T 细胞大类在不同组织中具有稳定的组织适应表达程序。
2. 淋巴结、脾、骨髓、肺和肠道的 T 细胞组成显著不同，外周血不能替代组织参照。
3. 克隆共享把循环、淋巴和组织状态联系起来，但共享本身不提供迁移方向。
4. 组织驻留、效应、细胞毒和调节程序同时受到谱系与局部生态位影响。

## 5. TCR 数据应如何解释

TCR 的价值是将“表达相似”提升为“克隆相关”。可用于：

- 计算组织内扩增；
- 比较同一供者不同组织间 clonotype 共享；
- 检查同一克隆跨状态分布；
- 识别 αβ 与 γδ T 细胞的受体使用特征。

但必须避免三种误读：

- 同一 TCR 不等于已知抗原特异性；
- 跨组织共享不等于已经观察到迁移方向；
- 缺少 VDJ 的细胞不能被当作未扩增克隆。

## 6. 关键图表怎么读

- 全图谱 UMAP：看谱系和状态范围，不用于证明连续发育轨迹。
- 组织丰度图：需以供者为统计单位，不能把每个细胞当独立重复。
- 组织差异表达：同时可能包含取材、消化和缺血效应。
- TCR 网络/共享图：支持克隆连接，不直接支持抗原或迁移因果。
- smFISH：增强组织定位可信度，但只验证选定 marker。

## 7. 创新点

1. 在同一器官供者框架下比较多组织，降低了跨队列整合带来的混杂。
2. 将 GEX 与 αβ/γδ TCR、BCR 结合。
3. 提供全局与谱系子集对象，便于从浏览到重分析的不同使用方式。
4. 为疾病、肿瘤和细胞治疗队列提供组织正常基线。

## 8. 局限性

1. 仅 12 名供者，人口学和临床背景覆盖有限。
2. 各供者组织缺失不均，组织效应与供者效应不能完全分离。
3. 死亡、器官获取、缺血和组织消化会改变即时转录状态。
4. 单时点数据不能直接证明迁移或状态转化方向。
5. smFISH 不是无偏空间组学。
6. 受体序列不等于抗原验证。

## 9. 对本综述的作用

本论文最适合作为“charting T cell molecular landscape by single-cell and spatial omics”部分的基础资源，并支撑两个观点：

- T 细胞状态导航必须考虑目标组织，因为同一工程细胞进入不同组织后会面对不同稳态和适应压力；
- 评价细胞治疗产品不能只在血液中看表型，应将其映射到跨组织正常参考。

## 10. 可直接写进综述的表述

> 由 12 名器官供者、16 类组织和约 33 万个免疫细胞构成的跨组织图谱显示，T 细胞状态由谱系程序与组织生态位共同决定；配对免疫受体进一步揭示了循环与组织状态之间的克隆联系，为细胞治疗产品的组织适应性评价提供了正常参照。

## 11. 最容易误读的地方

- 357,211 是全部高质量细胞，329,762 才是免疫细胞数。
- 108 个数据库 sample records 不是 108 名供者。
- 101 个标签是层级化注释，不是 101 个不可变细胞类型。
- 论文有原位验证，但不是全转录组空间测序。
- TCR 共享证明克隆联系，不证明抗原、迁移方向或状态因果。
