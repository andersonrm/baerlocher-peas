Winter Pea qRT-PCR
================
Riley M. Anderson & Chase Baerlocher
February 06, 2024

  

- [Overview](#overview)
- [How the
  ![\2^{-\Delta \Delta Ct}](https://latex.codecogs.com/png.latex?%5C2%5E%7B-%5CDelta%20%5CDelta%20Ct%7D "\2^{-\Delta \Delta Ct}")
  method works:](#how-the-2-delta-delta-ct-method-works)
- [A working example](#a-working-example)
  - [Proper reference gene:](#proper-reference-gene)
  - [Now, a bad reference gene:](#now-a-bad-reference-gene)
  - [Did the reference gene perform well as a reference for defense
    genes?](#did-the-reference-gene-perform-well-as-a-reference-for-defense-genes)
  - [Did the reference gene perform well as a reference for
    cPEMV2?](#did-the-reference-gene-perform-well-as-a-reference-for-cpemv2)
- [Exploratory plots:](#exploratory-plots)
  - [AO3](#ao3)
  - [LOX2](#lox2)
  - [PR1](#pr1)
  - [cPEMV2](#cpemv2)
  - [![\2^{-\Delta \Delta Ct}](https://latex.codecogs.com/png.latex?%5C2%5E%7B-%5CDelta%20%5CDelta%20Ct%7D "\2^{-\Delta \Delta Ct}")
    calcs](#2-delta-delta-ct-calcs)
- [Figures](#figures)
  - [AO3](#ao3-1)
  - [LOX2](#lox2-1)
  - [PR1](#pr1-1)
  - [cPEMV2](#cpemv2-1)
- [Session Information](#session-information)

## Overview

This is an analysis of Chase’s qRT-PCR data. Results are displayed as
the fold change difference in gene expression between the *no rhizobia*
(control) condition and the *rhizobia* (treatment) condition.

- Fold changes \> 1 represent upregulation in the *rhizobia* condition
  relative to *no rhizobia*

- Fold changes \< 1 represent downregulation in the *rhizobia* condition
  relative to *no rhizobia*

## How the ![\2^{-\Delta \Delta Ct}](https://latex.codecogs.com/png.latex?%5C2%5E%7B-%5CDelta%20%5CDelta%20Ct%7D "\2^{-\Delta \Delta Ct}") method works:

Expression of the gene of interest is measured and calibrated to a
reference gene.

The
![\Delta\Delta Ct](https://latex.codecogs.com/png.latex?%5CDelta%5CDelta%20Ct "\Delta\Delta Ct")
is the normalized difference in gene expression between a gene of
interest in control and treatment conditions:

![\Delta\Delta Ct = \Delta Ct\_{reference.gene} - \Delta Ct\_{target.gene}](https://latex.codecogs.com/png.latex?%5CDelta%5CDelta%20Ct%20%3D%20%5CDelta%20Ct_%7Breference.gene%7D%20-%20%5CDelta%20Ct_%7Btarget.gene%7D "\Delta\Delta Ct = \Delta Ct_{reference.gene} - \Delta Ct_{target.gene}")

where,

![\Delta Ct\_{reference.gene} = (Ct\_{control} - Ct\_{treatment})\_{ref.gene}](https://latex.codecogs.com/png.latex?%5CDelta%20Ct_%7Breference.gene%7D%20%3D%20%28Ct_%7Bcontrol%7D%20-%20Ct_%7Btreatment%7D%29_%7Bref.gene%7D "\Delta Ct_{reference.gene} = (Ct_{control} - Ct_{treatment})_{ref.gene}")

and

![\Delta Ct\_{target.gene} = (Ct\_{control} - Ct\_{treatment})\_{target.gene}](https://latex.codecogs.com/png.latex?%5CDelta%20Ct_%7Btarget.gene%7D%20%3D%20%28Ct_%7Bcontrol%7D%20-%20Ct_%7Btreatment%7D%29_%7Btarget.gene%7D "\Delta Ct_{target.gene} = (Ct_{control} - Ct_{treatment})_{target.gene}")

Visually, this table represents the data needed:

|                | Control | Treatment |
|:---------------|:--------|:----------|
| Reference gene | A       | B         |
| Target gene    | C       | D         |

Where the above formula becomes:

![\Delta\Delta Ct = (A - B) - (C - D)](https://latex.codecogs.com/png.latex?%5CDelta%5CDelta%20Ct%20%3D%20%28A%20-%20B%29%20-%20%28C%20-%20D%29 "\Delta\Delta Ct = (A - B) - (C - D)")

Alternatively,

![\Delta\Delta Ct = (D - B) - (C - A)](https://latex.codecogs.com/png.latex?%5CDelta%5CDelta%20Ct%20%3D%20%28D%20-%20B%29%20-%20%28C%20-%20A%29 "\Delta\Delta Ct = (D - B) - (C - A)")

Or if you prefer no parentheses:

![\Delta\Delta Ct = A + D - B - C](https://latex.codecogs.com/png.latex?%5CDelta%5CDelta%20Ct%20%3D%20A%20%2B%20D%20-%20B%20-%20C "\Delta\Delta Ct = A + D - B - C")

The above 3 equations are equivalent (this can be useful if your data
are organized in different ways).

To understand the intuition behind this method we first need to
understand what a Ct, or cycle threshold value actually is.

<figure>
<img src="./Yuan2006_1.png" alt="Yuan et al. 2006 Fig. 1" />
<figcaption aria-hidden="true">Yuan et al. 2006 Fig. 1</figcaption>
</figure>

PCR has three phases, an exponential phase, a linear phase, and a
plateau phase. PCR product will essentially double with each cycle in
the *exponential* phase as reagents are not limited. As reagents become
limiting, PCR product will be produced *linearly* with cycle number
until some reagents are fully depleted, at which time the PCR product
will no longer increase with cycle number, defining the *plateau* phase.

Plotting the PCR product with
![log\_{2}](https://latex.codecogs.com/png.latex?log_%7B2%7D "log_{2}")
transformation will yield a range where PCR product correlates with the
original template amount.

<figure>
<img src="./Yuan2006_2.png" alt="Yuan et al. 2006 Fig. 2" />
<figcaption aria-hidden="true">Yuan et al. 2006 Fig. 2</figcaption>
</figure>

The peak of this range is the transition from the exponential phase to
the linear phase which defines the cycle threshold number.

- Comparing two samples, a higher Ct value means lower expression since
  it took more PCR cycles to reach the same threshold value of PCR
  product.

In order for relative quantification of gene expression to be valid, the
chosen reference gene needs to have stable expression across treatment
conditions.

That is
![\Delta Ct\_{reference.gene} \sim 0](https://latex.codecogs.com/png.latex?%5CDelta%20Ct_%7Breference.gene%7D%20%5Csim%200 "\Delta Ct_{reference.gene} \sim 0")
where the expression of the reference gene should only marginally
fluctuate across samples and treatments.

- If
  ![\Delta Ct\_{reference.gene} = 0](https://latex.codecogs.com/png.latex?%5CDelta%20Ct_%7Breference.gene%7D%20%3D%200 "\Delta Ct_{reference.gene} = 0")
  then,

- A positive
  ![\Delta Ct\_{target.gene}](https://latex.codecogs.com/png.latex?%5CDelta%20Ct_%7Btarget.gene%7D "\Delta Ct_{target.gene}")
  will result in upregulation:

![+\Delta Ct\_{target} \to -\Delta\Delta Ct \to 2^{-(-\Delta\Delta Ct)} \to FC\>1](https://latex.codecogs.com/png.latex?%2B%5CDelta%20Ct_%7Btarget%7D%20%5Cto%20-%5CDelta%5CDelta%20Ct%20%5Cto%202%5E%7B-%28-%5CDelta%5CDelta%20Ct%29%7D%20%5Cto%20FC%3E1 "+\Delta Ct_{target} \to -\Delta\Delta Ct \to 2^{-(-\Delta\Delta Ct)} \to FC>1")

alternatively,

- A negative
  ![\Delta Ct\_{target.gene}](https://latex.codecogs.com/png.latex?%5CDelta%20Ct_%7Btarget.gene%7D "\Delta Ct_{target.gene}")
  will result in downregulation:

![-\Delta Ct\_{target} \to +\Delta\Delta Ct \to 2^{-(\Delta\Delta Ct)} \to FC\<1](https://latex.codecogs.com/png.latex?-%5CDelta%20Ct_%7Btarget%7D%20%5Cto%20%2B%5CDelta%5CDelta%20Ct%20%5Cto%202%5E%7B-%28%5CDelta%5CDelta%20Ct%29%7D%20%5Cto%20FC%3C1 "-\Delta Ct_{target} \to +\Delta\Delta Ct \to 2^{-(\Delta\Delta Ct)} \to FC<1")

## A working example

**Downregulation** is defined by a **higher** Ct value in the treatment
condition compared to the control condition. Let’s try the calculations
with ideal data and with data that have a poor reference gene.

### Proper reference gene:

|                | Control | Treatment |
|:---------------|--------:|----------:|
| Reference gene |  26.012 |    26.475 |
| Target gene    |  24.149 |    26.688 |

Our
![\Delta Ct\_{ref}](https://latex.codecogs.com/png.latex?%5CDelta%20Ct_%7Bref%7D "\Delta Ct_{ref}")
= A - B

    ## [1] -0.463

Not bad, pretty close to 0

Our
![\Delta Ct\_{target}](https://latex.codecogs.com/png.latex?%5CDelta%20Ct_%7Btarget%7D "\Delta Ct_{target}")
= C - D

    ## [1] -2.539

Ok, this indicates that our target gene is **downregulated** in the
treatment condition.

![\Delta\Delta Ct](https://latex.codecogs.com/png.latex?%5CDelta%5CDelta%20Ct "\Delta\Delta Ct")
\<- (A - B) - (C - D)

    ## [1] 2.076

Notice that the
![\Delta\Delta Ct](https://latex.codecogs.com/png.latex?%5CDelta%5CDelta%20Ct "\Delta\Delta Ct")
value is **positive**.

Now for the fold change transformation:
![2^{-(\Delta\Delta Ct)}](https://latex.codecogs.com/png.latex?2%5E%7B-%28%5CDelta%5CDelta%20Ct%29%7D "2^{-(\Delta\Delta Ct)}")
or
![2^{-(2.076)}](https://latex.codecogs.com/png.latex?2%5E%7B-%282.076%29%7D "2^{-(2.076)}")

    ## [1] 0.2371711

Our answer falls between 0 and 1 indicating that the target gene is
**downregulated**. This result makes sense given the negative
![\Delta Ct\_{target}](https://latex.codecogs.com/png.latex?%5CDelta%20Ct_%7Btarget%7D "\Delta Ct_{target}")
value.

### Now, a bad reference gene:

This example will use identical values for the target gene. Only the
reference gene has changed.

|                | Control | Treatment |
|:---------------|--------:|----------:|
| Reference gene |  21.903 |    26.475 |
| Target gene    |  24.149 |    26.688 |

Our
![\Delta Ct\_{ref}](https://latex.codecogs.com/png.latex?%5CDelta%20Ct_%7Bref%7D "\Delta Ct_{ref}")
= a - b

    ## [1] -4.572

Terrible, our reference gene is not stable.

Our
![\Delta Ct\_{target}](https://latex.codecogs.com/png.latex?%5CDelta%20Ct_%7Btarget%7D "\Delta Ct_{target}")
= c - d

    ## [1] -2.539

Again, this would indicate that our target gene is **downregulated** in
the treatment condition.

![\Delta\Delta Ct](https://latex.codecogs.com/png.latex?%5CDelta%5CDelta%20Ct "\Delta\Delta Ct")
\<- (a - b) - (c - d)

    ## [1] -2.033

Notice that our
![\Delta\Delta Ct](https://latex.codecogs.com/png.latex?%5CDelta%5CDelta%20Ct "\Delta\Delta Ct")
value is now **negative**!

Now for the fold change transformation:
![2^{-(\Delta\Delta Ct)}](https://latex.codecogs.com/png.latex?2%5E%7B-%28%5CDelta%5CDelta%20Ct%29%7D "2^{-(\Delta\Delta Ct)}")
or
![2^{-(-2.033)}](https://latex.codecogs.com/png.latex?2%5E%7B-%28-2.033%29%7D "2^{-(-2.033)}")

    ## [1] 4.09255

Now our answer is much greater than 1 indicating that the target gene is
highly **upregulated**! Even slight difference in expression of the
reference gene can generate spurious results!

### Did the reference gene perform well as a reference for defense genes?

![](Chase-PeaCR_files/figure-gfm/ref_gene_performance_defense-1.png)<!-- -->

### Did the reference gene perform well as a reference for cPEMV2?

![](Chase-PeaCR_files/figure-gfm/ref_gene_performance_cPEMV2-1.png)<!-- -->

![](Chase-PeaCR_files/figure-gfm/defense_genes_models-1.png)<!-- -->

## Exploratory plots:

### AO3

![](Chase-PeaCR_files/figure-gfm/AO3_test_plot-1.png)<!-- -->

### LOX2

![](Chase-PeaCR_files/figure-gfm/LOX2_test_plot-1.png)<!-- -->

### PR1

![](Chase-PeaCR_files/figure-gfm/PR1_test_plot-1.png)<!-- -->

### cPEMV2

![](Chase-PeaCR_files/figure-gfm/cPEMV2_test_plot-1.png)<!-- -->

### ![\2^{-\Delta \Delta Ct}](https://latex.codecogs.com/png.latex?%5C2%5E%7B-%5CDelta%20%5CDelta%20Ct%7D "\2^{-\Delta \Delta Ct}") calcs

## Figures

### AO3

![](Chase-PeaCR_files/figure-gfm/ao3_fig-1.png)<!-- -->

### LOX2

![](Chase-PeaCR_files/figure-gfm/lox2_fig-1.png)<!-- -->

### PR1

![](Chase-PeaCR_files/figure-gfm/pr1_fig-1.png)<!-- -->

### cPEMV2

![](Chase-PeaCR_files/figure-gfm/cpemv2_fig-1.png)<!-- -->

## Session Information

    R version 4.2.3 (2023-03-15 ucrt)
    Platform: x86_64-w64-mingw32/x64 (64-bit)
    Running under: Windows 10 x64 (build 19045)

    Matrix products: default

    locale:
    [1] LC_COLLATE=English_United States.utf8 
    [2] LC_CTYPE=English_United States.utf8   
    [3] LC_MONETARY=English_United States.utf8
    [4] LC_NUMERIC=C                          
    [5] LC_TIME=English_United States.utf8    

    attached base packages:
    [1] stats     graphics  grDevices utils     datasets  methods   base     

    other attached packages:
     [1] lmerTest_3.1-3  sjPlot_2.8.13   lme4_1.1-32     Matrix_1.5-3   
     [5] multcomp_1.4-25 TH.data_1.1-2   MASS_7.3-58.2   survival_3.5-3 
     [9] mvtnorm_1.1-3   car_3.1-2       carData_3.0-5   emmeans_1.8.5  
    [13] cowplot_1.1.1   lubridate_1.9.2 forcats_1.0.0   stringr_1.5.0  
    [17] dplyr_1.1.1     purrr_1.0.1     readr_2.1.4     tidyr_1.3.0    
    [21] tibble_3.2.1    ggplot2_3.4.1   tidyverse_2.0.0

    loaded via a namespace (and not attached):
     [1] Rcpp_1.0.10         lattice_0.20-45     zoo_1.8-12         
     [4] rprojroot_2.0.3     digest_0.6.31       utf8_1.2.3         
     [7] R6_2.5.1            backports_1.4.1     evaluate_0.20      
    [10] coda_0.19-4         pillar_1.9.0        rlang_1.1.0        
    [13] rstudioapi_0.14     minqa_1.2.5         performance_0.10.2 
    [16] nloptr_2.0.3        rmarkdown_2.21      ggeffects_1.2.0    
    [19] splines_4.2.3       munsell_0.5.0       broom_1.0.4        
    [22] numDeriv_2016.8-1.1 modelr_0.1.11       compiler_4.2.3     
    [25] xfun_0.38           pkgconfig_2.0.3     htmltools_0.5.5    
    [28] insight_0.19.1      tidyselect_1.2.0    codetools_0.2-19   
    [31] fansi_1.0.4         tzdb_0.3.0          withr_2.5.0        
    [34] sjmisc_2.8.9        grid_4.2.3          nlme_3.1-162       
    [37] xtable_1.8-4        gtable_0.3.3        lifecycle_1.0.3    
    [40] bayestestR_0.13.0   magrittr_2.0.3      scales_1.2.1       
    [43] datawizard_0.7.0    estimability_1.4.1  cli_3.6.1          
    [46] stringi_1.7.12      generics_0.1.3      vctrs_0.6.1        
    [49] boot_1.3-28.1       sandwich_3.0-2      sjlabelled_1.2.0   
    [52] tools_4.2.3         glue_1.6.2          sjstats_0.18.2     
    [55] hms_1.1.3           abind_1.4-5         fastmap_1.1.1      
    [58] yaml_2.3.7          timechange_0.2.0    colorspace_2.1-0   
    [61] knitr_1.42         
