# MultiK
## This edited version of MultiK includes fixed bugs and performs SCT or log normalization depending on the slot selected. 

Single cell RNA-seq analysis tool that objectively selects multiple insightful numbers of clusters (*K*).


## Getting started


## Installing
**MultiK** relies on the following R packages: **Seurat**, **SigClust**, please install them before installing **MultiK**.

```{}
#install.packages("Seurat")
#install.packages("SigClust")
```

```{}
#install.packages("devtools")
#library(devtools)
#install_github("siyao-liu/MultiK")
```

**MultiK( )** is the main function that implements the subsampling and application of the Seurat clustering over multiple resolution parameters. Details about this function can be found in the user manual:

```{}
?MultiK
```


The main function, **MultiK( )**, takes a Seurat object with the normalized expression matrix and other parameters set by default values if not specified. MultiK explores a range of resolution parameters (from 0.05 to 2.00 with a step size of 0.05) in Seurat clustering, and aggregates all the clustering runs that give rise to the same K groups regardless of the resolution parameter and computes a consensus matrix for each K.

Note: MultiK re-selects highly variable genes in each subsampling run. Also, MultiK, by default, uses 30 principal components and 20 K-nearest neighbors in Seurat clustering.  



## MultiK workflow

## Example
For this vignette, we use a 3 cell line mixture dataset published from Dong et al. 2019 (https://pubmed.ncbi.nlm.nih.gov/31925417/) to demonstrate the workflow of MultiK. This dataset contains ~2,600 cells and is included in the MultiK package.

```{}
data(p3cl)
seu <- p3cl
```

### Step 1: Run **MultiK** main algorithm to determine optimal Ks

Run subsampling and consensusing clustering to generate output for evaluation (this step can take a long time). For demonstration purpose, we are running 10 reps here. For real data pratice, we recommend using at least 100 reps.
```{}
multik <- MultiK(seu, reps=10)
```

Make MultiK diagnostic plots: 
```{}
DiagMultiKPlot(multik$k, multik$consensus)
```

### Step 2: Assign _classes_ and _subclasses_

Get the clustering labels at optimal K level:
```{}
clusters <- getClusters(seu, 3)
```

Run SigClust at optimal K level:
```{}
pval <- CalcSigClust(seu, clusters$clusters)
```

Make diagnostic plots (this includes a dendrogram of cluster centroids with the pairwise SigClust _p_ values mapped on the nodes, and a heatmap of the pairwise SigClust _p_ values)
```{}
PlotSigClust(seu, clusters$clusters, pval)
```


## License
This software is licensed under MIT License.


## Contact
If you have any questions, please contact: **Siyao Liu** (<siyao@email.unc.edu>)
