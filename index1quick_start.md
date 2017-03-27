---
layout: page
title: 'HINGE Quick Start'
---

# Installation

## Dependencies
- g++ 4.9
- cmake 3.x
- libhdf5
- boost
- Python 2.7

The following python packages are necessary:

- numpy
- ujson
- configparser
- colormap
- easydev.tools

This software is still at prototype stage so it is not well packaged, however it is designed in a modular flavor so different combinations of methods can be tested.

Installing the software is very easy.

~~~~~~~~

git clone https://github.com/HingeAssembler/HINGE.git

cd HINGE

git submodule init

git submodule update

./utils/build.sh

~~~~~~~~
{: .language-shell}

# Running

In order to call the programs from anywhere, I suggest one export the directory of binary file to system environment, you can do that by using the script `setup.sh`. The parameters are initialised in `utils/nominal.ini`. The path to nominal.ini has to be specified to run the scripts.

A demo run for assembling the ecoli genome is the following:

~~~~~~~~~ shell
# Setup paths
source utils/setup.sh

# Setup destination directory
mkdir data/ecoli
cd data/ecoli
# reads.fasta should be in data/ecoli

# Run DAligner to get alignments
fasta2DB ecoli reads.fasta
DBsplit -x500 -s100 ecoli     
HPC.daligner -t5 ecoli | csh -v
# alternatively, you can put output of HPC.daligner to a bash file and edit it to support parallelisation


rm ecoli.*.ecoli.*
LAmerge ecoli.las ecoli.+([[:digit:]]).las
rm ecoli.*.las # we only need ecoli.las
DASqv -c100 ecoli ecoli.las

# Run filter

mkdir log

hinge filter --db ecoli --las \
ecoli.las -x ecoli --config <path-to-nominal.ini>

# Get maximal reads

hinge maximal --db ecoli --las \
ecoli.las -x ecoli --config <path-to-nominal.ini>

# Run layout

hinge layout --db ecoli --las ecoli.las \
 -x ecoli --config <path-to-nominal.ini> -o ecoli

# Run postprocessing

hinge clip ecoli.edges.hinges \
ecoli.hinge.list <identifier-of-run>


# get draft assembly

hinge draft-path <working directory> \
ecoli ecoli<identifier-of-run>.G2.graphml

hinge draft --db ecoli --las ecoli.las --prefix ecoli \
--config <path-to-nominal.ini> --out ecoli.draft


# get consensus assembly

hinge correct-head ecoli.draft.fasta \
 ecoli.draft.pb.fasta draft_map.txt

fasta2DB draft ecoli.draft.pb.fasta

HPC.daligner ecoli draft | zsh -v  

hinge consensus draft ecoli draft.ecoli.las \
ecoli.consensus.fasta <path-to-nominal.ini>

hinge gfa <working directory> \
 ecoli ecoli.consensus.fasta

#results should be in ecoli_consensus.gfa
~~~~~~~~~

## Parameters

In the pipeline described above, several programs load their parameters from a configuration file in the ini format.  All tunable parameters are described in [this document](/index2parameter_description/).

## Analysis of Results

### showing ground truth on graph
Some programs are for debugging and oberservation. For example, one can get the ground truth by mapping reads to reference and get `ecoli.ecoli.ref.las`.

This `las` file can be parsed to json file for other programs to use.

~~~~~~~~~
run_mapping.py ecoli ecoli.ref ecoli.ecoli.ref.las 1-$
~~~~~~~~~

In the prune step, if `ecoli.mapping.json` exists, the output `graphml` file will contain the information of ground truth.

### drawing alignment graphs and mapping graphs
Draw a read, for example 60947, and output figure to `sample` folder (need plus 1 as LAshow counts from 1):

~~~~~~~~~
draw2.py ecoli ecoli.las 60948 sample 100
~~~~~~~~~

Draw pileup on draft assembly, given a region represented as [start,end], which
is [3600000, 4500000] in this case.

~~~~~~~~~
draw2_pileup_region.py  3600000 4500000
~~~~~~~~~
