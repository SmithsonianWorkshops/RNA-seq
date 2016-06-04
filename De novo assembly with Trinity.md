## _De novo_ transcriptome assembly with Trinity

**Note: This tutorial has been updated and modified from the Trinity tutorial given by Brian Haas [here](https://github.com/trinityrnaseq/KrumlovTrinityWorkshopJan2016/wiki/Home/e67c7a4ae4fe005866a56371ea29f15c79e8ccfb) for use on the Smithsonian HPC. We will also use data from [Defining the transcriptomic landscape of Candida glabrata by RNA-Seq](http://www.ncbi.nlm.nih.gov/pubmed/?term=25586221)**


#### Get the data

I have downloaded the reads that we will be using for this workshop. They can be found in ```/pool/genomics/workshops/RNAseq```. Make a new subdirectory in your ```/pool/genomics``` directory and copy the reads there.

```
$ cd /pool/genomics/<username>
$ mkdir RNAseq_workshop
$ cd RNAseq_workshop
$ cp /pool/genomics/workshops/RNAseq/data.tar.gz .
```

Now unpack/unzip the tar file:

```
$ tar -xvzf data.tar.gz
```

Go ahead and take a look at the data in your directory:

```
ls -lh data
```

The paper from which these data are derived examined *C. glabrata* in two conditions, nutrient rich (wt) and under nitrosative stress (GNSO). Note that for each replicate, there are two files, ending in: 1.fastq and 2.fastq. This is because the reads are paired end.

In this tutorial we are assuming that there is no good reference genome. Because of this, we need to generate a reference transcriptome that includes all of the data that you wish to analyze, so our _de novo_ transcriptome assembly will include the reads from all of the replicates.

However, before we start the Trinity run, we will do some quality assessment with FASTQC.

####Read quality assessment with FASTQC

FastQC is a program that can quickly scan your raw data to help figure out if there are adapters or low quality reads present. Create a job file to run FastQC on one of the three files you downloaded.

* Create a job file to run FASTQC on the data you just copied to your working directory:  
	+ hint: use the QSub Generator: ```https://hydra-3.si.edu/tools/QSubGen```
    + *Remember Chrome works best with this and to accept the security warning message*  
    + **CPU time:** short *(we will be using short for all job files in this tutorial)*
    + **memory:** 2GB
    + **PE:** serial
    + **module:** ```bioinformatics/fastqc/0.11.5```
    + **command:** ```fastqc <FILE.fastq>```  
    + **job name:** FASTQC *(or name of your choice)*  
    + **log file name:** FASTQC.log  
    + hint: either use ```nano``` or upload your job file using ```scp``` from your local machine into the `assembly_tutorial` directory. See [here](https://confluence.si.edu/display/HPC/Disk+Space+and+Disk+Usage) and [here](https://confluence.si.edu/display/HPC/Transferring+files+to+or+from+Hydra) on the Hydra wiki for more info.  
    + hint: submit the job on Hydra using ```qsub``` 
	+ after your job finishes, find the results and download some of the images, e.g. ```per_base_quality.png``` to your local machine using ```scp```

####Trimming adapters with TrimGalore 
TrimGalore will auto-detect what adapters are present and remove very low quality reads (quality score <20) by default.  

* Create a job file to run TrimGalore on your data:  
	+ **command**: ```trim_galore --paired --retain_unpaired <FILE_1.fastq> <FILE_2.fastq>```  
	+ **module**: ```bioinformatics/trimgalore/0.4.0```
	+ You can then run FastQC again to see if anything has changed.


####Trinity _de novo_ assembly

Now we will start the Trinity run.

First open the [QSubGen web application](https://hydra-3.si.edu/tools/QSubGen).

- Choose medium time limit and reserve 6GB of memory.
- Select 'multi-thread' and choose 10 CPUs.
- In the modules field, type ```trinity``` and the select the module ```bioinformatics/trinity/2.1.1```.
- In job specific commands, type 
```
Trinity --seqType fq  \
          --left data/wt_SRR1582649_1.fastq,data/wt_SRR1582651_1.fastq,data/wt_SRR1582650_1.fastq,data/GSNO_SRR1582648_1.fastq,data/GSNO_SRR1582646_1.fastq,data/GSNO_SRR1582647_1.fastq \
          --right data/wt_SRR1582649_2.fastq,data/wt_SRR1582651_2.fastq,data/wt_SRR1582650_2.fastq,data/GSNO_SRR1582648_2.fastq,data/GSNO_SRR1582646_2.fastq,data/GSNO_SRR1582647_2.fastq \
          --max_memory 2G --min_contig_length 150 --CPU $NSLOTS --full_cleanup
 ```
- Choose a descriptive job name then click on ```Check if OK```
- If it passes, either save it and upload it to Hydra, or copy the text and paste it directly into your favority text editor.

Now save your text file into your ```/pool/genomics/<username>/RNAseq_workshop``` directory as ```trinity.job```.

Now submit your job with the command: ```qsub trinity.job```

Soon your transcriptome assembly will be finished!