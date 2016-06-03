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
- Save the job as ```trinity_stats.job```
- Submit the job with ```qsub trinity_stats.job```
- When the job is complete, view the log file. You will see scores like contig N50, etc.

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
Save the job file as ```trinity_bowtie.job```, then submit it.

Since it uses the bowtie read aligner, it will write most of the output to the ```bowtie_output``` directory. You can look at the files generated with

```
$ ls -lh bowtie_out
```
One of the output files will be a .bam file with the reads mapped. We will evaluate how well the reads mapped to the assembly with the script ```SAM_nameSorted_to_uniq_count_stats.pl```.

Create another job file using QSubGen. This time, we will load the Trinity module, and use the command:

```
SAM_nameSorted_to_uniq_count_stats.pl bowtie_out/bowtie_out.nameSorted.bam
```

Copy and paste the script and submit the job. Look at the log file. It should look something like this:

```
#read_type	count	pct
improper_pairs	3544	50.31
proper_pairs	2586	36.71
right_only	527	7.48
left_only	387	5.49

Total aligned reads: 7044

```

As you can see here, only 36.7% of the reads aligned properly to the assembled the transcriptome. 50% of the reads were 'improperly aligned'. This means that each end of the paired end reads ended up on different contigs. This is an indication that the assembly is quite fragemented. In this case, this is because we used a subsample of the original data or this workshop. If we were to use the whole data set, these statistics would likely be much higher. A normal Trinity assembly will have > 70% of the reads properly mapped to the assembly. If your number is below this threshold, it is possible that you should sequence at a greater depth of coverage.

####Assess number of full-length coding transcripts

We can also evaluate transcriptome assemblies based on the number of fully assembled coding transcripts. One way to do this is to BLAST the transcripts against a database of protein sequences. We will use a reduced version of SWISSPROT for this.