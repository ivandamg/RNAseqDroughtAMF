# Bioinformatics pipeline

## 1. Clean reads with fastp

         for FILE in $(ls *1.fastq.gz); do echo $FILE; sbatch --partition=pshort_el8 --job-name=$(echo $FILE | cut -d'_' -f1)fastp --time=0-02:00:00 --mem-per-cpu=24G --ntasks=1 --cpus-per-task=1 --output=$(echo $FILE | cut -d'_' -f1)_fastp.out --error=$(echo $FILE | cut -d'_' -f1)_fastp.error --mail-type=END,FAIL --wrap " cd /data/projects/p782_RNA_seq_Argania_spinosa/21_RNAseqV2/00_rawreads ; module load FastQC; ~/00_Software/fastp --in1 $FILE --in2 $(echo $FILE | cut -d'_' -f1)_2.fastq.gz --out1 ../02_TrimmedData/$(echo $FILE | cut -d'_' -f1)_1_trimmed.fastq.gz --out2 ../02_TrimmedData/$(echo $FILE | cut -d'_' -f1)_2_trimmed.fastq.gz -h ../02_TrimmedData/$(echo $FILE | cut -d',' -f1)_fastp.html --thread 4; fastqc -t 4 ../02_TrimmedData/$(echo $FILE | cut -d'_' -f1)_1_trimmed.fastq.gz; fastqc -t 4 ../02_TrimmedData/$(echo $FILE | cut -d'_' -f1)_2_trimmed.fastq.gz"; sleep  1; done


## 2. transcript quantification with salmon 
(https://combine-lab.github.io/salmon/getting_started/)


### a. build an index on the transcriptome

            
         sbatch --partition=pshort_el8 --job-name=SalmonIndex --time=0-02:00:00 --mem-per-cpu=12G --ntasks=1 --cpus-per-task=1 --output=SalmonIndexHap1.out --error=SalmonIndexHap1.error --mail-type=END,FAIL --wrap "module load Salmon; cd /data/projects/p782_RNA_seq_Argania_spinosa/30_FinalAssemblyPaper/10_RNAseq/01_Hap1; salmon index -t Aspinosa_hap1_cds.fasta -i Argan_hap1_index"

### b. Quantify the samples
 
            for FILE in $(ls *1_trimmed.fastq.gz); do echo $FILE; sbatch --partition=pshort_el8 --job-name=$(echo $FILE | cut -d'_' -f1)_Hap1 --time=0-02:00:00 --mem-per-cpu=12G --ntasks=1 --cpus-per-task=1 --output=$(echo $FILE | cut -d'_' -f1)_SalmonHap1.out --error=$(echo $FILE | cut -d'_' -f1)_SalmonHap1.error --mail-type=END,FAIL --wrap "module load Salmon/1.4.0-GCC-10.3.0; cd /data/projects/p782_RNA_seq_Argania_spinosa/50_FinalArgan/02_TrimmedData ; salmon quant -i /data/projects/p782_RNA_seq_Argania_spinosa/30_FinalAssemblyPaper/10_RNAseq/01_Hap1/Argan_hap1_index -l A -1 $FILE -2 $(echo $FILE | cut -d'_' -f1)_2_trimmed.fastq.gz -p 8 --validateMappings -o /data/projects/p782_RNA_seq_Argania_spinosa/30_FinalAssemblyPaper/10_RNAseq/01_Hap1/01_SalmonQuant/$(echo $FILE | cut -d'_' -f1)_quant"; sleep  1; done
