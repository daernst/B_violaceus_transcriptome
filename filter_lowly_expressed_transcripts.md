## Align reads to transcriptome with Bowtie2, estimate transcript abundance with RSEM, and filter lowly expressed transcripts:

```
#!/bin/bash

### ADD SCHEDULER HEADER HERE ### 


### Load required modules ###
module load perl samtools bowtie2/2.3.5.1 rsem gcc/7.3.1 mkl/18.0.2 R/3.4.4


### Build index for transcriptome ###
echo Prepping reference...
/share/apps/bioinformatics/trinity/trinityrnaseq-Trinity-v2.4.0/util/align_and_estimate_abundance.pl \
    --transcripts /home/daernst/Braden_B_violaceus_project/Trinity_B_violaceus_transcriptome_BBduk/B_violaceus_transcriptome_BBduk_CLEAN.Trinity.fasta \
    --est_method RSEM \
    --aln_method bowtie2 \
    --trinity_mode \
    --prep_reference
echo Done.


### Enter directory containing trimmed fastq files ###
cd /home/daernst/Braden_B_violaceus_project/bbduk-trimmed_fastq_files


### Align and estimate transcript abundance using Bowtie2 and RSEM ###
echo aligning and estimating transcript abundance for each sample...
/share/apps/bioinformatics/trinity/trinityrnaseq-Trinity-v2.4.0/util/align_and_estimate_abundance.pl \
    --transcripts /home/daernst/Braden_B_violaceus_project/Trinity_B_violaceus_transcriptome_BBduk/B_violaceus_transcriptome_BBduk_CLEAN.Trinity.fasta \
    --samples_file /home/daernst/Braden_B_violaceus_project/Trinity_B_violaceus_transcriptome_BBduk/samples_bbduk.txt \
    --seqType fq \
    --est_method RSEM \
    --output_dir /home/daernst/Braden_B_violaceus_project/Trinity_B_violaceus_transcriptome_BBduk/RSEM_output
    --aln_method bowtie2 \
    --SS_lib_type RF \
    --thread_count 32 \
    --trinity_mode
echo Done.


### Enter directory with RSEM output ###
cd /home/daernst/Braden_B_violaceus_project/Trinity_B_violaceus_transcriptome_BBduk/RSEM_output


### Make abundance matricies for genes and isoforms ###
echo Making abundance matricies for genes and isoforms...
 /home/daernst/Bioinformatics_programs/trinityrnaseq-v2.9.1/util/abundance_estimates_to_matrix.pl \
    --est_method RSEM \
    --out_prefix RSEM \
    --name_sample_by_basedir \
    --gene_trans_map /home/daernst/Braden_B_violaceus_project/Trinity_B_violaceus_transcriptome_BBduk/B_violaceus_transcriptome_BBduk_CLEAN.Trinity.fasta.gene_trans_map \
    Above_IS_ISA1/RSEM.isoforms.results \
    Below_NP_NPB3B/RSEM.isoforms.results \
    Below_NP_NPB1/RSEM.isoforms.results \
    Above_IS_ISA6B/RSEM.isoforms.results \
    Below_IS_ISB2/RSEM.isoforms.results \
    Above_NP_NPA6B/RSEM.isoforms.results \
    Above_NP_NPA1/RSEM.isoforms.results \
    Below_IS_ISB8B/RSEM.isoforms.results \
    Above_IS_ISA5B/RSEM.isoforms.results \
    Above_NP_NPA2/RSEM.isoforms.results \
    Below_NP_NPB2B/RSEM.isoforms.results \
    Below_IS_ISB6B/RSEM.isoforms.results
echo Done.


### Remove all transcripts with TPM < 1 from transcriptome assembly ###
echo Filter out transcripts with TPM<1 ...
/home/daernst/Bioinformatics_programs/trinityrnaseq-v2.9.1/util/filter_low_expr_transcripts.pl \
    --matrix RSEM.isoform.TPM.not_cross_norm \
    --transcripts /home/daernst/Braden_B_violaceus_project/Trinity_B_violaceus_transcriptome_BBduk/B_violaceus_transcriptome_BBduk_CLEAN.Trinity.fasta \
    --min_expr_any 1 \
    --gene_to_trans_map /home/daernst/Braden_B_violaceus_project/Trinity_B_violaceus_transcriptome_BBduk/B_violaceus_transcriptome_BBduk_CLEAN.Trinity.fasta.gene_trans_map \
    > B_violaceus_transcriptome_BBduk_CLEAN_FILTERED.Trinity.fasta
echo Done.
```
