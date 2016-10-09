# Paired end RNA sequencing reads assembled with PEAR [![](https://images.microbadger.com/badges/version/humburg/jurkat-only-rna-assembled.svg)](https://hub.docker.com/r/humburg/jurkat-only-rna-assembled/ "Get image from Docker Hub")

Overlapping reads from each pair were merged into a single sequence with 
[PEAR](http://sco.h-its.org/exelixis/web/software/pear/doc.html) (v0.9.10). 
Each end of the resulting sequence contains an eight base barcode at each end. 
Together these 16 bases form a unique molecular identifier.  The barcode on each 
end is followed by four constant bases (GACT/AGTC). 
