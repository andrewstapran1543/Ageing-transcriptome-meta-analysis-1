# Project report

All code for tasks described in this report is avaliable in the [Jupiter Notebook]().

## Introduction

In this project, we reproduce the results of a meta-analysis of gene expression in mice reported by Palmer, Daniel et al. in 2021  [palmer2021ageing](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7906136/#SD2). In this study transcriptome signatures of ageing in brain, heart and muscle were derived from 127 public microarray and RNA-Seq datasets from mice, rats, and humans. Gene expression patterns revealed overexpression of immune and stress response genes with age, as well as underexpression of metabolic and developmental genes. In muscle and heart cell response processes were found to be activated. It was indicated that gene ageing signatures are associated with key genes in protein interaction networks. Previously in thr paper [de2009meta](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2732303/) several common signatures of aging were highlighted including overexpression of inflammation, immune response and lysosomes related genes, underexpression of collagen, energy metabolism and MT genes, as well as alterations in expression of apoptosis genes. The reasoning that genes that display differential expression with age are mainly common for different tissues was confirmed.  

The outline of the original study is the following: 
1. Linear regression was performed for each gene inside each dataset. The slope of the regression corresponds to the level of change in gene expression.
2. Genes statistically significantly associated with age were selected.
3. Meta regression was performed with Binomial test.
4. False discovery rate correction by permutation, the genes by critical P value sorted.
5. Differentially expressed genes (DEGs) were identified in each set of datasets.
6. DEGs were identified in all datasets combined.
7. Enrichment analysis with David and topGO R tool was performed.
8. dN/dS analysis - the authors calculated the ratio of no synonymus to synonymous substitutions in the main DEGs with age in different species. The result of their calculation showed that those genes are predominantly evolutionary conserved across species.
9. Random Forest ML models was build to identify the most important GO terms in the age-related DEGs.
10. Tissue specificity analysis was run with tau index used as a measure of tissue specificity. From the results of this analysis the authors found that the majority of age-associated DEGs are evenly expressed among tissues.

### Machine learning approach used in the paper `palmer2021ageing`

...

### Weak points of the paper

For the analysis of the significantly different expressions across the datasets, the authors used the binomial test. In cases where the dataset number is small, such meta-regression tests are not statistically significant because they don't reflect the effect size. Additionally, two different data types are explored, RNA-seq and microarray DNA, which require correction for the type of the data when they are combined. 

### Suggestions for approach improvements

Another approach for meta-regression can be used, in our project we applied meta-regression using the PyMare package for a higher specificity analysis.

## Results

### Preprocessing of the gene expression data

From the data avaliable on the [github repository](https://github.com/maglab/AgeingSignatures2020_supplementary) we selected 20 mice microarray datasets originated from brain, heart and muscle. (12 (14) samples from brain, 5 from heart and 8 (12) from muscle) Each dataset contains expression levels of around 20000 genes from several samples.

#### Quality control

To explore a similarity of the samples we applied the Principal Component Analysis (PCA) method of data clustering. The samples belonging to different ages showed to be intermingled in the samples, so no samples were removed. 

#### Statistical testing

To estimate differencially expressed genes (DEGs) we conducted **linear regression** analysis for all the datasets. The regression equation:

$$Y_{ij} = \beta_{0j} + \beta_{1j}Age{i} + \epsilon_{ij}$$

The slope of this regression identifies the coefficient of differencial expression. **F test** with 0.05 cutoff was then used to identify the significance of the coefficients and the coefficients variances were calculated. 

...???

### Meta-analysis of the datasets

...???

subsetting_stats_genes - takes in the results of dataset builder and retorns tables with all significantl expressed genes

Firstly the global metaanalysis was conducted to identify DEGs across all tissues. 

Next, the same analysis was conducted separately for each tissue. ???

To identify genes differentially expressed in tissue across all datasets we applied **meta regression** with the corresponding function from the **PyMare** package. This function takes as input the estimated regression coefficients for individual datasets and the variance of these coefficients. It produces weighted assessments of gene expression effects, taking into account the level of uncertainty. In our meta regression function, we passed the coefficient values, variances of coefficients, and tissue parameters and obtained..... ???

### GO enrichment analysis

Gene Onthology enrichment analysis was done with **gseapy** python package to identify functional categories for the obtained DEGs. In the genes overexpressed with age across multiple datasets in the brain, the enrichment terms are the following:

<img
  src="/figs/Enrich_terms_over_brain.jpg"
  alt="Enrichment terms in the brain overexpressed genes"
  title="Enrichment terms in the brain overexpressed genes"
  style="display: inline-block; margin: 0 auto; max-width: 300px">
  
In the muscle overexpressed genes with age, the picture is the following:

<img
  src="/figs/Enrich_terms_over_muscle.jpg"
  alt="Enrichment terms in the muscle overexpressed genes"
  title="Enrichment terms in the muscle overexpressed genes"
  style="display: inline-block; margin: 0 auto; max-width: 300px">
  
For heart overexpressed genes:

<img
  src="/figs/Enrich_terms_over_heart.jpg"
  alt="Enrichment terms in the heart overexpressed genes"
  title="Enrichment terms in the heart overexpressed genes"
  style="display: inline-block; margin: 0 auto; max-width: 300px">
  
For heart underexpressed:

<img
  src="/figs/Enrich_terms_under_heart.jpg"
  alt="Enrichment terms in the heart underexpressed genes"
  title="Enrichment terms in the heart underexpressed genes"
  style="display: inline-block; margin: 0 auto; max-width: 300px">
  
Enriched terms in the genes differentially overexpressed with age across multiple datasets in all three tissues are:

<img
  src="/figs/Enrich_terms_over_allthetissues.jpg"
  alt="Enrichment terms overexpressed genes in all tissues "
  title="Enrichment terms overexpressed genes in all tissues"
  style="display: inline-block; margin: 0 auto; max-width: 300px">
  
While for underexpressed:

<img
  src="/figs/Enrich_terms_under_allthetissues.jpg"
  alt="Enrichment terms underexpressed genes in all tissues "
  title="Enrichment terms underexpressed genes in all tissues"
  style="display: inline-block; margin: 0 auto; max-width: 300px">
  
  

### Search in GenAge database

Intersection of the obtained list of differentially expressed genes across aging with GenAge database. 

...

## Discussion

### The list of differentially expressed genes across aging

According to the results of `palmer2021ageing` study all tissues and every tissue studied consistently overexpressed the following genes:

<img
  src="/figs/Palmer_table_overexpressed.jpg"
  alt="Table of the top-5 genes most consistently overexpressed with age across datasets for all tissues and for each tissue studied. `palmer2021ageing`"
  title="Overexpressed genes"
  style="display: inline-block; margin: 0 auto; max-width: 300px">
  
And the picture of underexpressed genes is the following:

<img
  src="/figs/Palmer_table_underexpressed.jpg"
  alt="Table of the top-5 genes most consistently underexpressed with age across datasets for all tissues and for each tissue studied. `palmer2021ageing`"
  title="Underexpressed genes"
  style="display: inline-block; margin: 0 auto; max-width: 300px">
  
We obtained a list of differentially expressed genes across aging and intersected them with GenAge database. Describtion of some of these genes ... .

...

We further conducted enrichment analyses for genes differentially expressed in each tissue and for all three tissues together. According to its findings, immune system activation genes were overexpressed in the brain. Differentiation-related genes were overexpressed in muscles, resulting in a decrease in regeneration activity and muscle deterioration. An increase in GABA signaling and ion transport was observed in the heart. All tissues collectively exhibit an increase in the activity of immune and proteilitic genes, while metabolic genes decrease. The described results are in line with the conclusions made in the paper `palmer2021ageing`. 


## Credits

GO enrichment analysis and this repository prepared by Daria Kozhevnikova

The Jupiter Notebook for the data meta-analysis prepared by Andrey Stapran

The search in GenAge and description of found matches prepared by ShahZeb Khan

## References

```{bibliography}
Palmer, Daniel et al. “Ageing transcriptome meta-analysis reveals similarities and differences between key mammalian tissues.” Aging vol. 13,3 (2021): 3313-3341. doi:10.18632/aging.202648

de Magalhães, João Pedro et al. “Meta-analysis of age-related gene expression profiles identifies common signatures of aging.” Bioinformatics (Oxford, England) vol. 25,7 (2009): 875-81. doi:10.1093/bioinformatics/btp073
```
Yes


Footer
© 2023 GitHub, Inc.
Footer navigation
Terms
Privacy
Security
Status
Docs
Contact GitHub
Pricing
API
Training
Blog
About
