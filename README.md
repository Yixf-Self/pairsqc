### example run
```
python pairsqc --pairs ../pairix/samples/merged_nodup.tab.chrblock_sorted.txt.gz --chrsize ~/data/references/hg19.chrom.sizes > log2
Rscript plot2.r
```

&nbsp;
### QC metrics and plots
#### Cis-to-trans ratio
* Cis-to-trans ratio is computed as number_of_cis_reads / (number_of_cis_reads + number_of_trans_reads) * 100, where a cis read is defined as an intrachromosomal read whose 5'-5' separation is > T. A trans read is an interchromosomal read. T=20kb.
* Cis-to-trans ratio at T=5kb and T=20kb show only minor difference (less than 10%).

#### Proportion of read orientations versus genomic separation
* s = 5'-5' separation of an intrachromosomal read.
* s is binned at log10 scale at interval of 0.1 (growing by ~1.25-fold).
* For each bin, the number of reads with each of the four orientations is obtained. To compute proportion, each count is supplemented with a pseudocount of 1E-100, and divided by the sum over the four orientations for that bin.
* The first bin where the four orientations converge is called resolution, and is determined by using standard deviation of the proportions < 0.02.

![](tests/proportion.20170208.png)
![](tests/sd_w_cut.20170208.png)

#### Contact propability versus genomic separation
* s = 5'-5' separation of an intrachromosomal read.
* s is binned at log10 scale at interval of 0.1 (growing by ~1.25-fold).
* For each bin, contact probability is computed as number_of_reads / number_of_possible_reads / bin_size.
  * number_of_possible_reads is computed as the sum of L_chr - s_mid - 1 over all chromosomes included in the input `chrsize` file, where L_chr is the length of a chromosome. This is equivalent of L_genome - N_chr * (s_mid + 1), where L_genome is the sum of all chromosome lengths and N_chr is the number of chromosomes. S_mid is the mid point of the bin at log10 scale (bin 10^2.8 ~ 10^2.9 has mid point 10^2.85).
  * bin_size is computed as max distance - min distance (e.g. for bin 10^2.8 ~ 10^ 2.9, the binsize is 10^2.9 - 10^2.8).

![](tests/log10prob.20170208.png)
