# Genome Wide Associations

The genome wide associations study (GWAS) can be thought as regressing the genetic variant on the outcome of interest, one variant at a time. So, theoretically, any statistical package can do GWAS. However, the issue is about the scale. Take UKBiobank for example, the imputed genotypes has more than 5 million signle nucleotide polymorphisms (SNPs) and 500K participants. Even a simple linear regression which scaled by NxM (500K X 5 million), it will take 2.5e12 times to run, let alone the constrain of memory and input/output cost. Furthermore, because the flexibility of the linear mixed effects (LME) model, it is often to see GWAS using LME to take into account of complex study design and polygenic background. Because LME is scaled by the cubic of the design matrix, it is impossible to run the LME GWAS with some go-to software suite in R, such as _**lme4**_. In this section, we will introduce some frequently used GWAS packages that are tailored for efficiently performing genome wide scans.&#x20;

### Models for GWAS&#x20;

In the GWAS, the dependent variable is the phenotypic outcome of interest. It can be continuous, such as height, or dichotomus, such as clinical diagnosis. The independent variable of interest is the bialleilic genetic variants, coded as the additive dosages, such as aa = 0, Aa = 1, AA = 2. The reason behind such coding scheme is because <mark style="color:yellow;">1). the dependency on the normal assumption</mark>, <mark style="color:yellow;">2). empirical evidence on the human trait inheritance</mark>, and <mark style="color:yellow;">3). the characteristics of linear model</mark> (In the case of small effect sizes, additive coding is capable capturing most of the dominant/recessive effects if not perfectly). For a single variant association, the model looks like:

$$y_i  = a_i  + \beta_j  G_{ij} + \sum_k\gamma_k PC_{ik} + age + sex    + covariates$$

For i subject, given jth genetic variant, controlling for first k genetic principal components (PC) and other covariates such as age and sex. The a term can be the random intercept to code the study design matrix or the polygenic background, such as:

$$a \sim N(0, \delta^2 ZZ^t + \epsilon^2 I)$$

Z is the study design matrix and assuming compound symmetry model with idenpendent errors.&#x20;

### Steps for running GWAS&#x20;

The GWAS analysis usually involves the following step:\


1\) Converting the genotype data into required format while filtering the genetic variants according to minor allele frequencies (usually greater than 0.005) and HWE (usually goodness of fit p < 1e-6). \
\
2\) Calculating <mark style="color:yellow;">**the relatedness matrix**</mark> and deriving <mark style="color:yellow;">**the genetic principal components**</mark> given a set of independent genetic variants (selecting SNPs based on [<mark style="color:yellow;">**Linkage Disequilibrium**</mark>](https://www.nature.com/articles/nrg2361)) \
\
3\) Clean up the phenotype spreadsheet and the covariate spreadsheet\
\
4\) Put all of them together and feed into the software of choose to get the GWAS results \
\
5\) Run post-GWAS softwares on the summary statistics to obtain more interpretable results

### Code Example

See [using-genesis-for-genetic-analysis.md](../../../about-libr-hackathon/study-materials/code-examples/using-genesis-for-genetic-analysis.md "mention")
