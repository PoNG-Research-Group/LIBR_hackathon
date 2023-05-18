# Mendelian Randomization

Mendelian Randomization (MR) is a causal inference method to mimic randomized control trial in the observational study by using genetic variants as the instrumental variables. The logic is like this:&#x20;

1. In the observational study, the exposure status is oftentimes confounded with other outcome contributing factors.&#x20;
2. Randomization is a way to reduce the confounds since the exposure status is randomly assigned.
3. To mimic the randomization in observational study, we need an instrumental variable that is causally associate with the variations in the exposure but randomly assigned in human population.
4. Genetic variants can be both a). randomly inherited in the human population (Mendelian principles) and b). causally related to the exposure of interest (e.g. influencing the metabolic pathways), it can be served as the instrumental variable.&#x20;
5. Therefore, MR is to use sets of causal variants to represent the exposure for the association test instead of using actually measured exposures, even though the goal is still to determine the relationship between exposures and outcomes.&#x20;

### Assumptions for MR

To have a valid MR inference, three assumptions need to be met:

1. The genetic variants associate with the risk factor of interest
2. There are no unmeasured confounders of the associations between genetic variants and outcome
3. The genetic variants affect the outcome only through their effect on the risk factor of interest

Among those three assumptions, 3 is the most frequently violated one with the typical MR analysis because of ample [**pleiotropy**](https://www.nature.com/scitable/topicpage/pleiotropy-one-gene-can-affect-multiple-traits-569/) in human genome. There are some methods trying to resolve the pleiotropy issue, such as median based methods and Egger regressions. Yet, it should always be taken with a grain of salt. See more detail discussion here: ([https://www.bmj.com/content/362/bmj.k601](https://www.bmj.com/content/362/bmj.k601))

### Running MR

Typically, the MR analysis involves in two stages. First, identify the genetic variants that associate with the exposure of interest through the combination of genome-wide associations and biological evidence. Obtaining their association weights from the GWAS. Second, see if those genetic variants associate with the outcome of interest the same way as the associations with exposure status. If the association weights with the exposure and the association weights with the outcome are coming from two independent cohorts, the weighted sum of two weights indicate the strength of the consistency, hence the metrics for determining the potential causal relationship between exposure and outcome through MR.&#x20;

Sounds like polygenic score or candidate gene analysis, right?&#x20;

### R package for performing MR

I recommend to use the R package maintained by the original developer of the genetic MR methods:

{% embed url="https://cran.r-project.org/web/packages/MendelianRandomization/index.html" %}

MR is frequently used in the context of post-GWAS analysis and can rely on only summary statistic, therefore, we won't showcase the application in UKB here. The key is to make sure the allele coding and the genome built used are consistent between the weights for the exposure and the weights fro the outcome. The developer of MR package has a thorough analytic examples available here:&#x20;

{% embed url="https://cran.r-project.org/web/packages/MendelianRandomization/vignettes/Vignette_MR.pdf" %}
