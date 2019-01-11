# *De Novo* Assembly of Next Gen Sequence Data

[Prof Murray Cox](https://www.genomicus.com)<br>
School of Fundamental Sciences

**In this lab, you will gain experience in taking fragmentary next generation sequencing reads and joining them together to create complete genomic sequences.**

## Background

Sequencing costs have reduced by orders of magnitude in recent years. The first human genome took years to produce, cost billions of dollars and required a collaborative effort from thousands of scientists. A little over ten years later, one research scientist can sequence a human genome in a couple of weeks for a little over a thousand dollars.

As sequencing costs have dropped, a large number of organisms have been sequenced. Some of these species are commercially important (cows and corn), others are medically relevant (HIV and hepatitis), while others are interesting for less pragmatic reasons (pandas and poodles). Access to genome sequences is now seen as critical for many research questions, industry goals and government policy decisions. Consequently, these technologies are being adopted rapidly. You can expect to encounter next generation sequencing in many future jobs – regardless of whether you go into academia, industry or government – or just as a private individual.

In this module, you will learn how to assemble a dataset of short next generation sequences. Your goal is to determine which gene and organism a set of sequences come from.

## UNIX Basics

Sequence assembly is tricky – it is mathematically complex and computationally expensive. Therefore, assembly is typically performed on large UNIX clusters using command line programs. This is the sort of environment we will be using today. 

You have already covered basic UNIX methods in module 1. If you are comfortable with the UNIX command line, go straight to the tasks for module 5 further down in this document. If you want a quick refresher, look at the resources here.

## Next Generation Sequences

Sequences from next generation technologies come in a lot of different flavors. We will be using sequences generated on the [Illumina platform](http://www.illumina.com/applications/sequencing.html), the dominant one in use today.

Next generation sequences are usually found in a format called [FASTQ](https://en.wikipedia.org/wiki/FASTQ_format), where the ‘Q’ stands for ‘quality’.  The entry for a single read would typically look something like this:

```
@ME8432_1
CTTCTGCTTCAATGGGCGAAAGACCCAATGACCTCATCAC
+ME8432_1
hhhgh`\ahQhEhhhhhhheOhhQQVhefhhLUheYShhE
```

Each sequence entry consists of four lines. The first line always starts with ‘@’ and is followed by a unique read identifier (here, ME8432_1). The second line lists the actual DNA (or RNA) sequence. The third line always starts with ‘+’, optionally followed by the unique identifier again. The fourth line lists the quality of each base along the sequence in a fairly esoteric ASCII encoding. You can read more about this quality encoding in this paper:

Cock, P. J. A., C. J. Fields, N. Goto, M. L. Heuer and P. M. Rice. 2010. [The Sanger FASTQ file format for sequences with quality scores, and the Solexa/Illumina FASTQ variants](https://doi.org/10.1093/nar/gkp1137). *Nucleic Acids Research* 38:1767--1771.

XXX - paper header

FASTQ files are typically large. A standard Illumina run produces up to eight lanes of data, each with >300 million different sequence reads. Files for each of these lanes would usually be at least 50 gigabytes in size – more than twice the size of the extended ‘Lord of the Rings’ DVD box set. File sizes are getting bigger all the time.

Assembling datasets this large is a major undertaking. You will be working with much smaller datasets in this class. However, you will be using real sequence data from real research projects. The assemblies you perform below are identical to those performed in actual research environments. The only differences are the file sizes and the shorter time these assemblies take to run.

## Assembly software

There are a lot of programs that perform *de novo* sequence assembly – joining up a number of short reads into a single, longer sequence. New programs are being developed all the time.  The one we're going to use here is an old standard, [Velvet](https://www.ebi.ac.uk/~zerbino/velvet/).

Daniel Zerbino and Ewen Birney wrote Velvet at the European Bioinformatics Institute in Cambridge, and published a description of its algorithm in 2008:

Zerbino, D. R. and E. Birney. 2008. [Velvet: Algorithms for de novo short read assembly using de Bruijn graphs](https://doi.org/10.1101/gr.074492.107). *Genome Research* 18:821--829.

XXX - paper header

Like many scientific programs, Velvet is open source – which means you can read the code if you want to – and freely available [on the web](https://www.ebi.ac.uk/~zerbino/velvet/).

Velvet is actually made up of two different programs that need to be run sequentially. These programs are called ```velveth``` and ```velvetg```.

A typical assembly job would look something like this:

```
velveth assembly_directory_21 21 -fastq -short input_file.fastq
velvetg assembly_directory_21
```

This means that the output files are placed in a new directory called *assembly_directory_21*, the k-mer size (we’ll talk about this later) is 21, the input format is FASTQ, the data are short reads (e.g., the Illumina technology or similar vs long read technologies like PacBio), and the input file is called ‘input_file.fastq’. Of course, the names of these files and directories must be changed to whatever is appropriate for your study!

The k-mer value is a vital parameter in *de novo* assembly. The k-mer can be any odd number from 1 to the length of the read. The k-mer is sometimes called the ‘word size’, and represents the length of the unit used to determine whether any two sequences ‘overlap’.

For instance:
```
Read 1:    GGCTAGAGGCTAGCTTCAATGGGCGAAAGACCC
Read 2:                GCTTCAATGGGCGAAAGACCCGAGAGAGCTGGCT
Assembly:  GGCTAGAGGCTAGCTTCAATGGGCGAAAGACCCGAGAGAGCTGGCT
```
In practice, assembly with Velvet is not carried out by checking whether two sequences actually overlap (a method used in so-called overlap assembly algorithms), but instead by using a branch of mathematics concerned with networks (de Bruijn assembly algorithms). We will cover this method in a lecture, and you can also find out more about this approach in these papers:

Zerbino, D. R. and E. Birney. 2008. [Velvet: Algorithms for de novo short read assembly using de Bruijn graphs](https://doi.org/10.1101/gr.074492.107). *Genome Research* 18:821--829.

## Assembly Example

Here are the steps needed to assemble a set of FASTQ sequences using Velvet. Note that you will be repeating this exercise on your own dataset later in the lab.

First, Make a new directory (say, ```example_assembly```):

```
mkdir example_assembly
cd example_assembly
ls
```

XXX - screenshot

Next, you need to choose a sequence dataset to assemble. To start off, let’s use the example dataset called [ME8432.fastq](example/ME8432.fastq). Move the example dataset into your new directory. 

Now, you need to get the programs.  Although there are better ways to set them up, for now just copy-and-paste the programs ‘velveth’ and ‘velvetg’ into this directory.  You can download these programs [here](code).  If you want to learn how to compile these programs for your own machine, [look here](compilation.html).

At this point, you are ready to perform an assembly with a chosen k-mer value. Let’s start with a k-mer of 21.

```
./velveth ME8432_21 21 -fastq -short ME8432.fastq
./velvetg ME8432_21
```

These particular commands should complete within seconds. For typical genome scale assemblies, they might take hours to days.

Look at the Velvet output. Do this by moving into the assembly directory and opening the files are there:

```
cd ME8432_21
ls
```

You should see a number of files, including *Graph*, *LastGraph*, *Log*, *PreGraph*, *Roadmaps*, *Sequences*, *contigs.fa* and *stats.txt*.  Take a look at some of these files using the command ```cat``` or similar.

XXX - screenshot showing files

The main file, *stats.txt*, contains almost all the information we need about the assembly. You should mostly use this file for your analyses below. For the example dataset, it should look something like this:

```
ID    lgth    out    in    …
1    329    0    0    …
```

This particular file only contains two lines: a header line and a single entry on the next row. The most interesting columns are the first and second. The first column gives the ID of the assembled fragment (i.e., contig 1). The second column gives the length of this assembled fragment (here, 329 nucleotides). 

For many datasets, you will see a number of assembled fragments rather than just one. Some of these assembled fragments may be very small (even, in extreme cases, just 1 nucleotide).

The actual assembled sequences are contained in the file *contigs.fa*. The assembled fragments will look something like this:

```
>NODE_1_length_329_cov_16.844984
GTCCTCCAATCTTACCGAAGAACAAATTGCTGAATTCAAAGAAGCCTTTGCCCTCTTTGA
TAAAGATAACAATGGCTCTATCTCATCAAGTGAATTGGCCACTGTGATGAGGTCATTGGG
TCTTTCGCCCAGTGAAGCAGAAGTAAATGATTTGATGAACGAAATAGATGTTGATGGTAA
CCATCAAATCGAATTTAGTGAATTTTTGGCTCTGATGTCTCGTCAACTCAAATCAAATGA
CTCTGAACAAGAACTACTAGAAGCTTTTAAAGTATTCGATAAGAACGGTGATGGTTTAAT
CTCCGCCGCTGAGTTGAAACACGTGCTACCATCCATTGGTGAAAAATTG
```

This assembled sequence has an ID number of 1 and is 329 nucleotides long – just as we saw above. Since the input reads were only 40 nucleotides long, the assembly process has clearly worked – at least to some extent. (Note that poorly assembled contigs – say, of very small size – may appear in the ‘stats.txt’ file, but the program often exclude them from the ‘contigs.fa’ file).
Finally, we want to find out what organism this sequence comes from and what gene it represents. We will use [BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi) and the [GenBank database](https://www.ncbi.nlm.nih.gov/genbank/). 

At the [BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi) website, choose ‘nucleotide blast’. On the following page, paste your sequence into the box. Under ‘Database’, select ‘Others (nr etc.)’. Under ‘Optimize for’, select ‘Optimize for: Somewhat similar sequences (blastn)’. Then click, the **BLAST** button.

You should soon see a screen that looks something like this (formatting may vary depending on website updates):

XXX - screenshot

The red lines (top) represent good blast hits. You are looking for the best match to a gene or mRNA sequence (as opposed to an entire genome, chromosome or clone), which in this case is to GenBank entry NM_001178457. This accession comes from Saccharomyces cerevisiae (Baker’s yeast), and matches the calmodulin gene, which codes for a calcium ion binding protein.

For some datasets, the top hits may be to entire genome sequences or other nucleotide fragments (e.g., chromosomes, bacterial artificial chromosomes or BACs, plasmids). You may have to look (well) down the BLAST list to identify what gene your sequence best matches to.

For fun, try blasting a single read from the original test input file ([ME8432.fastq](example/ME8432.fastq)). Does this differ from blasting your larger assembled fragment? If so, how? (*Hint*: the result may depend on the length of your reads, which varies from dataset to dataset).

## What Should I Do Now?

Your first challenge is to choose a dataset from the [FASTQ files provided](datasets) and perform *de novo* assembly using Velvet. Don’t use the example dataset ME8432.fastq and don’t choose the same dataset as your neighbor!  

Can you figure out what organism this dataset came from and what gene was sequenced? Remember to choose a new output directory name every time you run a new assembly – you don’t want to call the output directory ME8432_21 every time! Also, save your files if you want to use them at home. (All output files from Velvet are simple text files that you can open in any basic text editor, like TextEdit or Notepad).

Your second challenge is to explore how varying the k-mer value affects the quality of the sequence assembly. Do some k-mer values produce better assemblies, and if so, which ones? The general approach is to find the size of the largest assembled fragment under different values of the k-mer (I suggest running all values from 3 to the length of your reads, odd numbers only). Remember to specify a different k-mer value on the command line, and also change the output directory name (e.g., use something like ```ME8432_19``` if your dataset is ME8432 and your k-mer is 19).

Your third challenge is a very much a ‘stretch’ assignment. When done by hand, you need to run the same commands over and over again, except for changing the k-mer value. Can you figure out a process (i.e., an algorithm) to automate this? Represent your logic as pseudocode (a human readable explanation of what the process is). Taking this further, can you write a real script/program to automate the process? It is fine to use any language and take any approach. Hint: As a place to start, Google ‘bash’ and ‘for loop’. If you get stuck, ask, but I expect to see some real traction before I give out any more hints!

## Research Report

This exercise will be examined on the basis of a 4-5 page research report. You should include:

* Your name and Massey student ID
* Brief sections for introduction, methods, results, discussion and citations, including:

  1. The ID of your dataset (e.g., AB1234)
  2. The largest fragment that you assembled (i.e., the full DNA sequence)
  3. The species that you think your dataset came from (and your reasoning)
  4. The gene that you think your dataset came from (plus reasoning)
  5. A brief description of this gene (e.g., using information on GenBank and by Googling). What does it do? How does it work?
  6. Three graphs showing the effects of k-mer value on assembly quality (k-mer value versus number of contigs, k-mer value versus average contig length, and k-mer value versus maximum contig length), from a k-mer of 3 to the size of your input reads

* A copy of i) your pseudocode and ii) your script/program for automating the assembly process. 

> Note: the real code will be tested to check that it works!

See the grading criteria on Stream for more details on what you need to focus on. In short, the report should be a full and professional document. Make sure the layout is clear and check your spelling! (You will lose marks if I can’t find the information I’m looking for.) 

A proportion of your marks will be allocated to getting the right species and gene. Make sure that you are certain about this! Analyze a number of different assembled fragments if you are not sure, and ask for advice in class. 

Given your training in module 1 and later in this course, all graphs should be plotted in R.

However, note that most of your marks will be assigned for good scientific interpretation of the results and graphs. Make sure you explain why your graphs have the shape they do, and link this back to the de Bruijn assembly method. (That is, why does the assembly method produce the graphs you observe?) In particular, you will find that both small k-mers and large k-mers tend to produce poor assemblies – although for different reasons. From your knowledge of de Bruijn graphs and k-mer assembly, why do these two ‘zones’ of poor assembly occur?

Reports are due at the date and time announced in class (usually on the Wednesday of week 12 at noon). Preferably email an electronic copy as a PDF to <&#109;&#46;&#112;&#46;&#99;&#111;&#120;&#64;&#109;&#97;&#115;&#115;&#101;&#121;&#46;&#97;&#99;&#46;&#110;&#122;>. 

I will happily comment and advise on reports received after the due date, but late reports will not be awarded marks.

