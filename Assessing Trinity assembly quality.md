## Assessing Trinity assembly quality

####Check out the Trinity assembly

You should have the assembly written to ```trinity_out_dir.Trinity.fasta```. Check out the first few lines of the assembly:
```
$ head trinity_out_dir.Trinity.fasta
```

You will see the first few transcripts. Note that g1 refers to gene 1 and i1 refers to isoform 1. There can be many isoforms per gene. This can become important in downstream applications such as orthology assessment or differential expression.

Now check how many transcripts were assembled. An easy way to do this is to count the number of ```>``` in the fasta file. These each correspond to a transcript. You can do this with ```grep```.
```
$ grep -c '>' trinity_out_dir/Trinity.fasta
``` 
####Contig stats

Now we will generate stats about the transcripts. you can do this with the ```TrinityStats.pl``` script. This will be a very short job.

Again open [QSubGen](https://hydra-3.si.edu/tools/QSubGen)

- Choose ```short``` for ```CPU time```.
- Leave ```Memory``` at 1GB.
- Leave ```serial``` and ```sh``` selected.
- Begin typing ```trinity``` into the module field and select ```bioinformatics/trinity/2.1.1```
- Fill in ```Job specific commands``` with:
```
TrinityStats.pl trinity_out_dir.Trinity.fasta
```
- Choose a reasonable job name
- Copy and paste the script into your Unix text editor (hint: ```nano``` works well)

####Generate "more useful" stats

####Representation of reads

During this step, we will map some of the reads that we used in the assembler back to the transcriptome assembly. By evaluating how well paired end reads map back to the assembly, we can get a rough estimate of how well our assembly worked. To do this step, we will use the ```bowtie_PE_separate_then_join.pl``` script that is included in Trinity.

Let's open the familiar QSubGen. This time, you will fill it out yourself. Leave memory at the default and choose 10 CPU threads and load the ```bioinformatics/trinity/2.1.1	``` module.

The command will be:
```
bowtie_PE_separate_then_join.pl \
      --target trinity_out_dir.Trinity.fasta \
      --seqType fq \
      --left data/wt_SRR1582651_1.fastq \
      --right data/wt_SRR1582651_2.fastq \
      --aligner bowtie -- -p $NSLOTS --all --best --strata -m 300
```

Since it uses the bowtie read aligner, it will write most of the output to the ```bowtie_output``` directory. You can look at the files generated with

```
$ ls -lh bowtie_out
```
One of the output files will be a .bam file with the reads mapped. We will 