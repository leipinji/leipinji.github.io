---
layout: post
title: R heatmap for triangular matrix
---

# Get the upper part of the matrix

> matrix[lower.tri(matrix)]<-NA
> pheatmap(matrix,cluster_rows=F,cluster_cols=F)


