# miRanalysis
Contains example script used to run the analysis in Donayo et al. This script was run on the Compute Canada, Calcul Quebec Briaree cluster. See https://wiki.calculquebec.ca/w/Ex%C3%A9cuter_une_t%C3%A2che/en for more information.

The mm10 reference genome can be downloaded from iGenomes at https://support.illumina.com/sequencing/sequencing_software/igenome.html.
mirdeep2 can be downloaded from https://www.mdc-berlin.de/8551903/en/.

### Steps to follow before running the script: 
To prepare for BOWTIE alignment using the mirbase hairpin.fa and mature.fa file
$ module add fastx_tools/0.0.13

You can download the hairpin.fa file from http://www.mirbase.org/ftp.shtml
$ cd ~/miRNA_reference
$ fasta_formatter -i hairpin.fa -o hairpin2.fa -w 0
$ fasta_formatter -i mature.fa -o mature2.fa -w 0
$ tr 'U' 'T' < hairpin2.fa > hairpindna2.fa

Convert U to T for bowtie
$ tr 'U' 'T' < hairpin.fa > hairpindna.fa

### Commands to convert mapped sam to fasta and run mapper.pl from mirdeep2
$ module add mirdeep2/2.0.0.5
$ cat OV_17-92Precursor_bowtie.sam | grep -v ^@  | awk '{print ">"$1" "$2"\n"$6}' > OV_17-92Pre.mapped.fa
$ mapper.pl OV_17-92Pre.mapped.fa -p /RQusagers/johnsonr/miRNA_reference/hairpindna -c -m -s OV_17-92Pre.mapped.fa.collapsed -t OV_17-92Pre.mapped.arf -n 

$ ~/mirdeep2_core-master/quantifier.pl -p ~/miRNA_reference/hairpindna.fa -m ~/miRNA_reference/mature.fa -r OV_17-92Pre.mapped.fa.collapsed -t mmu -y OV_17-92 -e 15 
