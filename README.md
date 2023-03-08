# minigraph-cactus-nf

A nextflow pipeline for running minigraph-cactus

## Introduction
[minigraph-cactus][mgc] is a pipeline for making a pangenome graph out of
assemblies of genomes with too much shared variation for a phylogeny to make
sense, e.g., a set of assemblies of different individuals in the same species.
This nextflow pipeline automates it into a single command.

## Setup
This pipeline uses [nextflow][nf]. To learn more about how to run nextflow
pipelines like this one, check out [this quick introduction][warren-nf].

## Running
You just need a `seqFile` containing a list of paths to genome assemblies to
run this pipeline, and to specify one of them as the reference. See
[here][seqfile] for the format of the seqFile.

```bash
nextflow run WarrenLab/minigraph-cactus-nf \
    --seqFile [seqFile] \
    --reference [refname]
```

If it gets interrupted, you can have it resume where it left off with the
`-resume` flag. It will put the final output in `out/pangenome/`. Read the
[docs][mgc] to learn about the different files that are output.

### Arguments reference
#### Mandatory
* `--seqFile`: a tab-separated file where the first column is the name of a
  sequence and the second column is the path to that sequence. See cactus docs.
* `--reference`: the name of one of the sequences to use as a backbone

#### Optional
* `--maxAlignLength`: maximum alignment length for `cactus-align-batch` step.
  If not specified, a default of 10000 is used based on cactus documentation.
* `--chromosomesFile`: a text file containing chromosome names, one per line,
  that are present in the sequence specified as the reference. This can be
  useful because if the reference has lots of unplaced sequences, cactus spawns
  a set of jobs for each one, no matter how small. If you give it a list of
  chromosome names, it will instead run all of the unplaced sequence together
  as a single set of jobs.
* `--finalReference`: sometimes the best assembly you have isn't the one you
  actually want to use as a reference downstream, e.g., because it isn't
  annotated. If this is the case, you can specify the best assembly with
  `--reference` so that it gets used as the alignment backbone, and then
  specify the assembly you actually want to use as a reference downstream with
  `--finalReference`.

[mgc]: <https://github.com/ComparativeGenomicsToolkit/cactus/blob/master/doc/pangenome.md>
[nf]: <https://www.nextflow.io/>
[warren-nf]: <https://github.com/WarrenLab/docs/blob/main/nextflow.md>
[seqfile]: <https://github.com/ComparativeGenomicsToolkit/cactus/blob/master/doc/pangenome.md#interface>
