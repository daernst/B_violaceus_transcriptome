# B_violaceus_transcriptome

### This repository contains code for *de novo* transcriptome assembly for *B. violaceus*:

1. Read quality check with FASTQC and trimming with BBDuk
2. Assemble transcriptome with Trinity
3. Screen the raw Trinity assembly for vector contaminants using BLASTN and NCBI's UniVec database
4. Filter out all lowly expressed transcripts (i.e., TPM < 1)
5. Extract longest open reading frame per transcript for final assembly
6. Assess transcriptome assembly completeness using BUSCO
