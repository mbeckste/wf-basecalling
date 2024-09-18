## Basecalling workflow for Oxford Nanopore reads with GPU support

This is a CloWM compatible version of the helper workflow for signal processing and primary data analysis of Oxford Nanopore Technologies' reads developed by [EPI2ME labs](https://labs.epi2me.io/).



### Introduction

In brief this workflow can be used to perform:

+ Basecalling of a directory of pod5 or fast5 signal data
+ Basecalling in Duplex mode
+ Modified basecalling
+ Output basecalled sequences in various formats: FASTQ, CRAM or Unaligned BAM
+ If a reference is provided a sorted and indexed BAM or CRAM will be output
for basecalling a directory of `pod5` or `fast5` signal data with `dorado`
and aligning it with `minimap2` to produce a sorted, indexed CRAM.



### Pipeline overview

#### 1. Prerequisites

The workflow uses [Dorado](https://github.com/nanoporetech/dorado) for basecalling which includes the use of [Remora](https://github.com/nanoporetech/remora) for modified basecalling.
Basecalling with `Dorado` requires an NVIDIA GPU with [Pascal architecture or newer](https://www.nvidia.com/en-gb/technologies/) and at least 8 GB of vRAM. These requirements are fullfilled on the CloWM plattform which is powered by de.NBI cloud resources.


#### 2. Choosing a model

To select the relevant model see the `dorado` repository for a [table of available models](https://github.com/nanoporetech/dorado#available-basecalling-models) to select for `--basecaller_cfg` and `--remora_cfg`.

#### 3. Aligning to a reference

The workflow can optionally perform the alignment of the basecalled data using [minimap2](https://github.com/lh3/minimap2) to a reference of choice, provided with the `--ref` option.

#### 4. Duplex calling

wf-basecalling supports [duplex calling](https://github.com/nanoporetech/dorado#duplex), which is enabled with the `--duplex` option. If you used a chemistry and flowcell combination that supported duplex reads, you should switch this option on. The resulting BAM/CRAM will quality filtered and then automatically split in separate BAM/CRAM files for the simplex and duplex reads.
Since `dorado duplex` requires the inputs to be in `pod5` format, the workflow will perform the conversion automatically using [pod5 convert fast5](https://github.com/nanoporetech/pod5-file-format/blob/master/python/pod5/README.md#pod5-convert-fast5). These files are normally deleted upon completion of the analysis, but can optionally be saved by the user by providing the `--output_pod5` option.



### Compute requirements

Recommended requirements (these are automatically fullfilled on the CloWM plattform):

+ CPUs = 64
+ Memory = 256GB

Minimum requirements:

+ CPUs = 8
+ Memory = 64GB

Approximate run time: Variable depending on coverage, genome size, model of choice and GPU model.


### Run the workflow

A demo dataset for testing of the workflow is provided. It can be directly used together with preset default parameters by pressing the `Try it out` button in the workflow parameter form.




### Related protocols

<!---Hyperlinks to any related protocols that are directly related to this workflow, check the community for any such protocols.--->

This workflow is designed to take input sequences that have been produced from [Oxford Nanopore Technologies](https://nanoporetech.com/) devices.

Find related protocols in the [Nanopore community](https://community.nanoporetech.com/docs/).



### Input example

This workflow accepts a folder containing FAST5 or POD5 files as input.
The folder may contain other folders of FAST5 or POD5 files and all files will be processed by the workflow.
Please use the `Try it out` button in the workflow parameter form for a workflow execution with example data.




### Troubleshooting

* Duplex mode with wf-basecalling is reliant on internal optimisations to organise input files for better duplex rates, which is not possible when using streaming basecalling; therefore duplex combined with the `--watch_path` option could lead to lower duplex rates than what would be achieved running the algorithm after sequencing is completed.



### FAQ's

If your question is not answered here, please contact the workflow maintainer or report any issues or suggestions on the [github issues](https://github.com/epi2me-labs/wf-basecalling/issues) page or start a discussion on the [community](https://community.nanoporetech.com/).



### Further information

See the [EPI2ME website](https://labs.epi2me.io/) for lots of other resources and blog posts.
