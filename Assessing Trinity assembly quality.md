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

Now we will generate stats about the transcripts. you can do this with the ```TrinityStats.pl``` script. This will be a very short job.


