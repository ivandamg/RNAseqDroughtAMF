# Transcript quantification of AMF with salmon
https://combine-lab.github.io/salmon/getting_started/

Quantify if reads map to different AMF genomes.

We select a single species of each AMF order.

- Glomeraceae: Rhizophagus_irregularis
- Acaulosporaceae: Acaulospora_colombiana
- Diversisporaceae: Diversispora_epigaea
- Gigasporaceae: Gigaspora_rosea
- Claroideoglomeraceae: Claroideoglomus_candidum 
- Ambisporaceae: Ambispora_gerdemannii
- Paraglomeraceae: Paraglomus_occultum







## 1. build an index on the transcriptome

          sbatch --partition=pibu_el8 --job-name=SalmonIndexAMF --time=0-24:00:00 --mem-per-cpu=64G --ntasks=12 --cpus-per-task=1 --output=SalmonIndexAMF.out --error=SalmonIndexAMF.error --mail-type=END,FAIL --wrap "module load Salmon/1.4.0-GCC-10.3.0; cd /data/projects/p782_RNA_seq_Argania_spinosa/50_FinalArgan/20_InoculumComposition/01_RefMycGenomes; salmon index -t Acaulospora_colombiana_GCA_910592055.fna -i Acaulospora_colombiana_index;salmon index -t Ambispora_gerdemannii_GCA_910591945.fna -i Ambispora_gerdemannii_index; salmon index -t Claroideoglomus_candidum_GCA_910592215.fna -i Claroideoglomus_candidum_index; salmon index -t Diversispora_epigaea_GCA_003547095.fna -i Diversispora_epigaea_index; salmon index -t Gigaspora_rosea_GCA_003550325.fna -i Gigaspora_rosea_index; salmon index -t Paraglomus_occultum_GCA_910592205.fna -i Paraglomus_occultum_index; salmon index -t Rhizophagus_irregularis_GCA_000439145.fna -i Rhizophagus_irregularis_index"
  
                
                
