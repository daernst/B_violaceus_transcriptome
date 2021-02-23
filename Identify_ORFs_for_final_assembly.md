## Extract longest open reading frame per transcript for final assembly:

```
#!/bin/bash

### ADD SCHEDULER HEADER HERE ###


### Enter directory containing transcriptome ###
cd /home/daernst/Braden_B_violaceus_project/Trinity_B_violaceus_transcriptome_BBduk


### Extract open reading frames ###
echo Extracting long open reading frames...
/home/daernst/Bioinformatics_programs/TransDecoder-TransDecoder-v5.5.0/TransDecoder.LongOrfs \
   -t B_violaceus_transcriptome_BBduk_CLEAN_FILTERED.Trinity.fasta \
   --gene_trans_map B_violaceus_transcriptome_BBduk_CLEAN.Trinity.fasta.gene_trans_map \
   -m 100
echo Done.


### Extract the longest open reading frame per transcript ###
echo Predict likely coding regions and keep longest ORF per transcript...
/home/daernst/Bioinformatics_programs/TransDecoder-TransDecoder-v5.5.0/TransDecoder.Predict \
    -t B_violaceus_transcriptome_BBduk_CLEAN_FILTERED.Trinity.fasta \
    --single_best_only
echo Done.


### Clean up headers for final transcriptome assembly fasta ###
cut -d '.' \
   -f 1 B_violaceus_transcriptome_BBduk_CLEAN_FILTERED.Trinity.fasta.transdecoder.cds \
   > B_violaceus_final_transcriptome_assembly.fasta
```
