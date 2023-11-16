# RNA-seq analysis of E. coli with over-expressed DNA-damage handling proteins

## Introduction

In this project, you will be analysing RNA-seq data from Escherichia coli bacteria.
These bacteria have had some DNA-damage handling proteins over-expressed, and you
would like to perform a transcriptome-wide study of its effects.

## Setting up your environment

### Conda

You should create a new, empty conda environment. Make sure you have set up 
Bioconda to be able to install the necessary packages. [See this link for details
on setting up conda and bioconda](http://bioconda.github.io/).

### Git repository

Your working directory will be under git control. You will first copy this repository
to your GitHub account, and then work on this new copy using it as your remote.

#### Forking this repository

You should start by forking this repository. A **fork** creates a copy
of the repository *on your own GitHub account*. This copy will become the remote
repository where you will eventually save your work.

You can find the fork button in the main page of your GitHub repository.

Once you fork the project, you will have a copy of the repository on a new URL:

`https://github.com/<your_username>/testsim`

#### Adding your instructor as a collaborator

You should now add your instructor as a collaborator in your copy of the repository,
so that she/he can interact with you during the development. You can do this by adding
his username in the "Members" section on vertical menu bar on the left of the project
page.

#### Getting help

You can now ask for help from your instructor by creating a new issue in the GitHub
repository (click on the "Issues" tab on the left of the GitHub repo page),
adding a relevant title and description, **and assigning the issue to your instructor**.

You can assign the issue by clicking on the "Assignees" title on the top right.

> Make sure to check that your instructor appears on the list of possible assignees.

#### Cloning your fork and starting work

You should now **clone** your new repository to obtain a local copy on your machine.

You can then start working on your local copy of the repository.

> Remember to commit often. Don't go crazy about it, but do generate a history of your work.

### Organisation of your files

The repository already contains a folder structure to help you organise your files.
It also contains the data files for three different E. coli samples, and the pipeline
script you wrote in class that runs cutadapt, the star indexing, and the star alignment.

> You may need to create extra directories to store some files.

## Your tasks

Your job is to develop a pipeline to analyse this data by doing some quality checks
on the reads, removing sequencing adapters, aligning the adapter-free reads to the
E. coli genome, and generating a report about the whole process.

When complete, your pipeline should be able to automatically do the following (list not necessarily in order):

> Remember to activate your conda environment and to always check that you are in the root of your working directory.

- Download the E. coli genome
    - You can get if from
    `ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/005/845/GCF_000005845.2_ASM584v2/GCF_000005845.2_ASM584v2_genomic.fna.gz`
- QC the data using the FastQC software (`fastqc` in conda)
- Remove the adapters from the data (`cutadapt` in conda)
- Index the genome (`star` in conda)
- Align the adapter-free reads to the genome (also with star)
- Generate a report with MultiQC (multiqc in `conda`)

> Your scripts should automatically process all samples available in the `data` directory.

> Keep in mind that you should only repeat for each sample those commands that
> analyse samples. There are other, generic commands that should only be run once.

> **This may require modifying the provided sample-analysing script and moving part of it somewhere else.**

## Once you are done

Once you are done, you should first export a file with your conda environment information.

```shell
mkdir envs
conda env export > envs/rna-seq.yaml
```

You should **add** the new file to the staging area, **commit** it to the local repository,
and **push** your changes to the remote repository.

## Hints

### Organising your scripts

You can structure your program as you like. Here's an example for a parent
script that will call a sample-analysing script for each sample:

```shell
# place here any commands that need to be run before analysing the samples
for sid in $(<list of samples>)
do
   # place here the script with commands to analyse each sample
   # this command should receive the sample ID as the only argument
done
# place here any commands that need to run after analysing the samples
```

### Getting the list of samples

You can obtain the list of samples from the sample files with this command:

```shell
ls data/*.fastq.gz | cut -d "_" -f1 | sed 's:data/::' | sort | uniq
# Bonus brownie points if you replace the "sed" with something else.
```

### Creating directories

You can use the "-p" argument of "mkdir" to create a directory if
it doesn't exist, and do nothing if it does. This is useful so that there
are no errors when running a script many times.

### Monitoring the execution of your pipeline

You may want to redirect the output from your main script to a file, so you can see what worked
and what not. You can redirect **both** stdout and stderr like this:

`bash run_pipeline.sh &> log/run_pipeline.out`

By running that command you will see nothing on screen, so if you want to monitor the progress of
the pipeline you can see the output of the log file by running this command **on a new terminal**:

`tail -f log/run_pipeline.out`

### Final state of your working directory

When you are done, your working directory should look something like this:

> This is just to guide you, it doesn't need to be the exact same.

```shell
.
├── data
│   ├── ERR2868172_1.fastq.gz
│   ├── ERR2868172_2.fastq.gz
│   ├── ERR2868173_1.fastq.gz
│   ├── ERR2868173_2.fastq.gz
│   ├── ERR2868174_1.fastq.gz
│   └── ERR2868174_2.fastq.gz
├── envs
│   └── rna-seq.yaml
├── log
│   ├── cutadapt
│   │   ├── ERR2868172.log
│   │   ├── ERR2868173.log
│   │   └── ERR2868174.log
│   └── run_pipeline.out
├── out
│   ├── cutadapt
│   │   ├── ERR2868172_1.trimmed.fastq.gz
│   │   ├── ERR2868172_2.trimmed.fastq.gz
│   │   ├── ERR2868173_1.trimmed.fastq.gz
│   │   ├── ERR2868173_2.trimmed.fastq.gz
│   │   ├── ERR2868174_1.trimmed.fastq.gz
│   │   └── ERR2868174_2.trimmed.fastq.gz
│   ├── fastqc
│   │   ├── ERR2868172_1_fastqc.html
│   │   ├── ERR2868172_1_fastqc.zip
│   │   ├── ERR2868172_2_fastqc.html
│   │   ├── ERR2868172_2_fastqc.zip
│   │   ├── ERR2868173_1_fastqc.html
│   │   ├── ERR2868173_1_fastqc.zip
│   │   ├── ERR2868173_2_fastqc.html
│   │   ├── ERR2868173_2_fastqc.zip
│   │   ├── ERR2868174_1_fastqc.html
│   │   ├── ERR2868174_1_fastqc.zip
│   │   ├── ERR2868174_2_fastqc.html
│   │   └── ERR2868174_2_fastqc.zip
│   ├── multiqc
│   │   ├── multiqc_data
│   │   │   ├── multiqc_cutadapt.txt
│   │   │   ├── multiqc_data.json
│   │   │   ├── multiqc_fastqc.txt
│   │   │   ├── multiqc_general_stats.txt
│   │   │   ├── multiqc.log
│   │   │   ├── multiqc_sources.txt
│   │   │   └── multiqc_star.txt
│   │   └── multiqc_report.html
│   └── star
│       ├── ERR2868172
│       │   ├── Aligned.out.sam
│       │   ├── Log.final.out
│       │   ├── Log.out
│       │   ├── Log.progress.out
│       │   └── SJ.out.tab
│       ├── ERR2868173
│       │   ├── Aligned.out.sam
│       │   ├── Log.final.out
│       │   ├── Log.out
│       │   ├── Log.progress.out
│       │   └── SJ.out.tab
│       └── ERR2868174
│           ├── Aligned.out.sam
│           ├── Log.final.out
│           ├── Log.out
│           ├── Log.progress.out
│           └── SJ.out.tab
├── README.md
├── res
│   └── genome
│       ├── ecoli.fasta
│       └── star_index
│           ├── chrLength.txt
│           ├── chrNameLength.txt
│           ├── chrName.txt
│           ├── chrStart.txt
│           ├── Genome
│           ├── genomeParameters.txt
│           ├── SA
│           └── SAindex
└── scripts
    ├── analyse_sample.sh
    └── run_pipeline.sh
```
