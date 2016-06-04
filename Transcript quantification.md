## Transcript quantification

We will be using RSEM to quantify the expression levels of the transcripts that were assembled by Trinity.

To do this we will use the wrapper script included in the Trinity package called ```align_and_estimate_abundance.pl```.

Create another job file with QSubGen. This will be a serial job and you should reserve 4GB of memory. It will also run fine in the short queue.

Load the trinity module and enter the following program command:

```
align_and_estimate_abundance.pl --seqType fq \
         --left data/GSNO_SRR1582648_1.fastq \
         --right data/GSNO_SRR1582648_2.fastq  \
         --transcripts trinity_out_dir.Trinity.fasta  \
         --output_prefix GSNO_SRR1582648 \
         --est_method RSEM  --aln_method bowtie \
         --trinity_mode --prep_reference --coordsort_bam \
         --output_dir GSNO_SRR1582648.RSEM
```

Save the script into a file called ```trinity_rsem_GNSO_SRR1582648.job```. Since there are six biological replicates, we'll need to make six of these files--one for each replicate.

Take some time to create the five other files for the other treatments. Be sure to change both the names of the reads and the ```--output-prefix```.

Now you can submit each of these jobs using ```qsub job_file_name```. This is the magic of parallel computing clusters--you can run many jobs at once.