# SpartinaData

互花米草（*Spartina alterniflora*，smooth cordgrass）转录组 / 基因表达计数与功能注释基因集数据集。

本仓库包含两部分内容：

1. **基因表达计数矩阵**：不同组织/器官、不同地点、高温胁迫（HT）与对照（CK）条件下的 RNA-seq 基因原始计数（read counts）。
2. **功能注释基因集**：用于富集分析的 GO（Gene Ontology）和 KEGG 通路基因集合，以及对应的文献来源注释表。

---

## 1. 物种与参考基因组

- **物种**：互花米草 *Spartina alterniflora*
- **染色体数**：31 条染色体（2n = 62）
- **参考基因组**：Genome Warehouse / CNCB-NGDC 组装，序列号为 `GWHCBIM00000000`（各条染色体 / scaffold 依次编号为 `GWHCBIM00000001`、`GWHCBIM00000002` ……）
- **基因命名**：
  - `Chr01G000000` ～ `Chr31G000000`：位于 31 条染色体上的基因
  - `Chr0G000000`：未能定位到具体染色体的基因（unplaced）
  - `novel.xxx`：参考基因组注释之外、由转录组拼接得到的新基因

> 注：`GWHCBIM00000000` 是中国科学院等团队发表的互花米草参考基因组组装（Genome Warehouse 登录号）。若需要严格的引用信息，请以你使用的组装版本和发表文献为准。

---

## 2. 实验设计

数据为一个高温胁迫实验的 RNA-seq 基因表达定量结果，设计包含三个因子：

- **地点（Location）**：`TJ`、`YQ`、`LZ`（三个采样地点）
- **组织 / 器官（Tissue/Organ）**：花、叶、根、茎等多个组织/器官（见下方缩写表）
- **处理（Condition）**：
  - `CK`：对照（control）
  - `HT`：高温胁迫（heat / high-temperature treatment）

样本列命名规则：

```text
{地点}_{组织}_{处理}[_{重复}]
```

例如 `TJ_FLR_CK` 表示 TJ 地点、FLR 组织、对照处理；`YQ_FLR_HT` 表示 YQ 地点、FLR 组织、高温处理。部分样本带 `_1` 后缀，表示第 1 个生物学重复。

---

## 3. 文件清单

| 文件 | 类型 | 内容 |
|------|------|------|
| `FLR_gene_count.xls` 等 15 个 `*_gene_count.xls` | 制表符分隔文本 | 各组织的基因表达计数矩阵 |
| `GO_collection_20260312_GY_V3.xlsx` | Excel | 603 条 GO 术语及文献来源 |
| `KEGG_collection_20260312_GY_V3.xlsx` | Excel | 987 条 KEGG 通路及文献来源 |
| `GO_gene_sets.gmt` | GMT | 762 个 GO 基因集（基因列表） |
| `KEGG_gene_sets.gmt` | GMT | 141 个 KEGG 基因集（基因列表） |

15 个基因计数文件如下（文件名中的缩写即组织/器官代码）：

| 文件 | 样本列示例 | 基因数 |
|------|-----------|--------|
| `flower_gene_count.xls` | `TJ_F_CK_1 … LZ_F_HT_1` | 73,681 |
| `leaf_gene_count.xls` | `TJ_L_CK_1 … LZ_L_HT_1` | 73,681 |
| `root_gene_count.xls` | `TJ_R_CK_1 … LZ_R_HT_1` | 73,681 |
| `stem_gene_count.xls` | `TJ_S_CK_1 … LZ_S_HT_1` | 73,681 |
| `FL_gene_count.xls` | `TJ_FL_CK_1 … LZ_FL_HT_1` | 73,681 |
| `FLR_gene_count.xls` | `TJ_FLR_CK … LZ_FLR_HT` | 73,681 |
| `FLS_gene_count.xls` | `TJ_FLS_CK … LZ_FLS_HT` | 73,681 |
| `FR_gene_count.xls` | `TJ_FR_CK_1 … LZ_FR_HT_1` | 73,681 |
| `FS_gene_count.xls` | `TJ_FS_CK_1 … LZ_FS_HT_1` | 73,681 |
| `FSR_gene_count.xls` | `TJ_FSR_CK … LZ_FSR_HT` | 73,681 |
| `LS_gene_count.xls` | `TJ_LS_CK … LZ_LS_HT` | 73,681 |
| `LR_gene_count.xls` | `TJ_LR_CK_1 … LZ_LR_HT_1` | 73,681 |
| `LSR_gene_count.xls` | `TJ_LSR_CK … LZ_LSR_HT` | 73,681 |
| `SR_gene_count.xls` | `TJ_SR_CK_1 … LZ_SR_HT_1` | 73,681 |
| `mixed_gene_count.xls` | `CK1 CK2 CK3 HT1 HT2 HT3` | 78,085 |

