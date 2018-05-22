---
title: "RNA-Seq data analysis pipeline"
layout: post
---

# edgeR package for RNA-Seq data analysis


> edgeR works on a table with integer read counts.
rows corresponding to genes and columns to independent conditions
edgeR store data in a simple list-based object called a DEGList.
The philosophy of edgeR is that all the information should be store in a single variable.


## 1. gene count table

`
count<-read.table(file='gene.count.txt')
`

## 2. sample group

`
group<-c('Control','Control','Control','Treat','Treat','Treat')
`

## 3. DEGlist  

`
cds<-DEGList(counts=count,group=group)
`

## 4. filtering the data

> get rid of genes with low reads aligned. 
I suggest choose this cutoff as at least 10 counts per million on any gene in all conditions

`
keep<-rowSums(cpm(cds) >=10) >=2
cds<-cds[keep,]
`

> after filtering, it is a good idea to reset the library sizes

`
cds$samples$lib.size<-colSums(cds$counts)
cds$samples
`
## 5. normalizing the data

> edgeR is concerned with differential expression analysis rather than with the quantification of expression levels. It is concerned with relative changes in expression levels between conditions, but not directly with estimating absolute expression levels.


`
cds<-calNormFactors(cds)
`

## 6. data exploration(optional)

> produce a plot showing the sample relations based on multidimensional scaling.

`
plotMDS(cds,method='bcv',col=as.numeric(cds$samples$group))
legend("topleft",as.character(unique(cds$samples$group)),pch=20)
`

## 7. estimating the dispersion

> The first major step in the analysis of DEG data using the NB model is to estimate the dispersion parameter for each tag, a measure of the degree of inter-library variation for that tag. Estimating the common dispersion gives an idea of overall variability across the genome for this dataset.

`
cds<-estimateCommonDisp(cds,verbose=True)
`

`
cds<-estimateTagwiseDisp(cds)
`

## 8. differential expression analysis

`
results<-exactTest(cds,pair=c('Control','Treat'))
topTags(results,n=20)
`

> total number of differentially expressed genes at FDR < 0.05 
p value adjust method Benjamini and Hochberg's algorithm

`
degs<-decideTestsDEG(results,adjust.method='BH',p.value=0.05)
summary(degs)
`