## 2. Quantify the samples. by mapping the processed RNAseq reads to each AMF reference

                for FILE in $(ls *_1_trimmed.fastq.gz); do echo $FILE; sbatch --partition=pshort_el8 --job-name=Acua_$(echo $FILE | cut -d'_' -f1) --time=02:00:00 --mem-per-cpu=12G --cpus-per-task=1 --output=bbtest.out --error=bbtest.error --mail-type=END,FAIL --wrap " module load Salmon/1.4.0-GCC-10.3.0; salmon quant -i /data/projects/p782_RNA_seq_Argania_spinosa/50_FinalArgan/20_InoculumComposition/01_RefMycGenomes/Acaulospora_colombiana_index -l A -1 $FILE -2 $(echo $FILE | cut -d'_' -f1)_2_trimmed.fastq.gz -p 8 --validateMappings -o /data/projects/p782_RNA_seq_Argania_spinosa/50_FinalArgan/20_InoculumComposition/$(echo $FILE | cut -d'_' -f1)_Acaulospora_colombiana_quant"   ; sleep 1; done

                for FILE in $(ls *_1_trimmed.fastq.gz); do echo $FILE; sbatch --partition=pshort_el8 --job-name=AMBI$(echo $FILE | cut -d'_' -f1) --time=02:00:00 --mem-per-cpu=12G --cpus-per-task=1 --output=bbtest.out --error=bbtest.error --mail-type=END,FAIL --wrap "module load Salmon/1.4.0-GCC-10.3.0; salmon quant -i /data/projects/p782_RNA_seq_Argania_spinosa/50_FinalArgan/20_InoculumComposition/01_RefMycGenomes/Ambispora_gerdemannii_index -l A -1 $FILE -2 $(echo $FILE | cut -d'_' -f1)_2_trimmed.fastq.gz -p 8 --validateMappings -o /data/projects/p782_RNA_seq_Argania_spinosa/50_FinalArgan/20_InoculumComposition/$(echo $FILE | cut -d'_' -f1)_Ambispora_gerdemannii_quant "   ; sleep 1; done


                for FILE in $(ls *_1_trimmed.fastq.gz); do echo $FILE; sbatch --partition=pshort_el8 --job-name=Claro$(echo $FILE | cut -d'_' -f1) --time=02:00:00 --mem-per-cpu=12G --cpus-per-task=1 --output=bbtest.out --error=bbtest.error --mail-type=END,FAIL --wrap "module load Salmon/1.4.0-GCC-10.3.0; salmon quant -i /data/projects/p782_RNA_seq_Argania_spinosa/50_FinalArgan/20_InoculumComposition/01_RefMycGenomes/Claroideoglomus_candidum_index -l A -1 $FILE -2 $(echo $FILE | cut -d'_' -f1)_2_trimmed.fastq.gz -p 8 --validateMappings -o /data/projects/p782_RNA_seq_Argania_spinosa/50_FinalArgan/20_InoculumComposition/$(echo $FILE | cut -d'_' -f1)_Claroideoglomus_candidum_quant "   ; sleep 1; done

                for FILE in $(ls *_1_trimmed.fastq.gz); do echo $FILE; sbatch --partition=pshort_el8 --job-name=Dive$(echo $FILE | cut -d'_' -f1) --time=02:00:00 --mem-per-cpu=12G --cpus-per-task=1 --output=bbtest.out --error=bbtest.error --mail-type=END,FAIL --wrap "module load Salmon/1.4.0-GCC-10.3.0; salmon quant -i /data/projects/p782_RNA_seq_Argania_spinosa/50_FinalArgan/20_InoculumComposition/01_RefMycGenomes/Diversispora_epigaea_index -l A -1 $FILE -2 $(echo $FILE | cut -d'_' -f1)_2_trimmed.fastq.gz -p 8 --validateMappings -o /data/projects/p782_RNA_seq_Argania_spinosa/50_FinalArgan/20_InoculumComposition/$(echo $FILE | cut -d'_' -f1)_Diversispora_epigaea_quant "   ; sleep 1; done


                for FILE in $(ls *_1_trimmed.fastq.gz); do echo $FILE; sbatch --partition=pshort_el8 --job-name=Giga$(echo $FILE | cut -d'_' -f1) --time=02:00:00 --mem-per-cpu=12G --cpus-per-task=1 --output=bbtest.out --error=bbtest.error --mail-type=END,FAIL --wrap "module load Salmon/1.4.0-GCC-10.3.0; salmon quant -i /data/projects/p782_RNA_seq_Argania_spinosa/50_FinalArgan/20_InoculumComposition/01_RefMycGenomes/Gigaspora_rosea_index -l A -1 $FILE -2 $(echo $FILE | cut -d'_' -f1)_2_trimmed.fastq.gz -p 8 --validateMappings -o /data/projects/p782_RNA_seq_Argania_spinosa/50_FinalArgan/20_InoculumComposition/$(echo $FILE | cut -d'_' -f1)_Gigaspora_rosea_quant "   ; sleep 1; done

                for FILE in $(ls *_1_trimmed.fastq.gz); do echo $FILE; sbatch --partition=pshort_el8 --job-name=Para$(echo $FILE | cut -d'_' -f1) --time=02:00:00 --mem-per-cpu=12G --cpus-per-task=1 --output=bbtest.out --error=bbtest.error --mail-type=END,FAIL --wrap "module load Salmon/1.4.0-GCC-10.3.0; salmon quant -i /data/projects/p782_RNA_seq_Argania_spinosa/50_FinalArgan/20_InoculumComposition/01_RefMycGenomes/Paraglomus_occultum_index -l A -1 $FILE -2 $(echo $FILE | cut -d'_' -f1)_2_trimmed.fastq.gz -p 8 --validateMappings -o /data/projects/p782_RNA_seq_Argania_spinosa/50_FinalArgan/20_InoculumComposition/$(echo $FILE | cut -d'_' -f1)_Paraglomus_occultum_quant "   ; sleep 1; done

                for FILE in $(ls *_1_trimmed.fastq.gz); do echo $FILE; sbatch --partition=pshort_el8 --job-name=Rhizo$(echo $FILE | cut -d'_' -f1) --time=02:00:00 --mem-per-cpu=12G --cpus-per-task=1 --output=bbtest.out --error=bbtest.error --mail-type=END,FAIL --wrap "module load Salmon/1.4.0-GCC-10.3.0; salmon quant -i /data/projects/p782_RNA_seq_Argania_spinosa/50_FinalArgan/20_InoculumComposition/01_RefMycGenomes/Rhizophagus_irregularis_index -l A -1 $FILE -2 $(echo $FILE | cut -d'_' -f1)_2_trimmed.fastq.gz -p 8 --validateMappings -o /data/projects/p782_RNA_seq_Argania_spinosa/50_FinalArgan/20_InoculumComposition/$(echo $FILE | cut -d'_' -f1)_Rhizophagus_irregularis_quant "   ; sleep 1; done
                
                

                # Count nb. of genes with more than zero reads 
                for i in $(ls *_quant/*.sf); do cat $i | awk '$5 >0' | wc -l; done
                # Change name of quant file for all files
                cp 7A_quant/quant.sf 7A_quant.sf
                cp 8A_quant/quant.sf 8A_quant.sf
                cp 9A_quant/quant.sf 9A_quant.sf
                gzip *.sf


