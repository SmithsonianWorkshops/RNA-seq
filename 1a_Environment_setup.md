## Environment setup
#### Login to Hydra and install R libraries

```$ ssh <username>@hydra-login01.si.edu```

Now load the R module and open R

```$ module load tools/R/3.2.1```

```$ R```

Now we are going to install bioconductor packages that we will use during the differential expression portion of the analysis.

```> source("http://bioconductor.org/biocLite.R")```

```> biocLite()```

```> biocLite('edgeR')```

```> biocLite('ctc')```

```> biocLite('Biobase')```

```> biocLite('ape')```

```> install.packages('gplots')```

Exit R

```> quit()```

When prompted to save your workspace image, reply 'n' and press enter:

```Save workspace image? [y/n/c]: n```


