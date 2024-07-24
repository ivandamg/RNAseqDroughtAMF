# Transcript quantification of AMF with salmon
https://combine-lab.github.io/salmon/getting_started/

Quantify if reads map to different AMF genomes.

We select a single species of each AMF order.

1. Glomeraceae: Rhizophagus_irregularis
2. Acaulosporaceae: Acaulospora_colombiana
3. Diversisporaceae: Diversispora_epigaea
4. Gigasporaceae: Gigaspora_rosea
5. Claroideoglomeraceae: Claroideoglomus_candidum 
6. Ambisporaceae: Ambispora_gerdemannii
7. Paraglomeraceae: Paraglomus_occultum







1. build an index on the transcriptome

          sbatch --partition=pibu_el8 --job-name=SalmonIndexAMF --time=0-24:00:00 --mem-per-cpu=64G --ntasks=12 --cpus-per-task=1 --output=SalmonIndexAMF.out --error=SalmonIndexAMF.error --mail-type=END,FAIL --wrap "module load Salmon/1.4.0-GCC-10.3.0; cd /data/projects/p782_RNA_seq_Argania_spinosa/50_FinalArgan/20_InoculumComposition/01_RefMycGenomes; salmon index -t Acaulospora_colombiana_GCA_910592055.fna -i Acaulospora_colombiana_index;salmon index -t Ambispora_gerdemannii_GCA_910591945.fna -i Ambispora_gerdemannii_index; salmon index -t Claroideoglomus_candidum_GCA_910592215.fna -i Claroideoglomus_candidum_index; salmon index -t Diversispora_epigaea_GCA_003547095.fna -i Diversispora_epigaea_index; salmon index -t Gigaspora_rosea_GCA_003550325.fna -i Gigaspora_rosea_index; salmon index -t Paraglomus_occultum_GCA_910592205.fna -i Paraglomus_occultum_index; salmon index -t Rhizophagus_irregularis_GCA_000439145.fna -i Rhizophagus_irregularis_index"


                
                
                
