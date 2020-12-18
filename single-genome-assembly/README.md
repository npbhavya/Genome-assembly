# Running genome assembly on deepthought Flinders University cluster
## For a single genome file sequenced on Nanopore MinION platform

Documentation on deepthought - https://deepthoughtdocs.flinders.edu.au/en/latest/index.html

## Genome assembly steps

**loading the environment**

  `module load Miniconda/3.0`

**create a new conda environment**

  `conda create -y -n genome-assembly` \
  `source activate genome-assembly`\
  `conda install -c bioconda unicycler`\
  `conda install -c bioconda filtlong`\
  `conda install -c conda-forge mamba`\
  `conda install -c conda-forge -c bioconda`
  
**Input files** 
Fastq read file (post basecalling)

**Steps run in the job script below**
- QC using filtlong - https://github.com/rrwick/Filtlong
- Assembly using Unicycler - https://github.com/rrwick/Unicycler

**wrote a job script and submitted to SLURM job scedule** 

**Output** 
The output if the script ran successfully, will generate an output directory with assembly.fasta and assembly.gfa files.

## Bandage plot
Download the assembly.gfa file- output from Unicycler output locally to visualize on Bandage- https://rrwick.github.io/Bandage/
Upload the assembly.gfa file to visulaize the genome assembled

## Assembly quality
Ideally, the assembly will generate complet, circular chromosomes along with complete, circular plasmid sequences if present in the genome sequenced. 
Number of contigs =Number of chromsomes in the genome +number of plasmid present in the genome 
Total length=Total genome length of species sequenced 

Finally long read technologies are error prone, which can annotate lots of pseudogenes due to frameshifts. To determine the quality of the genome assembly on this front, following http://www.opiniomics.org/a-simple-test-for-uncorrected-insertions-and-deletions-indels-in-bacterial-genomes/

**Setting up the script to test frameshifts

  `git clone https://github.com/mw55309/ideel.git`
  `source activate genome-assembly`
  `conda install -c bioconda prodigal`
  `conda install -c bioconda diamond`
  `conda install -c r r`
  `cd ideel`
  `#Download uniport database and format UniProt TREMBL and saved as uniprot_trembl.diamond.dmnd`
  
**Running the scripts**

**Input files** - place the assembly.fasta in the directory ideel/genomes
**Database** - place uniprot_trembl.diamond.dmnd in ideel/uniprot_trembl.diamond.dmnd

**Setting up job script to submit to SLURM job scheduler**

**Output**
In ideel/hists/<output filename>.png plots

Ideally - Should have a lot of proteins with 1:1 ratio on the histograms (one tall peek)and no other peaks at all
The other peaks represent proteins that were only partially aligned against the proteins in the reference database, and can be pseudogenes or genes with frameshifts. Detailed explanation in the blog mentioned above.




