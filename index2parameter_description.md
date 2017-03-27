---
layout: page
title: 'Parameters used by HINGE'
---

All the parameters below can be set using the .ini file read by the HINGE programs.



### Filter
- *length_threshold = 1000;*{: style="color: red"}  Minimum read length
- *aln_threshold = 2500;*{: style="color: red"} Minimum alignment length between two reads to be considered when building graph
- *min_cov = 5;*{: style="color: red"}  Minimum coverage depth for a segment on a read to not be considered erroneous/chimeric
- *cut_off = 300;*{: style="color: red"}   When looking for chimeric segments, we look for coverage gaps on a read, after reducing all matches by cut_off in the beginning and in the end
- *theta = 300;*{: style="color: red"}   When classifying a match between two reads as a right/left overlap, internal match, etc., overhangs of length up to theta are ignored
- *use_qv = true;*{: style="color: red"}   Use qv scores provided by DAligner when creating the read masks (i.e., the part of the read that will actually be used for assembly)
- *coverage = true;*{: style="color: red"}   Use coverage values when creating the read masks. If both use_qv and coverage are set to true, an intersection of the two masks is taken.
- *coverage_frac_repeat_annotation = 3;*{: style="color: red"}
- *min_repeat_annotation_threshold = 10;*{: style="color: red"}
- *max_repeat_annotation_threshold = 20;*{: style="color: red"}

  A repeat annotation is placed on the read at position i+reso if

~~~~~~~~
 |coverage[i]-coverage[i+reso]| > min( max( coverage[i+reso]/coverage_frac_repeat_annotation, min_repeat_annotation_threshold), max_repeat_annotation_threshold)
~~~~~~~~

- *repeat_annotation_gap_threshold = 300;*{: style="color: red"}   How far two hinges of the same type can be on a read
- *no_hinge_region = 500;*{: style="color: red"}   Hinges cannot be placed within no_hinge_region of the start and end of the read
- *hinge_min_support = 7;*{: style="color: red"}   Minimum number of reads that have to start in a `reso` (default 40) length interval to be considered in hinge calling
- *hinge_unbridged = 6;*{: style="color: red"}   Number of reads that one has to see before a pileup to declare a potential hinge unbridged
- *hinge_bin = 100;*{: style="color: red"}   Physical length of the bins considered
- *hinge_tolerance_length = 100;*{: style="color: red"}   Matches starting within hinge_tolerance_length of a hinge are considered to be starting at the hinge

<!--- *quality_threshold = 0.23;*{: style="color: red"}   Quality threshold for edges to be considered in the backbone -->
<!--- *n_iter = 2;*{: style="color: red"}   iterations of filtering, the filtering needs several iterations, because when filter reads, you got rid of some edges;*{: style="color: red"} when filter edges, you got rid of some reads (if the last edge is filtered.) Typically 2-3 iterations will be enough.-->
<!--- *theta2 = 0;*{: style="color: red"}   When classifying a match between two reads as a right/left overlap, internal match, etc., an overhang must have length at least theta2 for the match to be seen as internal -->


### Running
- *n_proc = 12;*{: style="color: red"}   number of CPUs for layout step




### Layout

- *hinge_tolerance = 150;*{: style="color: red"}   This is how far an overlap must start from a hinge to be considered an internal overlap.
- *hinge_slack = 1000;*{: style="color: red"}   This is the amount by which  a forward overlap must be longer than a forward internal overlap to be preferred while building a graph.

- *matching_hinge_slack = 200;*{: style="color: red"}   We identify two in-hinges (out-hinges) on two different reads as corresponding to the same repeat event, if the reads match in the repeat part, and the two hinges are within matching_hinge_slack of each other
- *min_connected_component_size = 8;*{: style="color: red"}   In order to actually add a hinge to a read, we require that at least min_connected_component_size reads have a repeat annotation and they are all identified as the beginning (or end) of the same repeat
- *kill_hinge_overlap = 300;*{: style="color: red"}
- *kill_hinge_internal = 40;*{: style="color: red"}

  When filtering hinges (so that only one in-hinge and one out-hinge are left for each reapeat), we kill an in-hinge (out-hinge) if there is a forward (backward) extension read that starts at least kill_hinge_overlap before (after) the hinge,
or if there is a forward_internal (backward_internal) extension read that starts at most kill_hinge_internal after (before) the hinge, as illustrated below.

![image](/assets/param_description1.png)

- *num_events_telomere = 7;*{: style="color: red"}
- *del_telomeres = 0;*{: style="color: red"}   If set to 1, any read with more than num_events_telomere repeat annotations will be classified as a telomere read and will be deleted.
- *aggressive_pruning = 0;*{: style="color: red"}  If set to 1, the pruning will be more aggressive. We recommend it be set to 1 for large
genome.
- *use_two_matches = 1;*{: style="color: red"}   Allow the HINGE algorithm to consider the top two matches between a pair of reads (as opposed to just the longest match)


### Draft
<!--- *min_cov = 10;*{: style="color: red"}  obsolete-->
<!--- *trim = 200;*{: style="color: red"}  obsolete-->
<!--- *edge_safe = 100;*{: style="color: red"}  obsolete-->
- *tspace = 900;*{: style="color: red"}  space between new "trace points"
- *step = 50;*{: style="color: red"}



### Consensus

- *min_length = 4000;*{: style="color: red"}   Minimal length of reads used for final consensus
- *trim_end = 200;*{: style="color: red"}   Trim ends for alignments for final consensus
- *best_n = 1;*{: style="color: red"}   If one read has multiple alignments with the backbone assembly, choose the longest n segments for consensus.
- *quality_threshold = 0.23;*{: style="color: red"}   alignment quality threshold
