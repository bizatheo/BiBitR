
<!-- README.md is generated from README.Rmd. Please edit that file -->
Installing the BiBitR Package
-----------------------------

``` r
install.packages("devtools") # If not yet installed on your R Version
devtools::install_github("hadley/devtools") # Only run this if your currently installed 
                                            # devtools version is <= 1.12 (recursive dependencies bug)

devtools::install_github("ewouddt/BiBitR")
```

Should the installation of `BiBitR` throw an error, please install the dependencies manually:

``` r
install.packages(c("flexclust","biclust"))
```

Details
-------

`BiBitR` is a simple R wrapper which directly calls the original Java code for applying the BiBit algorithm after which the output is transformed to a `Biclust` S4 class object. The original Java code can be found at <http://eps.upo.es/bigs/BiBit.html> by Domingo S. Rodriguez-Baena, Antonia J. Perez-Pulido and Jesus S. Aguilar-Ruiz.

More details about the **BiBit** algorithm can be found in:

-   [Domingo S. Rodriguez-Baena, Antonia J. Perez-Pulido and Jesus S. Aguilar-Ruiz (2011), "A biclustering algorithm for extracting bit-patterns from binary datasets", *Bioinformatics*](http://bioinformatics.oxfordjournals.org/content/early/2011/08/08/bioinformatics.btr464.abstract).

The `bibit` function uses the original Java code directly (with the intended input and output). Because the Java code was not refactored, the `rJava` package could not be used.

The `bibit` function does the following:

1.  Convert R matrix to a `.arff` output file.
2.  Use the `.arff` file as input for the Java code which is called by `system()`.
3.  The outputted `.txt` file from the Java BiBit algorithm is read in and transformed to a `Biclust` object. Because of this, there is a chance of *overhead* when applying the algorithm on large datasets. Make sure your machine has enough RAM available when applying to big data.

Return Value
------------

A Biclust S4 Class object.

Example
-------

``` r
library(BiBitR)

data <- matrix(sample(c(0,1),100*100,replace=TRUE,prob=c(0.9,0.1)),nrow=100,ncol=100)
data[1:10,1:10] <- 1 # BC1
data[11:20,11:20] <- 1 # BC2
data[21:30,21:30] <- 1 # BC3
data <- data[sample(1:nrow(data),nrow(data)),sample(1:ncol(data),ncol(data))]

result <- bibit(data,minr=5,minc=5)
result
```
