## Check quality of raw reads with FastQC:

```
#!/bin/bash

### Load required modules ###
module load java/sunjdk_1.8.0 fastqc


### Check quality of read 1 fastq files ###
fastqc /home/daernst/Braden_B_violaceus_project/Raw_fastq_files/Read_1_concatenated/*.fastq.gz


### Check quality of read 2 fastq files ###
fastqc /home/daernst/Braden_B_violaceus_project/Raw_fastq_files/Read_2_concatenated/*.fastq.gz
```


## Quality and adapter trimming raw reads with BBDuk:

```
#!/bin/bash

### ADD SCHEDULER HEADER HERE ###


### Change directories to scratch space ###
cd /scratch/${PBS_JOBID}


### Adapter and quality trim raw reads for each sample ###
for SAMPLE in A12 B11 B12 C11 C12 D11 D12 E11 E12 F11 F12

do

    echo Trimming sample ${SAMPLE} with BBDuk...

    /share/apps/bioinformatics/bbmap/bbduk.sh \
        -Xmx8g \
        in1=/home/daernst/Braden_B_violaceus_project/Raw_fastq_files/Read_1_concatenated/${SAMPLE}_Read-1.fastq.gz \
        in2=/home/daernst/Braden_B_violaceus_project/Raw_fastq_files/Read_2_concatenated/${SAMPLE}_Read-2.fastq.gz \
        out1=/home/daernst/Braden_B_violaceus_project/bbduk-trimmed_fastq_files/${SAMPLE}_Read-1_bbduk-trimmed.fastq.gz \
        out2=/home/daernst/Braden_B_violaceus_project/bbduk-trimmed_fastq_files/${SAMPLE}_Read-2_bbduk-trimmed.fastq.gz \
        ref=/share/apps/bioinformatics/bbmap/resources/adapters.fa \
        ktrim=r \
        k=23 \
        mink=11 \
        hdist=1 \
        tpe \
        tbo \
        qtrim=rl \
        trimq=20 \
        minlength=50

    echo Done.

done
```

## Check quality of trimmed reads with FastQC:

```
#!/bin/bash

### Load required module ###
module load java/sunjdk_1.8.0 fastqc


### Check quality of trimmed fastq files ###
fastqc /home/daernst/Braden_B_violaceus_project/bbduk-trimmed_fastq_files/*_bbduk-trimmed.fastq.gz
```
