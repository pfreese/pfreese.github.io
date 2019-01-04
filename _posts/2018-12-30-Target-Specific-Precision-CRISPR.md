---
layout: post
title: Target-Specific Precision of CRISPR-Mediated Genome Editing
tags: papers
---

<a href="https://golang.org/pkg/testing/" target="_blank">official documentation on testing</a> by Anob M. Chakrabart et al., Molecular Cell

- Background

Double-stranded breaks induced by RNA-guided nucleases at target sites are recognized by the cell's DNA damage response pathways and repaired.
	- When an exogenous repair template is provided, the homologous recombination (HR) repair pathway allows introduction of precise modifications in the DNA sequence, including single point mutations or insertion of exogenous sequences
	- In the absence of a template, breaks are often repaired through error-prone mechanisms that result in insertions or deletions (indels) of variable length, often leading to disrupted reading frames and truncated, nonfunctional proteins.

Systematic investigation of off-target cleavage sites has shown that predicting the specificity of any given RNA-guided nuclease is not straightforward. Can systematic characterization of CRISPR-induced alterations in experimental systems provide information about how RNA-guided nucleases interact with complex genome and optimize desired editing outcomes? Recent characterization of indel patterns at multiple genomic loci have revealed that individual targets show reproducible repair outcome, with distinct preferences for class (insertion or deletion) and length of indels. Thus, can we transition from a qualitative understanding of RNA-guided nuclease determinants to quantitative parameters that predict indel activity?

- Methodology:
1. ~1500 pooled sgRNAs targeting 450 genes transfected into Cas9-expressing HepG2 cells
2. After editing target sites for 5 days, target regions captured by pull-down with probes, sequenced to ~6000-8000 coverage

- 1,248 sites showed detectable indels, ranging from 1-188 per target, with a median count of 32

- Findings

- "Precision" of editing at a particular site categorized by the commonest indel frequency. At times, discretized into three groups (imprecise - 0 < commonest indel frequency; middle (0.25-0.5); precise (>0.5)). There was a large range of editing precision, randing from some targets displaying up to 79 distinct, infrequent indels (<5% each) and others showing a single dominant indel (up to 94% frequency).
- Imprecise targets showed a high proportion of deletions (insertions being ~20% of total indels); insertions were more frequent in the middle precision group.
- While imprecise and middle targets sowed a range of indel sizes (deletions as long as 2.3kb), precise targets displayed a strong bias toward single-nucleotide indels).
- Predictive power of an artificial neural network trained to predict editing precision (i.e., commonest indel frequency) had only moderate preditive power (R^2 = 0.24).
	- Used 80% of targets randomly selected for training; remaining 20% used for testing (=130 sites). Used the raw sgRNA sequences as input: 20 nt plus the PAM sequence using a one-hot encoding - input layer thus has 86 nodes (21 variable nt x 4 possibilities, plus the 2 constant 'G's in the PAM sequence represented as constant values). These are followed by a single hidden layer of 512 neurons using ReLU activation functions, connected to a single output node followed by a softplus activation function. Loss function was mean squared error (L2 norm loss). ANN was built, trained, and deployed using Apache MXNET.
	- Used to identify important positions in the proto-spacer - if certain positions ave a significant influence on editing precision, randomizing those nt is expected to reduce the correlation between predicted & actual indel frequencies. Performed the nucleotide randomization 10 times, reporting the average percent reduction in R^2.
	- The nucleotide at position -4 from the PAM sequence had the strongest influence on editing precision, reducing the model's accuracy by ~80% upon randomization. Nucleotides -2, -3, and -5 also showed an effect (ranging from ~15% to ~50% decreases in accuracy).
	- The vast majority of target sites with an "A" or "T" at -4 repaired double-stranded breaks via insertions (77% and 91% of targets, respectively); 79% of sites with a G showed deletions and were mostly imprecise targets.

- Chromatin states affect RNA-guided nuclease activity, though effects were small in magnitude albeit consistent in different replicates.
	- Assayed by inducing histone hyperacetylation at target sites with inhibition of histone deacetylase (HDAC) activity with trichostatin A (TSA), and treatment of cells with EZH2i inhibitors which decrease H3K27me3 levels
	- TSA treatment (so increase of histone acetylation) significantly increased the efficiency of indel formation; EZH2i (decrease of H3K27me3) inhibited indel formation in constrast. Effect was particularly pronounced at sites w/ low endogenous levels of histone acetylation such that transient TSA treatment may be most effective in enhancing editing efficiency at sites in repressive chromatin environments.
	- These effects were primarily observed at imprecise editing sites; dominant indels at precise sites were unchanged (though only 6 genomic loci were assayed).


- Further questions

- How would a neural network perform in predicting the type of indel (e.g., insertion/deletion, which base(s) altered) instead of the commonest indel frequency?
- How does the indel profile of a particular site change across different cell types? 
- Are similar edits seens for other Cas systems (i.e., from other species that have different PAM sequences)?

- Sofware availability
https://github.com/luslab/crispr-indels.
