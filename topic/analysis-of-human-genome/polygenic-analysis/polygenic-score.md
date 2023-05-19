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
