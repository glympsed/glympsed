# glympsed: Gene Pathway and Structure Discovery

`glympsed` is a pipeline for querying large RNAseq gene expression data sets with more than 100 samples and 30,000 genes each for patterns of common expression using the unsupervised machine-learning library, [Keras](http://keras.io/). This tool can be used to enable discovery of common patterns of expression where there is are unknown relationships between samples. The results can then be used to provide evidence to generate hypotheses for further investigation.

## 1 Running software

The pipeline uses a combination of Python scripts (<i>file.py</i>) as well as <strong>iPython Notebook</strong> (<i>file.ipynb</i>). The user is expected to provide a matrix of counts for each sample x gene ID.

#### 1.1 Script Overview

* Scripts for visualizing the clustering of the nodes (hidden layers) that the Autoencoder method found can be found in the **run_unsupervised.py** script that is within the *sample-models/* directory and can be called from main.

* The file **calc-gene-pathway-counts.R** - R script that can be used to calculate the number of genes that are within each pathway and the number of pathways that each gene appears in - output as CSV for each.

* **Autoencoder weights** - visualizing 50 nodes (rows) x 5549 genes (columns) to explore the clustering of nodes within a subspace of the gene space.

* **Autoencoder codings** - visualize 50 nodes (rows) x 950 samples (columns) to explore the clustering of nodes within a subspace of the sample space.

#### 1.2 Calling from the Command Line

Calling the <i>main.py</i> script from the command line:

```
    $ python __main__.py
```

The <i>main.py</i> script calls the executer function to run the project.

<hr>

## 2 Data

This project contains three sets of data - open-access Pseudomonas, simulated, and <strong>MMETSP</strong>.

#### 2.1 Pseudomonas

Reproduces a previous study with data from Pseudomonas aeruginosa ([Tan et al 2015](http://msystems.asm.org/content/1/1/e00025-15)). 

#### 2.2 Simulated

Steps for creating simulated data:

* create 2 _Beta_ probability distributions
* multiplication of the 2 _Betas_
* take *Binomial* distribution (*Bernoulli*) of point above

#### 2.3 MMETSP

The Marine Microbial Eukaryotic Transcriptome Sequencing Project (MMETSP), which contains RNAseq data from 678 divergent samples representing more than 40 phyla ([Keeling et al. 2014](http://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.1001889). Many of the species in this data set do not have a reference genomes. Following transcriptome assembly, the procedure for annotation is not straightforward.

<hr>

## 3 Analysis  

#### 3.1 Overview

This project makes use of a variety of dimensional reduction, clustering, and unsupervised and semi-supervised machine-learning techniques. These include <strong>PCA, ICA, and Autoencoders</strong>. In addition, <strong>t-SNE</strong> was used for dimensional reduction, clustering, and visualization.  

#### 3.2 PCA and ICA

Did not perform well.

#### 3.3 Autoencoder

#### 3.4 t-SNE

[t-SNE](https://lvdmaaten.github.io/tsne/) can be used as an independent method for dimensional reduction and visualization or in combination with a dimensional reduction technique - e.g. PCA.

Some examples of t-SNE usage:

* <strong>PCA into 50 dimensions and then scatter plot of all genes in 2-dimensions with t-SNE</strong> to try to visualize the clustering of those 50 PCs as matching the 50 hidden nodes from Autoencoding (bad results as expected since PCA does not do well on this data)

* <strong>t-SNE directly on raw data to try to find clustering - scatter plot of all genes in 2-dimensions</strong> - this is also not good as expected (since PCA did not work well)

* <strong>2-dimensional plot of all nodes from hidden layer (Autoencoder) clustered with t-SNE</strong> - this is actually two plots: one for the Autoencoder weights and another for the Autoencoder codings. In these plots you can start to see some nodes (some hidden layers) that really stand out from the rest - **such as node 18** - which probably has a major influence on genetic expression

#### 3.5 Extra

In addition to the methods mentioned above - we have visualizations of the following *post-structure-discovery* methods:

* <strong>Feature importance of nodes (from hidden layers of Autoencoder) as predictors</strong> for classifying genes to their respective nodes of highest weight - through random forest multi-classification - this means I assigned each gene a label that corresponded to the node that had the highest weight value for it

* <strong>Feature importance of samples (from original dataset of 950 samples / predictors)</strong> for classifying genes to their respective nodes of highest weight - through random forest multi-classification and then boosted classification methods (*GBM*)

<strong>NOTE:</strong> feature importance is measured as the gain in reducing misclassifications per feature / predictor / column. So this means feature importance is a measure of (generally) how much better any specific feature is at making the entire model predict the nodes / classes of the genes.  

## Collaboration

This is a collaboration between Lisa Cohen (Titus Brown lab, UC Davis), Harriet Alexander (Titus Brown lab, UC Davis), Dave Harris (Ethan White lab, University of Florida), Yuan Liu (Princeton University), and Oliver Muellerklein (Wayne Getz lab, UC Berkeley). We started as team "burgers and mushrooms" at the Moore Foundation's Data Driven Discovery Barn-Raising event held at the Mount Desert Island Biological Laboratory in Bar Harbor, Maine from May 1-6, 2016 coordinated by Dr. Casey Greene and Dr. Blaire Sullivan.
