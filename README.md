# This repository is created to start the process of creating a genomic catalogue of *treptococcus pyogenes* genomes


This is a set of commands perfomed to prepare the files to build the catalog

1. gbff files were downloaded from the website: https://www.ncbi.nlm.nih.gov/assembly/?term=streptococcus%20pyogenes
2. Gbff files were decompressed and transformed into gbk files using ugene

`for i in `cat list`; do gunzip $i.gbff.gz ; ugene convert-seq --in=$i.gbff --out=genbank/$i.gbk --format=genbank ; done`

We changed strategy while performing some of the analyses and decided to run PIRATE and Panaroo as methods to identify and characterize the pangenome of the species. 

For this, we downloaded all fasta sequences for all assemblies and re-annotate it with Prokka.
 
 `for i in `cat list`; do gunzip $i.fna.gz ; /media/venezuela7/repository_software_recent/prokka/bin/prokka --outdir Prokka_$i $i.fna ; done`
 
Once we re-annotated the genomes we moved all gff files to a separate folder for analyses:

`for i in `cat list`; do cp Prokka_$i/*gff gff_files/$i.gff ; done`

## Analyses with PIRATE

We ran PIRATE on the new set of annotated genomes uner 4 different conditions 50%, 70%, 90% and 95% of completion:

`PIRATE -i gff_files/ -s "50,70,90,95" -o pirate_results/ -a -r`

## Analyses with Panaroo

We ran panaroo to detect contamination first and remove any atypical genomes:

`panaroo -i ./*.gff -o panaroo_alig/ -t 1 -a core --clean-mode moderate`

We identified <it GCA_003240915.1_ASM324091v1_genomic \it> as being a contaminant (see contamination MDS plot) and removed it from the analyses

We ran the main analyses and generated a core genome alignment for pylogenetic analyses:

`panaroo -i *.gff -o ./results_clean/ --clean-mode strict -a core --aligner clustal --core_threshold 0.98 -t 1`

We perform a pangenome search with a strict setting:

`panaroo -i *.gff -o results_strict/ --clean-mode strict`

We infer the phylogenetic relationships among the isolates using RAXML under a Multigamma subdstitution model

`raxmlHPC -p 12345 -x 12345 -# 100 -m MULTIGAMMA -s core_alignment.fasta -K GTR -n T1`

or

`iqtree -s core_gene_alignment.fasta.phy -m MFP+MERGE -bb 1000 -nt AUTO`

Meanwhile, we also estimated the changes in the pangenome using an infinite gene model (IGM):


