# mafViper

A Python package for processing, analyzing and visualizing Mutation Annotation Format (MAF) files in genomic research.

## Installation

```bash
pip install mafviper
```

## Modules and Functions

### 1. MAF File Reading (`read_maf`)

```python
from mafviper import read_maf

maf_dataframe = read_maf(
    maf_file='path/to/your/file.maf', 
    skip_rows_until='Hugo_Symbol', 
    sep='\t', 
    use_all=True
)
```

#### Parameters:
- `maf_file` (str): Path to the MAF file
- `skip_rows_until` (str, optional): Column name to start reading from. Default is 'Hugo_Symbol'
- `sep` (str, optional): Delimiter used in the MAF file. Default is tab ('\t')
- `use_all` (bool, optional): 
  - If `True` (default): Includes all mutation types
  - If `False`: Only keeps variants with 'Mutation_Status' as 'Somatic'

#### Returns:
- Pandas DataFrame containing validated MAF data

### 2. MAF Processing (`classify_variants`, `merge_mafs`, `get_titv`)

```python
from mafviper import classify_variants, merge_mafs, get_titv

# Classify variants
non_synonymous_maf = classify_variants(
    maf, 
    discard_synonymous=True
)

# Merge multiple MAF files
merged_maf = merge_mafs(
    maf_list=[maf1, maf2, maf3], 
    discard_synonymous=False
)

# Generate Ti/Tv classification
titv_summary = get_titv(maf_df)
```

#### `classify_variants`
- `maf`: Input MAF DataFrame
- `discard_synonymous` (bool, optional):
  - If `True`: Removes synonymous mutations
  - If `False`: Keeps all mutation types

#### `merge_mafs`
- `maf_list`: List of MAF DataFrames to merge
- `discard_synonymous` (bool, optional):
  - If `True`: Removes synonymous mutations after merging
  - If `False`: Keeps all mutation types

#### `get_titv`
- `maf_df`: MAF DataFrame with SNP variants
- Returns a summary DataFrame of Transition/Transversion (Ti/Tv) classifications

### 3. Visualization (`get_maf_summary`)

```python
from mafviper import get_maf_summary

get_maf_summary(
    maf_df,
    output_pdf='MAF_Summary_Dashboard.pdf',
    add_stat=None,
    show_barcodes=False,
    top_genes=10,
    raw_count=True,
    nonsyn_titv=False,
    use_nonsyn=True
)
```

#### Parameters:
- `maf_df`: Input MAF DataFrame
- `output_pdf` (str, optional): Output PDF filename. Default is 'MAF_Summary_Dashboard.pdf'
- `add_stat` (str, optional): Default is median
  - `'mean'`: Add mean mutation count line
  - `'median'`: Add median mutation count line
- `show_barcodes` (bool, optional): Display sample barcodes
- `top_genes` (int, optional): Number of top mutated genes to display. Default is 10
- `raw_count` (bool, optional):
  - If `True`: Display raw mutation counts
  - If `False`: Display mutation proportions
- `nonsyn_titv` (bool, optional):
  - If `True`: Calculate Ti/Tv using non-synonymous mutations
  - If `False`: Calculate Ti/Tv using all mutations
- `use_nonsyn` (bool, optional):
  - If `True`: Use only non-synonymous mutations for summary
  - If `False`: Use all mutations for summary

## Example Workflow

```python
from mafviper import read_maf, merge_mafs, get_maf_summary

# Read MAF files
maf1 = read_maf('sample1.maf')
maf2 = read_maf('sample2.maf')

# Merge MAF files
merged_maf = merge_mafs([maf1, maf2])

# Generate visualization
get_maf_summary(
    merged_maf, 
    output_pdf='my_maf_summary.pdf', 
    top_genes=15, 
    add_stat='mean'
)
```

## Requirements
- pandas
- seaborn
- matplotlib

## License
Apache 2.0

## Author
Joseph Rosen
