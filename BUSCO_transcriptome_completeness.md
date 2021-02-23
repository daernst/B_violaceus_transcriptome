## Assess transcriptome assembly completeness using BUSCO:

```
#!/bin/bash

### ADD SCHEDULER HEADER HERE ###


### Load required modules ###
module load blast/2.3.0+ python/3.6.0 gcc boost augustus/3.2.2 hmmer/3.1b2


### Move to directory with transcriptome assembly ###
cd /home/daernst/Braden_B_violaceus_project/Trinity_B_violaceus_transcriptome_BBduk/TransDecoder_output/final_assembly


### Run BUSCO to assess transcriptome completeness based on eukaryota single-copy orthologs ###
/home/daernst/Bioinformatics_programs/busco/bin/busco \
    -i B_violaceus_final_transcriptome_assembly.fasta \
    -l /home/daernst/Bioinformatics_programs/BUSCO_datasets/eukaryota_odb10 \
    -m transcriptome \
    -o B_violaceus_BUSCO_eukaryota \
    -c 12 \
    -e 1e-03


### Run BUSCO to assess transcriptome completeness based on metazoa single-copy orthologs ###
/home/daernst/Bioinformatics_programs/busco/bin/busco \
    -i B_violaceus_final_transcriptome_assembly.fasta \
    -l /home/daernst/Bioinformatics_programs/BUSCO_datasets/metazoa_odb10 \
    -m transcriptome \
    -o B_violaceus_BUSCO_metazoa \
    -c 12 \
    -e 1e-03
```
