# Processing Genotype Data

### **Understanding Genetic Variants**

&#x20;Genetics is the study of genes, their functions, and their effects. Within this domain, the term "genetic variant" is used to refer to a difference in the DNA sequence among individuals. This difference could be as small as a single base change (known as a single nucleotide polymorphism, or SNP) or as large as entire sections of a chromosome being duplicated or deleted. Genetic variants are a part of natural diversity, and while most are benign, some can contribute to traits like eye color, height, or even susceptibility to certain diseases.

### **Platforms for Obtaining Genotyping Calls**

&#x20;Genotyping is the process of determining the genetic variants that an individual possesses. There are several platforms available for obtaining genotyping calls, including but not limited to, microarray-based platforms like Illumina and Affymetrix, and sequencing-based platforms like Whole Genome Sequencing (WGS) and Whole Exome Sequencing (WES). Each platform has its own strengths and limitations. For example, microarrays are cost-effective and allow for high-throughput analysis, but they're limited to detecting known variants. In contrast, sequencing can detect novel variants, but it's more expensive and requires more computational resources.

### **Curating Genotyping Data**

Once you have the genotyping calls, the data needs to be curated before it can be analyzed. This typically involves several steps. First, the raw genotyping data needs to be quality controlled (QC). This includes checking for sample contamination, removing poor quality samples or genetic variants, and assessing the data for genotyping errors. Then, the data is usually "cleaned" by removing variants that are rare or not in Hardy-Weinberg equilibrium (HWE, a principle that describes the genetic variation in a population at equilibrium). Finally, the cleaned data may need to be [phased](https://www.nature.com/articles/nrg3054) (ordering the variants along the chromosomes) and [imputed](https://en.wikipedia.org/wiki/Imputation\_\(genetics\)) (using statistical methods to fill in missing genotypes) to increase the completeness and utility of the data for subsequent analyses.

Usually, curating the data requires several specialized softwares and converting different file formats. We won't go into details about the design features, pros/cons, and typical use case about each software here. Nonetheless, it is good to have some basic idea about what they are. Here are three commonly used software suite to handle the genetic data:

PLINK 2.0\
[https://www.cog-genomics.org/plink/2.0/](https://www.cog-genomics.org/plink/2.0/)\
\
BGEN\
[https://www.well.ox.ac.uk/\~gav/bgen\_format/software.html](https://www.well.ox.ac.uk/\~gav/bgen\_format/software.html)\
\
VCFtools\
[https://vcftools.github.io/index.html](https://vcftools.github.io/index.html)

### LIBR Hackathon Dataset

In this hackathon event, we focus on the imputed genotype data from UKBiobank. Fortunately, we don't need to process the data ourselves as the UKBiobank research team already provides well curated imputed genotype data. We further cleaned up the data (Filtering minor allele frequencies and HWE) and converted them into easier to access format (PLINK), to ensure the genetic data is ready to be analyzed. To access the data, see [ukbiobank-data-exploration-gui.md](../data-exploration/ukbiobank-data-exploration-gui.md "mention")

### References

An overview on GWAS\
[https://www.nature.com/articles/s43586-021-00056-9](https://www.nature.com/articles/s43586-021-00056-9)\


