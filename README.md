# Comparing two RNAseq analysis tools

This repository is the final deliverable for our tutored project as a group of Master bioinformatics students.  
The goal was to make and benchmark 3 pipelines using STAR+HTseq count, STAR quantmode, and Kallisto for RNAseq analysis. These were made with Snakemake.  
The Snakefile contains all 3 pipelines. There is also a config file to allow easy customization of the pipeline.
  
# Kallisto pipeline  
It runs the 2 main Kallisto commands:  
   - The first one indexes the cDNA fasta file  
   - The second runs the quantification tool  
   - We added an extra transcript-to-gene tool to convert the counts to gene counts  
  
# STAR-HTseq pipeline  
Here we have to use 3 different pieces of software :  
  - Star itself, to index the genome and map the reads on it  
  - Samtools, to convert the .sam output of Star into a .bam, and then to sort and index this file  
  - Htseq-count, which is the quantification tool for the reads we got using Star.  
  
# STAR quantmode pipeline  
This pipeline uses the same index as the previous pipeline, followed by the built-in STAR quantmode command.  
  - Index the reference genome with STAR    
  - Count the reads by feature with STAR quantmode   

# Setting it up  
- Place the Snakefile in a folder along with the config.json and the /input/ folder.  
- Reads must be paired-end (2 files per condition), and placed in the /input/ folder.  
- You must change the read extension in the config file, ex: `"READ_EXTENSION" : ".fastq"`  
- You must change the paths for the DNA, cDNA and the GTF file in the config.json file, ending with a /  
- The genomes must be .fa  
- If the reads are compressed, change the SUP_STAR_COMPR parameter in the config file to `"--readFilesCommand zcat"`, otherwise leave it empty: "".  
- You must add the list of experimental conditions as a list in the config file, ex: `"SAMPLE": ["C1reads", "C2reads"]`  
- You can also change the separator between the read name and number, ex: "C1reads_1" "C1reads_2" => `"SEPARATOR":"_"`  
  
- To launch: move to your directory and type snakemake in the terminal, add -j [number of threads] to choose the total number of threads to be used. You can change the number of threads per rule in the config file: 3 rules can be run at the same time, so the total number needs to be three times the per rule thread number. Example:  
Console command: `snakemake -j 12`  
In the config file:`"THREADS" : "4"`  
