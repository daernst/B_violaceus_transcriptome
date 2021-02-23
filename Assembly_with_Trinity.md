## Assemble transcriptome with Trinity:

```
#!/bin/bash

### ADD SCHEDULER HEADER HERE ### 


echo Script start time : `date +"%H:%M:%S"`


### Load required modules ###
module load trinity/2.4.0 bowtie2/2.3.5.1


### Change directories to scratch space ###
cd /scratch/${SLURM_JOB_ID}


### Set directory containing fastq files ###
READ_DIR <- "/home/daernst/Braden_B_violaceus_project/bbduk-trimmed_fastq_files"


### Concatenate all read 1 fastq files ###
echo Concatenating all read 1 files
cat ${READ_DIR}/*_Read-1_bbduk-trimmed.fastq.gz > ${READ_DIR}/All_B_violaceas_Read-1_bbduk-trimmed.fastq.gz
echo Done.
echo Time : `date +"%H:%M:%S"`


### Concatenate all read 2 fastq files ###
echo Concatenating all read 2 files
cat ${READ_DIR}/*_Read-2_bbduk-trimmed.fastq.gz > ${READ_DIR}/All_B_violaceas_Read-2_bbduk-trimmed.fastq.gz
echo Done.
echo Time : `date +"%H:%M:%S"`


### Assemble transcriptome with Trinity ###
echo Starting Trinity Run...
echo Time : `date +"%H:%M:%S"`
/share/apps/bioinformatics/trinity/trinityrnaseq-Trinity-v2.4.0/Trinity \
    --seqType fq \
    --left ${READ_DIR}/All_B_violaceas_Read-1_bbduk-trimmed.fastq.gz \
    --right ${READ_DIR}/All_B_violaceas_Read-2_bbduk-trimmed.fastq.gz \
    --SS_lib_type RF \
    --CPU 24 \
    --min_contig_length 200 \
    --max_memory 768G \
    --output /home/daernst/Braden_B_violaceus_project/Trinity_B_violaceus_transcriptome_BBduk \
    --full_cleanup
echo Done.
echo Time : `date +"%H:%M:%S"`
```
