---
layout: page
title: 'The HINGE Assembler'
---


# HINGE  
Software accompanying  "HINGE: Long-Read Assembly Achieves Optimal Repeat Resolution"

- [Preprint](http://biorxiv.org/content/early/2017/03/16/062117)

- [Paper](http://genome.cshlp.org/content/early/2017/03/20/gr.216465.116.abstract)

- [Software](https://github.com/HingeAssembler/HINGE)

- An ipython notebook to reproduce results in the paper can be found in this [repository](https://github.com/govinda-kamath/HINGE-analyses).

- [Quick Start Guide](/index1quickstart)

CI Status: ![image](https://travis-ci.org/HingeAssembler/HINGE.svg?branch=master)





## Introduction

HINGE is a long read assembler based on an idea called _hinging_.

## Pipeline Overview

HINGE is an OLC(Overlap-Layout-Consensus) assembler. The idea of the pipeline is shown below.

![image](assets/High_level_overview.png)

At a high level, the algorithm can be thought of a variation of the classical greedy algorithm.
The main difference with the greedy algorithm is that rather than each read having a single successor,
and a single predecessor, we allow a small subset of reads to have a higher number of successors/predecessors.
This subset is identified by a process called _hinging_. This helps us to recover the graph structure
directly during assembly.

Another significant difference from HGAP or Falcon pipeline is that it does not have a pre-assembly or read correction step.



## Algorithm Details

### Reads filtering
Reads filtering filters reads that have long chimer in the middle, and short reads.
Reads which can have higher number of predecessors/successors are also identified there.
This is implemented in `filter/filter.cpp`

### Layout
The layout is implemented in `layout/hinging.cpp`. It is done by a variant of the greedy algorithm.

The graph output by the layout stage  is post-processed by running `scripts/pruning_and_clipping.py`.
One output is a graphml file which is the graph representation of the backbone.
This removes dead ends and Z-structures from the graph enabling easy condensation.
It can be analyzed and visualized, etc.



# Results:

For ecoli 160X dataset,  after shortening reads to have a mean length of 3500 (with a variance of 1500), the graph is preserved.


![image](assets/ecoli_shortened.png)

Results on the bacterial genomes of the [NCTC 3000](http://www.sanger.ac.uk/resources/downloads/bacteria/nctc/) project can be found at [web.stanford.edu/~gkamath/NCTC/report.html](https://web.stanford.edu/~gkamath/NCTC/report.html)