2. Quantify the samples. by mapping the processed RNAseq reads to each AMF reference

                #in cluster      
                for FILE in $(ls *_1_clean.fastq.gz); do echo $FILE; sbatch --partition=pall --job-name=$(echo $FILE | cut -d'_' -f1) --time=02:00:00 --mem-per-cpu=12G --cpus-per-task=1 --output=bbtest.out --error=bbtest.error --mail-type=END,FAIL --wrap "module load UHTS/Analysis/salmon/0.11.2; salmon quant -i /data/projects/p782_RNA_seq_Argania_spinosa/03_SalmonQuantificationAMF/Claroideoglomus_candidum_index -l A -1 $FILE -2 $(echo $FILE | cut -d'_' -f1)_2_clean.fastq.gz -p 8 --validateMappings -o /data/projects/p782_RNA_seq_Argania_spinosa/03_SalmonQuantificationAMF/$(echo $FILE | cut -d'_' -f1)_Claroideoglomus_candidum_quant "   ; sleep 1; done
                for FILE in $(ls *_1_clean.fastq.gz); do echo $FILE; sbatch --partition=pall --job-name=DE$(echo $FILE | cut -d'_' -f1) --time=02:00:00 --mem-per-cpu=12G --cpus-per-task=1 --output=bbtest.out --error=bbtest.error --mail-type=END,FAIL --wrap "module load UHTS/Analysis/salmon/0.11.2; salmon quant -i /data/projects/p782_RNA_seq_Argania_spinosa/03_SalmonQuantificationAMF/Diversispora_eburnea_index -l A -1 $FILE -2 $(echo $FILE | cut -d'_' -f1)_2_clean.fastq.gz -p 8 --validateMappings -o /data/projects/p782_RNA_seq_Argania_spinosa/03_SalmonQuantificationAMF/$(echo $FILE | cut -d'_' -f1)_Diversispora_eburnea_quant "   ; sleep 1; done
                for FILE in $(ls *_1_clean.fastq.gz); do echo $FILE; sbatch --partition=pall --job-name=DE$(echo $FILE | cut -d'_' -f1) --time=02:00:00 --mem-per-cpu=12G --cpus-per-task=1 --output=bbtest.out --error=bbtest.error --mail-type=END,FAIL --wrap "module load UHTS/Analysis/salmon/0.11.2; salmon quant -i /data/projects/p782_RNA_seq_Argania_spinosa/03_SalmonQuantificationAMF/Diversispora_epigaea_index -l A -1 $FILE -2 $(echo $FILE | cut -d'_' -f1)_2_clean.fastq.gz -p 8 --validateMappings -o /data/projects/p782_RNA_seq_Argania_spinosa/03_SalmonQuantificationAMF/$(echo $FILE | cut -d'_' -f1)_Diversispora_epigaea_quant "   ; sleep 1; done
                for FILE in $(ls *_1_clean.fastq.gz); do echo $FILE; sbatch --partition=pall --job-name=Ri$(echo $FILE | cut -d'_' -f1) --time=02:00:00 --mem-per-cpu=12G --cpus-per-task=1 --output=bbtest.out --error=bbtest.error --mail-type=END,FAIL --wrap "module load UHTS/Analysis/salmon/0.11.2; salmon quant -i /data/projects/p782_RNA_seq_Argania_spinosa/03_SalmonQuantificationAMF/Rhizophagus_irregularis_index -l A -1 $FILE -2 $(echo $FILE | cut -d'_' -f1)_2_clean.fastq.gz -p 8 --validateMappings -o /data/projects/p782_RNA_seq_Argania_spinosa/03_SalmonQuantificationAMF/$(echo $FILE | cut -d'_' -f1)_Rhizophagus_irregularis_quant "   ; sleep 1; done
                for FILE in $(ls *_1_clean.fastq.gz); do echo $FILE; sbatch --partition=pall --job-name=Sc$(echo $FILE | cut -d'_' -f1) --time=02:00:00 --mem-per-cpu=12G --cpus-per-task=1 --output=bbtest.out --error=bbtest.error --mail-type=END,FAIL --wrap "module load UHTS/Analysis/salmon/0.11.2; salmon quant -i /data/projects/p782_RNA_seq_Argania_spinosa/03_SalmonQuantificationAMF/Scutellospora_calospora_index -l A -1 $FILE -2 $(echo $FILE | cut -d'_' -f1)_2_clean.fastq.gz -p 8 --validateMappings -o /data/projects/p782_RNA_seq_Argania_spinosa/03_SalmonQuantificationAMF/$(echo $FILE | cut -d'_' -f1)_Scutellospora_calospora_quant "   ; sleep 1; done
                for FILE in $(ls *_1_clean.fastq.gz); do echo $FILE; sbatch --partition=pall --job-name=Fm$(echo $FILE | cut -d'_' -f1) --time=02:00:00 --mem-per-cpu=12G --cpus-per-task=1 --output=bbtest.out --error=bbtest.error --mail-type=END,FAIL --wrap "module load UHTS/Analysis/salmon/0.11.2; salmon quant -i /data/projects/p782_RNA_seq_Argania_spinosa/03_SalmonQuantificationAMF/Funneliformis_mosseae_index -l A -1 $FILE -2 $(echo $FILE | cut -d'_' -f1)_2_clean.fastq.gz -p 8 --validateMappings -o /data/projects/p782_RNA_seq_Argania_spinosa/03_SalmonQuantificationAMF/$(echo $FILE | cut -d'_' -f1)_Funneliformis_mosseae_quant "   ; sleep 1; done
                for FILE in $(ls *_1_clean.fastq.gz); do echo $FILE; sbatch --partition=pall --job-name=Fm$(echo $FILE | cut -d'_' -f1) --time=02:00:00 --mem-per-cpu=12G --cpus-per-task=1 --output=bbtest.out --error=bbtest.error --mail-type=END,FAIL --wrap "module load UHTS/Analysis/salmon/0.11.2; salmon quant -i /data/projects/p782_RNA_seq_Argania_spinosa/03_SalmonQuantificationAMF/Rhizophagus_clarus_index -l A -1 $FILE -2 $(echo $FILE | cut -d'_' -f1)_2_clean.fastq.gz -p 8 --validateMappings -o /data/projects/p782_RNA_seq_Argania_spinosa/03_SalmonQuantificationAMF/$(echo $FILE | cut -d'_' -f1)_Rhizophagus_clarus_quant "   ; sleep 1; done
                
                for FILE in $(ls *_1_clean.fastq.gz); do echo $FILE; sbatch --partition=pall --job-name=Gr$(echo $FILE | cut -d'_' -f1) --time=02:00:00 --mem-per-cpu=12G --cpus-per-task=1 --output=bbtest.out --error=bbtest.error --mail-type=END,FAIL --wrap "module load UHTS/Analysis/salmon/0.11.2; salmon quant -i /data/projects/p782_RNA_seq_Argania_spinosa/03_SalmonQuantificationAMF/Gigaspora_rosea_index -l A -1 $FILE -2 $(echo $FILE | cut -d'_' -f1)_2_clean.fastq.gz -p 8 --validateMappings -o /data/projects/p782_RNA_seq_Argania_spinosa/03_SalmonQuantificationAMF/$(echo $FILE | cut -d'_' -f1)_Gigaspora_rosea_quant "   ; sleep 1; done
                
                for FILE in $(ls *_1_clean.fastq.gz); do echo $FILE; sbatch --partition=pall --job-name=Gc$(echo $FILE | cut -d'_' -f1) --time=02:00:00 --mem-per-cpu=12G --cpus-per-task=1 --output=bbtest.out --error=bbtest.error --mail-type=END,FAIL --wrap "module load UHTS/Analysis/salmon/0.11.2; salmon quant -i /data/projects/p782_RNA_seq_Argania_spinosa/03_SalmonQuantificationAMF/Glomus_cerebriforme_index -l A -1 $FILE -2 $(echo $FILE | cut -d'_' -f1)_2_clean.fastq.gz -p 8 --validateMappings -o /data/projects/p782_RNA_seq_Argania_spinosa/03_SalmonQuantificationAMF/$(echo $FILE | cut -d'_' -f1)_Glomus_cerebriforme_quant "   ; sleep 1; done
                

                # Count nb. of genes with more than zero reads 
                for i in $(ls *_quant/*.sf); do cat $i | awk '$5 >0' | wc -l; done
                # Change name of quant file for all files
                cp 7A_quant/quant.sf 7A_quant.sf
                cp 8A_quant/quant.sf 8A_quant.sf
                cp 9A_quant/quant.sf 9A_quant.sf
                gzip *.sf


