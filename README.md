# HealthHack 2016
## Eliminating errors from high-throughput sequencing data
Sequencing of entire human genomes is now readily available and
this technology is used extensively to study the contribution
of genetic variation to a wide variety of diseases. The
identification of inherited genetic variants carried by an
individual has become a relatively routine task.  

However, not all genetic variation is inherited. Each
individual accumulates a large number of mutations throughout
their life, with different cells carrying different sets of
mutations. While most of these have no noticeable impact
some cause disease. Since these mutations are only present in 
a small fraction of cells they are a lot harder to identify.
Not only is it necessary to generate more data to be able
to observe these, potentially very rare, mutations, the
presence of errors in the data limits our ability to 
detect disease causing mutations.

The aim of this project is to post-process high-throughput
sequencing data to identify and correct errors in the sequences.
This will improve the signal-to-noise ratio and lower the 
threshold for the frequency required to reliably detect mutations.
Ultimately this will not only lead to a better understanding of 
disease but also has the potential to enable better targeted
treatments for patients.

## Available data
Data from a deep sequencing experiment is available. This consists
of high coverage data for a panel of 163 genes. Each individual molecule
has been tagged with a unique identifier. These UIDs are part of the
sequencing data obtained from the sample and can be used to identify
sequence reads that originate from the same molecule
(see [documentation](docker/data/assembled/README.md) for details).

## Development environment
A [Docker image](https://hub.docker.com/r/humburg/healthhack-2016/)
containing software tools that are likely to be useful
for this project is available. See the accompanying
[documentation](docker/README.md) for further details.  
