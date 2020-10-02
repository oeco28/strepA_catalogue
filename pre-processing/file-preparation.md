This is a set of commands perfomed to prepare the files to build the catalog

1. gbff files were downloaded from the website: https://www.ncbi.nlm.nih.gov/assembly/?term=streptococcus%20pyogenes
2. Gbff files were decompressed and transformed into gbk files using ugene

`for i in `cat list`; do gunzip $i.gbff.gz ; ugene convert-seq --in=$i.gbff --out=genbank/$i.gbk --format=genbank ; done`

We changed strategy while performing some of the analyses and decided to run PIRATE and Panaroo as methods to identify and characterize the pangenome of the species. 

For this, we downloaded all fasta sequences for all assemblies and re-annotate it with Prokka.
 
 `for i in `cat list`; do gunzip $i.fna.gz ; /media/venezuela7/repository_software_recent/prokka/bin/prokka --outdir Prokka_$i $i.fna ; done`
 
Once we re-annotated the genomes we moved all gff files to a separate folder for analyses:

`for i in `cat list`; do cp Prokka_$i/*gff gff_files/$i.gff ; done`

And then we ran PIRATE on the new set of annotated genomes:

`PIRATE -i gff_files/ -s "50,70,90,95" -o pirate_results/ -a -r`
