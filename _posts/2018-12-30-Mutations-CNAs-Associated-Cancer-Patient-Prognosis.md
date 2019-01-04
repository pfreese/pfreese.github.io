---
layout: post
title: Systematic identification of mutations and copy number alterations associate with cancer patient prognosis
tags: papers
---

<a href="https://golang.org/pkg/testing/" target="_blank">official documentation on testing</a> by Joan Smith and Jason Sheltzer in eLIFE

- Background:
Cancers that arise from the same tissue can exhibit a wide range of clinical outcomes (e.g., in early stage colorectal cancer, ~60% of patients will be cured by surgery alone while the remaining 40% will experience a recurrence that's often fatal). Various pathological and molecular biomarkers are dividied into two classes:
	1. Predictive (identify patients who are likely to respond to specific therapies like EGFR mutations that sensitize tumors to EGFR inhibition).
	2. Prognostic (provide information on cancer aggressiveness and likelihood of patient death - e.g., tumor de-differentiation and lymph-node infilation are strongly associated with poor outcomes).

This paper focuses on determining genetic alterations with prognostic value - e.g., which genetic alterations are more frequent in either deadly or treatable cancers, between indolent tumors and aggressive malignancies? Previous studies have shown DNA copy number alterations (CNAs) have indicated that highly-aneuploid tumors tend to have worse outcomes than diploid tumors, but these analyses focused largely on arm-length changes or alterations affecting single oncogenes or tumor-suppressors. This study aims to extend these findings in an unbiased pan-cancer study examining the importance of copy number changes in most genes at the single-gene level.

- Methodology:
Analysis of publicly available genomic profiles of 17,879 tumors (from The Cancer Genome Atlas (TCGA) and International Cancer Genome Consortium (ICGC) projects) from patients with known outcomes

- Findings

- Cancer mutations (among genes mutated in >2% of patients in each of the 16 TCGA cohorts) uncovered very few mutations that were significantly associated with patient outcome as assessed by Cox proportional hazard analysis of mutant/normal for the gene overall.
	- Only two of the top 30 most-frequently mutated cancer driver genes (EGFR and TP53) were associated w/ prognosis in more than two tumor types.
	- Mutations in KRAS, PIK3CA, CDKN2A, BRAF, KMT2D, ATM, SMAD4 and many other genes were frequently observed but were never significantly linked with patient outcome.
	- Analysis at the codon-level (e.g., KRAS^c12) were similarly uniformative.
	- Clonality (restricting to mutations at clonal levels in single tumors) did not improve patient stratification.
	- The 5 genes with the strongest survival associations were all observed in glioma (PTEN and EGFR mutations conferred dismal prognosis, while mutations in IDH1, TP53, and ATRX were associated with favorable prognosis).

- In contrast to mutations, copy number alterations (CNAs) at driver genes were commonly associated with patient mortality.
	- Used copy # of each gene at its TSS and regressed this value against patient outcome in each tumor cohort.
	- Amplification of EGFR, PIK3CA, and BRAF, and deletion of CDKN2A, RB1, and EP300 were strongly associated w/ shorter patient survival times in 4+ cancer types each.
	- Among 30 most frequently-mutated cancer driver genes, they detected 108 significant associations between gene copy number and outcome, compared to 23 with mutation and outcome.
	- Discretization of CNA measurements into 'deletions' (log2FC < -0.3), 'amplifications' (log2FC > 0.3), or 'copy-neutral' did not significantly change findings, so copy number alterations having continuous values does not seem to be the reason more prognostic associations are found for it compared to mutational status.
	- Methodological detail: Z-scores from multiple different individual tumor-type analyses combined into a meta-analysis Z-score via Stouffer's Method:
		Z = sum(Z_i)/sqrt(k)

- Driver gene CNAs contain prognostic information not captured by TP53 mutational status or total aneuploidy, both of which have previously been associated w/ poor clinical outcomes.
	- Assessed by multivariate models that included TP53 mutational status, total tumor aneuploidy, or total number of breakpoints, with most (75%+) driver gene CNAs remaining prognostic.

- Focal CNAs (<3 Mb) typically portend worse prognosis than broad CNAs.
	- Interpreted as a reflection of aneuploidy-induced fitness penalties with large CNAs changing the dosage of multiple genes at once impairing tumor growth, while targeted alterations specifically altering driver gene CN maximize malignant potential.
	- Focal CNAs affect patient outcome by changing te expression levels of WT genes (using transcript level correlation w/ gene CN changes).

- Results overall generally replicated in independent ICGC data set & ~2700 tumors with targeted sequencing and CN analysis from Memorial Sloan Kettering.

- Further questions

- If CNAs act primarily through altering driver gene expression levels, do select mutations that similarly affect gene expression levels also have prognostic associations?
- Which cancers that have highly-prognostic CNAs also have therapies that could potentially improve patient outcome.
- Are there assay(s) that can reliably measure both driver gene CNAs (for prognostic purposes) and mutational status (for potential targeted therapy)?
- Is finding that glioma has largest number of mutational associations with prognosis biological or technical?

- Sofware availability

https://github.com/joan-smith/genomic-features-survival
