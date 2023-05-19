# Polygenic Score

The polygenic score (PGS) is to use the coefficient weights derived from the GWAS, combined with the genotype data you have (independent of the GWAS samples), to form a single value continuous score representing the genetic propensities of a given trait. Currently, it is known to have better predictive power if:\
a. The training GWAS size is large\
b. The predicting trait is genetically driven (high heritability and low sparsity)\
c. The training population matches with the target population

### The purpose of PGS

The PGS serves in multiple purpose despite its relatively weak predictive performance. \
1\. Can be a good causal instrument\
2\. Can serve as imputing missing measurements&#x20;

### Key factor considering PGS

To use PGS, the common practice includes the following important consideration:\
1\. Making sure your testing data has not been used in the GWAS\
2\. Always put genetic PCs as a way to control the population structure\
3\. Consider a way to control for genetic background differences

### Tutorial of PGS in R

The Nature Protocol paper by Choi et al. nicely provide a guide to run PGS in R ([https://www.nature.com/articles/s41596-020-0353-1](https://www.nature.com/articles/s41596-020-0353-1)). Here I put their github page tutorial in the following link:

{% embed url="https://choishingwan.github.io/PRS-Tutorial/" %}

### Useful resources for PGS

[<mark style="color:yellow;">**PGS catalogue**</mark>](../../data-exploration/ukbiobank-data-exploration-gui.md): This is a PGS weight repository that people put their trained weights in there whenever a PGS paper is published. It is very useful since they provide a python function to calculate the scores en mass and user can download individual weights as well. However, always need to make sure the training samples are not coming from UKBiobank since UKBiobank has become the go-to dataset to obtain GWAS results.&#x20;

[<mark style="color:yellow;">**GWAS catalogue**</mark>](https://www.ebi.ac.uk/gwas/): This is a repository of summary statistics of GWAS, which then can be used for generating PGS. The downside comparing to use PGS catalogue directly, is that the GWAS catalogue might need extra processing, such as liftOver the genome build, realign the effect alleles, and calibrating with LDs (more details in [#tutorial-of-pgs-in-r](polygenic-score.md#tutorial-of-pgs-in-r "mention")).&#x20;



