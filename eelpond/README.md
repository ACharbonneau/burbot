# Burbot RNAseq analysis using eelpond

We are going to use [eelpond](https://dib-lab.github.io/eelpond/) to build an annotated transcriptome from Burbot RNAseq data, and then map our reads back to that transcriptome for quantification and analysis.
Building a transcriptome from all 40 samples would take a great deal of time and compute power. Further, after a certain point, adding more data simply introduces more error and makes the assembly more difficult rather than better. As such, we're going to do this analysis in two parts. First, we'll build and annotate a transcriptome using a subset of our samples, then we'll run a pipeline to align all the samples to it.

This is the overall workflow for this project:

1. Trim reads and assemble a subset of four individuals using trimmomatic, trinity
2 Annotate using dammit (if you use some number other than 4 individuals, change line 35 of 1_burbot_assemble.yaml to the right number)
3. Trim all reads to prep for Salmon via eelpond
4. Quantify expression levels using Salmon via eelpond
5. Determine differentially expressed genes using DESeq2 via eelpond

This should only require running two overall workflows though, one for the subset of data, and one for the overall data.

## Detailed steps and notes

### 1 Trim, assemble, annotate

This requires two files: my_transcriptome_samples.tsv and 1_burbot_assemble.yaml 
 - my_transcriptome_samples.tsv:  a tab separated file that contains all the relevant information for the subset of samples that will be used in the assembly
 - 1_burbot_assemble.yaml: a yaml file detailing the programs to be used and the specific parameters for each program
 
#### To run

You should probably run this in a screen session so it can run while you're away:

`screen`
`cd eelpond/`
`conda activate eelpond`
`./run_eelpond 1_burbot_assemble.yaml assemble annotate`

then *briefly* hit: 'control' and 'r' at the same time, then the letter 'd' to get out of your screen session.

When you come back to check on it, you re-open your screen session by typing 

`screen -r`

### 2 Trim and prep all reads

This requires two files: my_transcriptome_samples.tsv and 1_burbot_assemble.yaml 
 - all_my_samples.tsv:  a tab separated file that contains all the relevant information for all of samples that will be used in the assembly
 - 2_burbot_quantify.yaml: a yaml file detailing the programs to be used and the specific parameters for each program

#### To run
./run_eelpond assemblyinput quantify diffexp
