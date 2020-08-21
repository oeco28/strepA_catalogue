#This is a set of commands perfomed to prepare the files to build the catalog

1. gbff files were downloaded from the website: https://www.ncbi.nlm.nih.gov/assembly/?term=streptococcus%20pyogenes
2. Gbff files were decompressed and transformed into gbk files using ugene

`for i in `cat list`; do gunzip $i.gbff.gz ; ugene convert-seq --in=$i.gbff --out=genbank/$i.gbk --format=genbank ; done`

