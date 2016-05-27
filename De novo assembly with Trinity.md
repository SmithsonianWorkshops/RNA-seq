## _de novo_ transcriptome assembly with Trinity

**Note: This tutorial has been updated and modified from the paper _De novo transcript sequence reconstruction from RNA-seq using the Trinity platform for reference generation and analysis_ doi:[10.1038/nprot.2013.084](dx.doi.org/10.1038/nprot.2013.084) for use on the Smithsonian HPC**


#### Copy the data to you directory

I have downloaded the sample reads for this workshop. They can be found in ```/pool/genomics/workshops/RNAseq```. Make a new subdirectory in your ```/pool/genomics``` directory and copy the reads there.

```
$ cd /pool/genomics/<username>
$ mkdir RNAseq_workshop
$ cp /pool/genomics/workshops/RNAseq/TrinityNatureProtocol.tgz /pool/genomics/<username>
```

Now unpack/unzip the tar file and change into the TrinityNatureProtocol directory:

```
$ tar -xvzf TrinityNatureProtocol.tgz
$ cd TrinityNatureProtocol
```

Since the _de novo_ transcriptome should include as much information as possible, we will concatenate the reads across all sample treatments.

```
$ cat 1M_READS_sample/*.left.fq > reads.ALL.left.fq
$ cat 1M_READS_sample/*.right.fq > reads.ALL.right.fq
```

Now we will start the Trinity run.

First open the [QSubGen web application](https://hydra-3.si.edu/tools/QSubGen).

- Choose medium time limit and reserve 2GB of memory.
- Select 'multi-thread' and choose 10 CPUs.
- In the modules field, type ```trinity``` and the select the module ```bioinformatics/trinity/2.1.1```.
- In job specific commands, type ```Trinity --seqType fq --max_memory 20G --left reads.ALL.left.fq --right reads.ALL.right.fq --SS_lib_type RF --CPU $NSLOTS --full_cleanup```
- Choose a descriptive job name then clicke on ```Check if OK```
- If it passes, either save it and upload it to Hydra, or copy the text and paste it directly into your favority text editor.