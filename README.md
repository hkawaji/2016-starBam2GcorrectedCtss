starBam2GcorrectedCtss.sh  Version 0.1 (May 31, 2016)
===============================
Convert BAM file to CTSS bed, where additional G is corrected
based on the supplementary note 3e of Nature genet 38:626-35.

Note that handling of 'additional G' depends on aligners (and their options).
This script is developed for STAR (https://github.com/alexdobin/STAR)
'--alignEndsType Local' (default), which is different from default behavior
of TopHat and BWA-SW.


Usage
-----

  starBam2GcorrectedCtss.sh -i SORTED.bam -c CHROM_SIZE -g GENOME_SEQ -q MAPPING_QUALITY

- SORTED.bam: alignment file produced by STAR
- CHROM_SIZE: chrom_size file, which is the used by BEDtools
- GENOME_SEQ: fasta file of the genome.
- MAPPING_QUALITY: threshold for mapping quality.


Output
------
The output is formatted as BED (for CTSS), where names represent internal scores
and the score represent the corrected counts.


Definition for interpretation
-----------------------------
For the entire profile,
- P: the chance of adding a 'G' (one value for the entire profile)

For each entry (alignment):
- X: observed read counts corresponding to the CTSS (this can be obtained from BAM entry)
- A0: counts of reads that are observed in this CTSS, with extra G mismatching to the genome (this can be obtained immediately from BAM entry)
- A:  counts of reads that are obserbed in this CTSS, with extra G.
- N:  corrected read counts corresponding to the CTSS
- U:  counts of reads that are obserbed in this CTSS, without extra G. 
- F:  the counts of reads that are obserbed in this CTSS but expected to belong to the next (1bp downstream) CTSS

State (example: 'HHHGGGHH' - H represent a base of non-G):
- S: start case (4th base, in the exammple above)
- G: generic case (5th, 6th base)
- E: end case (7th base)
- O: other case (1-3rd,8th base)


Requirements:
-------------
- bedtools
- samtools


Credit
----
- Author: Hideya Kawaji <kawaji@gsc.riken.jp>
- Copyright: RIKEN, Japan.
