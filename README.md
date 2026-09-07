# SpartinaData

Transcriptome / gene-expression count matrices and functional gene-set collections for smooth cordgrass (*Spartina alterniflora*).

This repository contains two types of data:

1. **Gene-expression count matrices**: RNA-seq raw read counts across multiple tissues/organs, sampling locations, and heat-stress (HT) versus control (CK) conditions.
2. **Functional annotation gene sets**: GO (Gene Ontology) and KEGG pathway gene sets for enrichment analysis, together with the literature-source annotation tables from which they were compiled.

---

## 1. Species and Reference Genome

- **Species**: smooth cordgrass *Spartina alterniflora*
- **Chromosome number**: 31 chromosomes (2n = 62)
- **Reference genome**: Genome Warehouse / CNCB-NGDC assembly, accession `GWHCBIM00000000` (individual chromosomes/scaffolds are numbered `GWHCBIM00000001`, `GWHCBIM00000002`, ...)
- **Gene naming**:
  - `Chr01G000000` – `Chr31G000000`: genes on the 31 chromosomes
  - `Chr0G000000`: genes not assigned to a specific chromosome (unplaced)
  - `novel.xxx`: novel genes derived from transcriptome assembly that are absent from the reference annotation

> Note: `GWHCBIM00000000` is the *Spartina alterniflora* reference genome assembly deposited in the Genome Warehouse. For a formal citation, please refer to the exact assembly version and the corresponding publication you use.

---

## 2. Experimental Design

The data are RNA-seq gene-expression quantifications from a heat-stress experiment with three factors:

- **Location**: `TJ`, `YQ`, `LZ` (three sampling sites)
- **Tissue / Organ**: flower, leaf, root, stem, and several other tissues/organs (see the abbreviation table below)
- **Condition**:
  - `CK`: control
  - `HT`: heat / high-temperature stress

Sample-column naming convention:

```text
{Location}_{Tissue}_{Condition}[_{Replicate}]
```

For example, `TJ_FLR_CK` means site TJ, tissue FLR, control; `YQ_FLR_HT` means site YQ, tissue FLR, heat stress. Some samples carry a `_1` suffix indicating biological replicate 1.

---

## 3. File Inventory

| File | Type | Content |
|------|------|---------|
| 15 files named `*_gene_count.xls` | Tab-separated text | Gene-expression count matrix for each tissue |
| `GO_collection_20260312_GY_V3.xlsx` | Excel | 603 curated GO terms with literature sources |
| `KEGG_collection_20260312_GY_V3.xlsx` | Excel | 987 curated KEGG pathways with literature sources |
| `GO_gene_sets.gmt` | GMT | 762 GO gene sets (gene lists) |
| `KEGG_gene_sets.gmt` | GMT | 141 KEGG gene sets (gene lists) |

The 15 gene-count files (the abbreviation in each filename is the tissue/organ code):

| File | Example sample columns | Gene count |
|------|------------------------|------------|
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

> `mixed_gene_count.xls` contains mixed/pooled samples. Its sample columns carry no location prefix, with three biological replicates each for CK and HT. This file additionally includes 4,404 `novel.xxx` genes.

---

## 4. Gene-Count File Format (`*_gene_count.xls`)

> Despite the `.xls` extension, these files are actually **tab-separated plain-text** files and can be read directly with `read.delim()`, `pandas.read_csv(sep="\t")`, etc.

One gene per row, 16 columns in total:

| Column | Name | Description |
|--------|------|-------------|
| 1 | `gene_id` | Gene identifier, e.g. `Chr03G019800`, `novel.703` |
| 2–7 | sample count columns | Raw read counts (integers) per sample; see the naming convention |
| 8 | `gene_name` | Gene name (currently identical to `gene_id`) |
| 9 | `gene_chr` | Assembly sequence ID, e.g. `GWHCBIM00000003` |
| 10 | `gene_start` | Gene start position (1-based) |
| 11 | `gene_end` | Gene end position (1-based) |
| 12 | `gene_strand` | Strand, `+` or `-` |
| 13 | `gene_length` | Gene length (bp) |
| 14 | `gene_biotype` | Gene biotype, e.g. `protein_coding` |
| 15 | `gene_description` | Functional annotation (UniProt/Swiss-Prot and Pfam, joined by `&&`; `-` means no annotation) |
| 16 | `Family` | Transcription-factor (TF) family, e.g. `bHLH`, `WRKY`, `NAC`; `-` means not classified as a TF |

Typical `gene_description` format:

```text
- && sp|P09189|HSP7C_PETHY Heat shock cognate 70 kDa protein OS=Petunia hybrida ... && PF00012:Hsp70 protein
```

The three segments correspond to: self/no annotation, Swiss-Prot homologous protein, and Pfam domain, joined by `&&`.