> `mixed_gene_count.xls` 为混合/混池样本，样本列没有地点前缀，CK 与 HT 各 3 个生物学重复；该文件额外包含 4,404 个 `novel.xxx` 新基因。

---

## 4. 基因计数文件格式（`*_gene_count.xls`）

> 尽管扩展名是 `.xls`，这些文件实际上是**制表符（Tab）分隔的纯文本文件**，可直接用 `read.delim()`、`pandas.read_csv(sep="\t")` 等读取。

每行一个基因，共 16 列：

| 列 | 名称 | 说明 |
|----|------|------|
| 1 | `gene_id` | 基因编号，如 `Chr03G019800`、`novel.703` |
| 2–7 | 样本计数列 | 各样本的原始 read count（整数），列名见样本命名规则 |
| 8 | `gene_name` | 基因名称（当前与 `gene_id` 相同） |
| 9 | `gene_chr` | 所在序列的组装编号，如 `GWHCBIM00000003` |
| 10 | `gene_start` | 基因起始位置（1-based） |
| 11 | `gene_end` | 基因终止位置（1-based） |
| 12 | `gene_strand` | 链方向，`+` 或 `-` |
| 13 | `gene_length` | 基因长度（bp） |
| 14 | `gene_biotype` | 基因类型，如 `protein_coding` |
| 15 | `gene_description` | 功能注释（UniProt/Swiss-Prot 与 Pfam，用 `&&` 分隔；`-` 表示无注释） |
| 16 | `Family` | 转录因子（TF）家族，如 `bHLH`、`WRKY`、`NAC`；`-` 表示未归类为转录因子 |

`gene_description` 的典型格式：

```text
- && sp|P09189|HSP7C_PETHY Heat shock cognate 70 kDa protein OS=Petunia hybrida ... && PF00012:Hsp70 protein
```

三段分别对应：无/自注释、Swiss-Prot 同源蛋白、Pfam 结构域，中间用 `&&` 连接。

主要转录因子家族（按数量排序）包括：`bHLH`、`LBD`、`MYB_related`、`NAC`、`C2H2`、`ERF`、`WRKY`、`FAR1`、`bZIP`、`C3H`、`MYB`、`TCP`、`B3`、`G2-like`、`M-type_MADS`、`GRAS`、`Trihelix`、`HD-ZIP`、`ARF`、`HSF`、`GATA` 等。

---

## 5. 组织 / 器官缩写

可以明确对应的缩写：

| 缩写 | 含义 |
|------|------|
| `F` | 花（flower） |
| `L` | 叶（leaf） |
| `R` | 根（root） |
| `S` | 茎（stem） |
| `mixed` | 混合/混池样本 |

其余组合缩写（`FL`、`FLR`、`FLS`、`FR`、`FS`、`FSR`、`LS`、`LR`、`LSR`、`SR`）表示不同组织/器官或发育时期，推测 `F*` 系列多为花/果/花序相关，`L*` 系列多为叶相关（如 `LS` 可能为叶鞘 leaf sheath），`SR` 可能为茎/根相关。**这些缩写的确切定义请以原始实验记录为准。**

