# Adapting ESM to Non-model Organisms

Notebooks to explore use cases of protein language models using [homolog-search-tools](https://github.com/chrisnguyen11/homolog-search-tools), a module I designed for this type of analysis.


## Overview

### 01: Species Bias
- Question:  Does the distribution of protein sequence likelihoods differ across organisms under the ESM-2 protein language model?
- Key Result: Controlling for proteome protein family composition using putative pairs of homologs, reveals no significant difference between the log-likelihoods of *E. coli* and *A. baylyi* proteome under the ESM-2 model (Sign test p-value range [0.245, 0.528]), suggesting that proteome protein family composition drives the difference when comparing the full proteomes.
- Connection: 02: Motivates fine-tuning ESM-2 on  *A. baylyi*  to adapt to species-specific sequence patterns.


### 02: LoRA Fine-tuning
- Question: Does fine-tuning ESM-2 on the *A. baylyi* proteome increase the protein likelihood under the model?
- Key Result: Fine-tuning ESM-2 on the *A. baylyi* proteome using LoRA significantly improved the log-likelihood under the model across all datasets (Sign test $p_{value} < 0.0001$) with the largest improvement in *A. baylyi* with 95.48% of sequences improved, followed by *E. coli* with 92.95% of sequences improved, then UniRef50 with 86.05% of sequences improved; this result is consistent with the evolutionary distance to the *A. baylyi* proteome, the data used for fine-tuning. 
- Connection: Refine an *A. baylyi* fine-tuned embedding space to generate sequences with optimization strategies with *A. baylyi* sequence patterns. 


### 03: ESM Decoder
- Question: Can ESM-2 embeddings be mapped back to the original amino acid sequence?
- Key Result: The full-length embedding decoder achieves a near perfect mapping, while the mean-pooled decoder exhibits model collapse, suggesting residue-level information is critical for consistent sequence reconstruction. 
- Connection: Provides a framework for mapping promising candidates identified in the *A. baylyi* fine-tuned embedding space back to amino acid sequences. 


## Key Results

### 01: Species Bias

Comparing the full proteomes, the Kolmogorov-Smirnov test identifies a significant difference in log-likelihoods between *E. coli* and *A. baylyi*. However, this difference is confounded by protein family composition, to control for protein family composition we identified putative pairs of homologs shared between *E. coli* and *A. baylyi*. We defined subsets of putative homologs that represented the log-likelihood distribution of the full proteome. These subsets have non-normal and 
asymmetric log-likelihood differences, violating the assumptions of the paired t-test and Wilcoxon Signed-Rank test. A representative subset:

| Metric | Value |
|---|---|
| Sign Test Statistic | 0.46268 |
| Sign Test p-value | 0.24575 |
| Bootstrapped Median | -0.0008 |
| Bootstrapped Median 95% CI | [-0.00260, 0.00087] |

Across the valid putative homolog subsets, the Sign test statistic near 0.5 and Bootstrapped Median confidence interval 
includes 0, indicating no significant difference between paired *E. coli* and *A. baylyi* homologs. This suggests that protein family composition drives the difference in log-likelihood, not species-specific bias in ESM-2.

### 02: LoRA Fine-tuning

Given that the distribution of log-likelihood differences between the fine-tuned and baseline model is non-normal and asymmetric, the Sign test describes the directional significance either positive for fine-tuned model or negative for the baseline model. Fraction improved articulates the relative effect size and the Bootstrapped Median quantifies the median in terms of log-likelihood with confidence intervals.

| Dataset | Fraction Improved | 95% CI | Bootstrapped Median | 95% CI |
|---|---|---|---|---|
| *A. baylyi* | 95.4807% | [94.7172%, 96.1384%] | 0.011366 | [0.011114, 0.011604] |
| *E. coli* | 92.9525% | [92.1457%, 93.6821%] | 0.010586 | [0.010403, 0.010785] |
| UniRef50 | 86.0450% | [85.1488%, 86.8954%] | 0.006714 | [0.006502, 0.006904] |

All three datasets had a positive significant improvement, Sign test $p_{value} < 0.001$. The decrease in log-likelihood improvement from *A. baylyi*, *E. coli* to UniRef50 is consistent with the evolutionary distance from the target proteome, *A. baylyi*. Likely sequence similarity between *A. baylyi* and *E. coli* is driving the improvement in *E. coli* log-likelihoods. However, the variance of the fine-tuned log-likelihood distribution for  *E. coli* and UniRef50 is greater than the baseline ESM-2 model suggesting that consistency on out-of-distribution sequences comes at the cost of fine-tuning.

### 03. ESM-2 Decoder

The AUROC evaluation metric is critical to quantify the decoder performance, AUROC with weighted reduction calculates a score for each class and computes the weighted average using their support while AUROC with macro reduction calculates a score for each class and averages them. For imbalanced datasets, AUROC-macro treats rarer classes equal to the majority class.

| Metric | Full-Length Embedding Decoder | Mean-Pooled Embedding Decoder |
|---|---|---|
| Test Accuracy | 0.9998 | 0.103 |
| Test Precision | 0.9998 | 0.111 |
| Test AUROC - Weighted | 1.0 | 0.577 |
| Test AUROC - Macro | 1.0 | 0.371 |

Full-Length Decoder: Near-perfect mapping across all natural residues, with test of AUROC-macro 1.0.

Mean-Pooled Decoder: Model collapse with the test accuracy of 0.103 and AUROC-macro of 0.371 indicate the model performs worse than random when weighting all residues equally.

## Summary

These notebooks describe a framework for adapting ESM-2 to non-model organisms, 01: Species Bias outlines that protein family composition drives log-likelihood differences between model and non-model organisms. 02: LoRA Fine-tuning demonstrates a fine-tuning procedure adapts ESM-2 to a non-model proteome, improving log-likelihoods under the model while preserving the log-likelihoods of out-of-distribution sequences. 03: ESM-2 Decoders develops methods to map from embedding spaces back to amino acid sequences. Together these notebooks can be used for sequence generation of non-model organisms in a fine-tuned embedding space.