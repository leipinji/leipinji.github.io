---
title: "RNA-Seq data analysis pipeline"
layout: post
---

# edgrR package for RNA-Seq data analysis


edgeR works on a table with integer read counts.

rows corresponding to genes and columns to independent conditions

edgeR store data in a simple list-based object called a DEGList.

The philosophy of edgeR is that all the information should be store in a single variable.


1. gene count table

`
count<-read.table(file='gene.count.txt')
`

2. sample group

`
group<-c('Control','Control','Control','Treat','Treat','Treat')
`

3. DEGlist  

`
cds<-DEGList(counts=count,group=group)
`


