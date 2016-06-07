## Differential Gene Expression with edgeR

_Note: First we need to install one more R package. To do so follow these instructions:_

Once you are logged in to hydra:

```
$ module load tools/R/3.2.1
$ R
```

Now that you are in R, you need to load the biocLite function again:

```
> source("http://bioconductor.org/biocLite.R")
> biocLite()
> biocLite('qvalue')
```

You will be prompted to update your other libraries. Respond with ```n```.

####Create sample text file

You will need to create a tab delimited text file containing information for your different samples. In the first column, you will have the name of the condition and, in the second column, you will enter the name of the sample. Start a new text file, samples.txt, with nano.

```
$ nano samples.txt
```

Enter the following (keep in mind that the condition and names should be separated by tabs!).

```
GSNO	GSNO_SRR1582648
GSNO	GSNO_SRR1582647
GSNO	GSNO_SRR1582646
WT	wt_SRR1582651
WT	wt_SRR1582649
WT	wt_SRR1582650
```
