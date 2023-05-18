# Using GENESIS for Genetic Analysis

## Performing genetic analysis in the R environment

### Part I - Estimating the population structure

This part is using the GENESIS to handle the genetic data

```R
library(SNPRelate)
library(GWASTools)
library(GENESIS)
library(ggplot2)
library(qqman)
```

The approach is considered best practice for performing GWAS in samples of mixed ancestry with a high degree of relatives and is advocated for by the PAGE consortium.

The code essentially lifts from the two tutorials in the package documentation(http://bioconductor.org/packages/release/bioc/vignettes/GENESIS/inst/doc/pcair.html and http://bioconductor.org/packages/devel/bioc/vignettes/GENESIS/inst/doc/assoc\_test.html) The three main contributions this framework has over other approaches is:

1. PC-AIR: computing genetic PCs in sample of individuals with relatedness.
2. PC-Relate: compute genetic relateness in sample of mixed ancestries/population structure.
3. GENESIS: perform GWAS association allowing for hetergeneous variance in each ancestry group.

```R
# Genetics (plink format)
fn_prefix='UKB'
phenotype_csv=''
outfile='/path/to/out/assoc_stats.tsv'

```

```R
# Model specification - @@ TODO - modify accordingly
pc_cols = paste0('PC', seq(1, 10))
covars = c(pc_cols, 'high.educ', 'household.income', 'age', 'sex', 'abcd_site')
DV = 'nihtbx_cryst_uncorrected'
groupVar = 'race_ethnicity' # may want to substitute in categorical variable based on genetic ancestry

```

```R
###################### Create GDS file if one does not exist #################
gds.fn=paste0(fn_prefix, '.gds')
# If gds file does not exist create it
if (!file.exists(gds.fn)){
    snpgdsBED2GDS(bed.fn = paste0(fn_prefix, '.bed'),
    bim.fn = paste0(fn_prefix, '.bim'),
    fam.fn = paste0(fn_prefix, '.fam'),
    out.gdsfn = gds.fn)
}

```

```R
###################### Prunned SNPs and Kinship Matrix #################
pruned_file = paste0(fn_prefix, '_prunned.snps')
kinship_file = paste0(fn_prefix, '_kinship.RData')

if (!file.exists(pruned_file) | !file.exists(kinship_file)){
    gds <- snpgdsOpen(gds.fn)
    ## Prunned set of SNPs
    if (!file.exists(pruned_file)){
        snpset <- snpgdsLDpruning(gds, method="corr", slide.max.bp=10e6,
                          ld.threshold=sqrt(0.1), verbose=FALSE)
        pruned <- unlist(snpset, use.names=FALSE)
        print(paste0(length(pruned), ' pruned snps identified'))
        write(pruned, pruned_file)
    }else{
        pruned = read.csv(pruned_file, header=F)$V1
    }
    ## Kinship matrix
    if (!file.exists(kinship_file)){
        ibd.robust = snpgdsIBDKING(gds)
        save(ibd.robust, file=kinship_file)
    }else{
        # Load File
        load(kinship_file)
    }
    KINGmat = as(ibd.robust$kinship, 'dsyMatrix')
    row.names(KINGmat) = ibd.robust$sample.id
    colnames(KINGmat) = ibd.robust$sample.id
    snpgdsClose(gds)
}

```

```R
###################### PCA-AiR #################
# read in GDS data
ABCD_geno <- GdsGenotypeReader(filename = gds.fn)
# create a GenotypeData class object
ABCD_genoData <- GenotypeData(ABCD_geno)
ABCD_genoData

mypcair <- pcair(ABCD_genoData, kinobj = KINGmat, divobj = KINGmat,
                 snp.include = pruned)

pcair_file = paste0(fn_prefix, '_pcair.RData')
save(mypcair, file=pcair_file)

# Output from PC-AiR
summary(mypcair)

# plot top 2 PCs
plot(mypcair)
# plot PCs 3 and 4
plot(mypcair, vx = 3, vy = 4)

```

```R
###################### PCA-Relate #################
pcrelate_file = paste0(fn_prefix, '_pcrelate.RData')
if (!file.exists(pcrelate_file)){
    ABCD_genoData <- GenotypeBlockIterator(ABCD_genoData, snpInclude=pruned)
    mypcrelate <- pcrelate(ABCD_genoData, pcs = mypcair$vectors[,1:2],
                        training.set = mypcair$unrels)
    save(mypcrelate, file=pcrelate_file)
}else{
    load(pcrelate_file)
}


```

## Part II - Genetic Associations

```R
###################### Association Code #################
bim = read.table(paste0(fn_prefix, '.bim'),
                col.names=c('chr', 'id', 'v3', 'bp', 'a1', 'a2'))

# read in GDS data
ABCD_geno <- GdsGenotypeReader(filename = paste0(fn_prefix, '.gds'))
# create a GenotypeData class object
ABCD_genoData <- GenotypeData(ABCD_geno)
ABCD_genoData
# construct GRM from pc-relate results
myGRM = pcrelateToMatrix(mypcrelate, thresh = 2^(-11/2), scaleKin = 2)

```

```R
nda = readRDS(nda_file)
nda = nda[nda$event_name=='baseline_year_1_arm_1', ] # Baseline data

# Intersect nda file and genetics
row.names(nda) = nda$src_subject_id
nda = nda[row.names(myGRM), ] # Restrict to individuals with genetics
colnames(nda)[colnames(nda)=='src_subject_id'] = 'scanID' # scanID used to match individuals with genetics

# Append PCs from pcair
pc_cols = paste0('PC', seq(1, 10))
nda[row.names(mypcair$vectors[, 1:10]), pc_cols] = mypcair$vectors[, 1:10]

# Convert nda to scanAnnot object
genoID <- getScanID(ABCD_geno)
scanAnnot <- ScanAnnotationDataFrame(nda[genoID, ])
scanAnnot

# Construct genotype iterator appended with behavior
ABCD_genoData <- GenotypeData(ABCD_geno, scanAnnot = scanAnnot) # This is the object which association is performed on

```

```R
# Null model
# group.var is argument allowing for hetergeneous trait varaince across ethnic groups
nullmod <- fitNullModel(genoIterator, outcome = DV, covars = covars,
                        cov.mat = myGRM, family = gaussian, group.var=groupVar)
assoc <- assocTestSingle(genoIterator, null.model = nullmod)
write.table(assoc, outfile, quote=FALSE, row.names=FALSE)

```