---

## 6. GO / KEGG 注释集合（`.xlsx`）

这两个文件是从多物种文献中整理得到的 GO 术语 / KEGG 通路集合，用于功能注释或富集分析的背景。

### 6.1 `GO_collection_20260312_GY_V3.xlsx`

- 共 603 行、7 列
- 列：`No.`、`biological processes`、`GO`、`species`、`PUBMED id`、`species_latin`、`species_chinese`
- `GO` 列为 GO 术语编号（如 `GO:0005198`），`species_latin` / `species_chinese` 为该术语来源物种的拉丁名 / 中文名，`PUBMED id` 为来源文献 PMID

### 6.2 `KEGG_collection_20260312_GY_V3.xlsx`

- 共 987 行、6 列
- 列：`No.`、`KEGG_id`、`KEGG_term_standard`、`species_latin`、`species_chinese`、`PUBMED id`
- `KEGG_id` 形如 `OSA00030`（OSA 前缀表示水稻 *Oryza sativa* 通路编号），`KEGG_term_standard` 为通路名称

来源物种覆盖多种植物（约 26–29 种），例如 *Glycine max*（大豆）、*Vitis vinifera*（葡萄）、*Camellia sinensis*（茶树）、*Solanum tuberosum*（马铃薯）、*Zea mays*（玉米）、*Brassica rapa*（芸薹）等。

---

## 7. GO / KEGG 基因集（`.gmt`）

GMT（Gene Matrix Transposed）格式，每行一个基因集：

```text
<基因集名称>  <描述/编号>  <基因1>  <基因2>  <基因3> ...
```

- `GO_gene_sets.gmt`：762 个 GO 基因集。第一列为 GO 术语名（如 `LIGASE_ACTIVITY`），第二列为 GO 编号（如 `GO:0016874`），后续为属于该术语的基因编号（如 `Chr01G003960`）。
- `KEGG_gene_sets.gmt`：141 个 KEGG 通路基因集。第一列为通路名（如 `LYSINE_BIOSYNTHESIS`），第二列为通路编号（如 `osa00300`），后续为属于该通路的基因编号（含 `Chr` 基因和 `novel` 基因）。

---

## 8. 使用示例

### 读取基因计数矩阵（R）

```r
counts <- read.delim("leaf_gene_count.xls", header = TRUE,
                     row.names = 1, check.names = FALSE)
head(counts[, 1:6])   # 前 6 列为样本计数
```

### 读取基因计数矩阵（Python）

```python
import pandas as pd

df = pd.read_csv("leaf_gene_count.xls", sep="\t")
counts = df.iloc[:, 1:7]      # 前 6 个样本列
annot  = df.iloc[:, 7:]       # gene_name 及之后的注释列
```

### 富集分析

`.gmt` 文件可直接用于 clusterProfiler、gseapy 等工具：

```r
library(clusterProfiler)
gs <- read.gmt("GO_gene_sets.gmt")
enrich_result <- enricher(gene = my_gene_list, TERM2GENE = gs)
```

---

## 9. 说明

- 计数列为**原始 read count**，未做 FPKM/TPM 标准化；差异表达分析前请使用 DESeq2、edgeR 等工具进行归一化。
- 部分样本列顺序在不同文件中略有差异（例如 `SR_gene_count.xls`），读取时请以列名而非位置为准。
- `mixed_gene_count.xls` 比其余文件多出 4,404 个 `novel.xxx` 新基因，合并分析时请注意基因集是否一致。

---

## 10. 数据来源与引用

- 参考基因组：*Spartina alterniflora* 基因组组装 `GWHCBIM00000000`（Genome Warehouse，CNCB-NGDC）
- GO / KEGG 集合中的 `PUBMED id` 列记录了各术语/通路的来源文献，引用时请对应到具体文献。
- 本仓库仅用于数据共享与整理；发表前请根据你使用的具体数据和文献补充正式引用。
