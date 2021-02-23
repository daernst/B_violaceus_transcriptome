## Use BLASTN to screen the raw Trinity assembly for vector contaminants using NCBI's UniVec database:

```
#!/bin/bash

### ADD SCHEDULER HEADER HERE ###


### Load required module ###
module load blast/2.9.0+


### Download UniVec database ###
cd /home/daernst/Braden_B_violaceus_project/Trinity_B_violaceus_transcriptome_Pinnacle/VecScreen/
wget ftp://ftp.ncbi.nih.gov/pub/UniVec/UniVec_Core


### Make UniVec blast database ###
echo Making UniVec database...
makeblastdb \
    -in /home/daernst/Braden_B_violaceus_project/Trinity_B_violaceus_transcriptome_Pinnacle/VecScreen/UniVec_Core \
    -input_type fasta \
    -dbtype nucl \
    -parse_seqids \
    -out /home/daernst/Braden_B_violaceus_project/Trinity_B_violaceus_transcriptome_Pinnacle/VecScreen/UniVecDB
echo Done.


### Blast transcriptome against UniVec database ###
echo Blasting transcriptome against UniVec database...
blastn \
    -db /home/daernst/Braden_B_violaceus_project/Trinity_B_violaceus_transcriptome_Pinnacle/VecScreen/UniVecDB \
    -query /home/daernst/Braden_B_violaceus_project/Trinity_B_violaceus_transcriptome_BBduk/B_violaceus_transcriptome_BBduk.Trinity.fasta \
    -num_threads 8 \
    -task blastn \
    -reward 1 \
    -penalty -5 \
    -gapopen 3 \
    -gapextend 3 \
    -dust yes \
    -soft_masking true \
    -evalue 0.01 \
    -searchsp 1750000000000 \
    -outfmt 6 \
    > /home/daernst/Braden_B_violaceus_project/Trinity_B_violaceus_transcriptome_BBduk/VecScreen/univec_blastOut-B_violaceus_transcriptome_BBduk.Trinity
echo Done.
```
