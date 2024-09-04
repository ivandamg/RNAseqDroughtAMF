# Commands to blast a sequence to a ref database

## 1. make db

      makeblastdb -dbtype prot -in Medicago_truncatula.fa -parse_seqids -out db_Mtrunactula_aa/Mtruncatula_db makeblastdb -dbtype prot -in Argania_spinosa_v1.fa -parse_seqids -out db_Aspinosa_aa/Aspinosa_db      


## 2.  blastp
      
      blastp -db /Users/mateusgo/Documents/40_RNAseqPaper/03_MycorrhizalGenes/db/Mtruncatula_db -outfmt 6 -out DMI1/Blast_DMI1_Medicago.xml -query /Users/mateusgo/Documents/40_RNAseqPaper/03_MycorrhizalGenes/DMI1/DMI1.fa 