The most abundant transcription-factor families (by gene count) include `bHLH`, `LBD`, `MYB_related`, `NAC`, `C2H2`, `ERF`, `WRKY`, `FAR1`, `bZIP`, `C3H`, `MYB`, `TCP`, `B3`, `G2-like`, `M-type_MADS`, `GRAS`, `Trihelix`, `HD-ZIP`, `ARF`, `HSF`, `GATA`, and others.

---

## 5. Tissue / Organ Abbreviations

Abbreviations that can be mapped unambiguously:

| Code | Meaning |
|------|---------|
| `F` | Flower |
| `L` | Leaf |
| `R` | Root |
| `S` | Stem |
| `mixed` | Mixed / pooled samples |

The remaining compound codes (`FL`, `FLR`, `FLS`, `FR`, `FS`, `FSR`, `LS`, `LR`, `LSR`, `SR`) represent different organs or developmental stages. The `F*` series are likely flower/fruit/inflorescence-related, the `L*` series leaf-related (e.g. `LS` may be leaf sheath), and `SR` may be stem/root-related. **The exact definitions of these abbreviations should be confirmed against the original experimental records.**

---

## 6. GO / KEGG Annotation Collections (`.xlsx`)

These two files are GO-term / KEGG-pathway collections compiled from multi-species literature, intended as backgrounds for functional annotation or enrichment analysis.

### 6.1 `GO_collection_20260312_GY_V3.xlsx`

- 603 rows × 7 columns
- Columns: `No.`, `biological processes`, `GO`, `species`, `PUBMED id`, `species_latin`, `species_chinese`
- `GO` is the GO term ID (e.g. `GO:0005198`), `species_latin` / `species_chinese` are the Latin / Chinese names of the source species, and `PUBMED id` is the PMID of the source publication.

### 6.2 `KEGG_collection_20260312_GY_V3.xlsx`

- 987 rows × 6 columns
- Columns: `No.`, `KEGG_id`, `KEGG_term_standard`, `species_latin`, `species_chinese`, `PUBMED id`
- `KEGG_id` looks like `OSA00030` (the `OSA` prefix denotes *Oryza sativa* pathway IDs), and `KEGG_term_standard` is the pathway name.

The source species span roughly 26–29 plant species, including *Glycine max* (soybean), *Vitis vinifera* (grape), *Camellia sinensis* (tea), *Solanum tuberosum* (potato), *Zea mays* (maize), *Brassica rapa* (field mustard), and others.

---

## 7. GO / KEGG Gene Sets (`.gmt`)

GMT (Gene Matrix Transposed) format, one gene set per line:

```text
<gene set name>  <description/id>  <gene1>  <gene2>  <gene3> ...
```

- `GO_gene_sets.gmt`: 762 GO gene sets. The first column is the GO term name (e.g. `LIGASE_ACTIVITY`), the second column is the GO ID (e.g. `GO:0016874`), and the following columns are the gene IDs belonging to that term (e.g. `Chr01G003960`).
- `KEGG_gene_sets.gmt`: 141 KEGG pathway gene sets. The first column is the pathway name (e.g. `LYSINE_BIOSYNTHESIS`), the second column is the pathway ID (e.g. `osa00300`), and the following columns are the gene IDs (including `Chr` genes and `novel` genes).

---

## 8. Usage Examples

### Read a count matrix (R)

```r
counts <- read.delim("leaf_gene_count.xls", header = TRUE,
                     row.names = 1, check.names = FALSE)
head(counts[, 1:6])   # the first 6 columns are sample counts
```

### Read a count matrix (Python)

```python
import pandas as pd

df = pd.read_csv("leaf_gene_count.xls", sep="\t")
counts = df.iloc[:, 1:7]      # first 6 sample columns
annot  = df.iloc[:, 7:]       # gene_name and the annotation columns after it
```

### Enrichment analysis

The `.gmt` files can be used directly with clusterProfiler, gseapy, and similar tools:

```r
library(clusterProfiler)
gs <- read.gmt("GO_gene_sets.gmt")
enrich_result <- enricher(gene = my_gene_list, TERM2GENE = gs)
```

---

## 9. Notes

- The count columns are **raw read counts** (not FPKM/TPM normalized); use DESeq2, edgeR, or similar tools for normalization before differential-expression analysis.
- The column order differs slightly in a few files (e.g. `SR_gene_count.xls`); always select samples by column name rather than position.
- `mixed_gene_count.xls` contains 4,404 extra `novel.xxx` genes compared with the other files; keep this in mind when merging gene sets.

---

## 10. Data Sources and Citation

- Reference genome: *Spartina alterniflora* genome assembly `GWHCBIM00000000` (Genome Warehouse, CNCB-NGDC)
- The `PUBMED id` columns in the GO/KEGG collections record the source publications for each term/pathway; cite the corresponding literature when using them.
- This repository is intended for data sharing and organization only; please add formal citations for the specific data and publications you use before publishing.
