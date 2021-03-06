R Session - mldr.datasets Package
=================================

This notebook shows how to use the `mldr.datasets` package to retrieve multi-label datasets from the [Cometa](https://cometa.ujaen.es) repository, partition these datasets and writing them using disparate file formats. You can run the code by yourself by loading this file from RStudio.

Installing and loading the package
----------------------------------

`mldr.datasets` is available at CRAN, so you can install as any other R package through the `install.packages()` function. Development version is available in GitHub.

~~~~ {.r}
# install.packages("mldr.datasets"           # Uncomment this line if you need to install the package from CRAN
# remotes::install_github("fcharte/mldr.datasets")    # Uncomment this line if you want to install the latest version (in development) from GitHub

library(mldr.datasets)
~~~~

    ## 
    ## Attaching package: 'mldr.datasets'

    ## The following object is masked from 'package:stats':
    ## 
    ##     density

Query the available datasets
----------------------------

~~~~ {.r}
available.mldrs()[c(1,16,20),] # Get some meta-data columns from three of the available datasets
~~~~

~~~~ {.r}
available.mldrs()  # Simply call available.mldrs() to retrieve the updated full list from cometa.ujaen.es
~~~~

Download a dataset from the repository
--------------------------------------

~~~~ {.r}
databib <- bibtex() # Load the bibtex dataset, downloading it if it is not locally available
~~~~

    ## Looking for dataset bibtex in the download directory

    ## Looking for dataset bibtex online...

    ## Downloading dataset bibtex

~~~~ {.r}
summary(databib) # Get a summary of traits of the data
~~~~

    ##                   Length Class      Mode     
    ## name                 1   -none-     character
    ## dataset           1997   data.frame list     
    ## attributesIndexes 1836   -none-     numeric  
    ## attributes        1995   -none-     character
    ## labels               6   data.frame list     
    ## labelsets         2856   -none-     numeric  
    ## measures            13   -none-     list     
    ## bibtex               1   -none-     character

~~~~ {.r}
cat(toBibtex(emotions)) # Retrieves citation information of a previously loaded dataset: emotions
~~~~

    ## @incollection{,
    ##   title = "Multi-Label Classification of Emotions in Music",
    ##   author = "Wieczorkowska, A. and Synak, P. and Ra'{s}, Z.",
    ##   booktitle = "Intelligent Information Processing and Web Mining",
    ##   year = "2006",
    ##   volume = "35",
    ##   chapter = "30",
    ##   pages = "307--315"
    ## }

Dataset partitioning and exporting to other file fomat
------------------------------------------------------

~~~~ {.r}
# Divide the emotions dataset into 5 folds and write them using MULAN and CSV
# file formats, taking into account the sparsity level
write.mldr(random.kfolds(emotions, k = 5), format=c("MULAN", "CSV"), 
           sparse = sparsity(emotions) > 0.5, basename="emotions",
           noconfirm = TRUE)
~~~~
