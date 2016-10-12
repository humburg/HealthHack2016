# Docker image for HealthHack 2016 [![](https://images.microbadger.com/badges/version/humburg/healthhack-2016.svg)](https://hub.docker.com/r/humburg/healthhack-2016/ "View the image on DockerHub")

The image created from the [Docker file](Dockerfile) contains an environment suitable
for the development of deep sequencing data analysis solutions.

## Quick start
If you are familiar with using docker on your system this section provides a brief
summary of how to use the image. More detailed instructions can be found
[below](#using-the-image).

Create the data volume (see the accompanying [documentation](data/assembled/README.md)
for details on the data themselves):

```
docker create --name jurkat_assembled humburg/jurkat-only-rna-assembled
```

and start a container:

```
docker run -ti --volumes-from jurkat_assembled -v /path/to/source/code:/code humburg/healthhack-2016
```

The data files will then be located in `/data`.

## Using the image
The Docker image contains a number of software tools useful in the processing and analysis
of deep sequencing data [see below](#included-software) for an overview of what is available.
Data is distributed separately in data volumes that can be added to containers created from
this image.

### Setting up Docker
If you haven't done so already, start by installing 
[Docker](https://www.docker.com/products/overview). Binaries
are available for [Linux](https://docs.docker.com/engine/installation/linux/),
[OS X](https://download.docker.com/mac/stable/Docker.dmg)<sup name="a1">[1](#fn1)</sup> and 
[Windows](https://download.docker.com/win/stable/InstallDocker.msi)<sup name="a2">[2](#fn2)</sup>. 

#### Ensuring sufficient disk space
Docker images can take up a lot of space. To avoid unpleasant surprises ensure that
Docker images are stored on a partition with sufficient free space. This is especially
important if the OS is running from an SSD with very limited capacity as the default location
for images will usually be located there as well. The default location for images varies
by platform. On Linux it is `/var/lib/docker` and instructions on changing this can be found 
[here](https://forums.docker.com/t/how-do-i-change-the-docker-image-installation-directory/1169).
 On Windows and OS X all Docker content is stored in a virtual machine. Here it is the location
 of the machine image and size of the virtual hard disk that need to be managed. On Windows
 Docker uses Hyper-V and disk images can be 
 [moved via the Hyper-V Manager](https://technet.microsoft.com/en-au/library/cc708355(v=ws.10).aspx). More importantly, the size of the disk is capped at 60GB *and cannot be extended* without
 resetting the Docker installation (which will remove all images). To set this to a larger
 value edit `C:\Program Files\Docker\Docker\resources\MobyLinux.ps1` to change

 ```
$global:VhdSize = 60*1024*1024*1024  # 60GB
 ```

 to 

 ```
$global:VhdSize = 200*1024*1024*1024  # 200GB
 ```
 
 (for 200GB maximum size).

### Getting the data
Sequencing reads from a deep sequencing experiment with unique molecular identifiers (UMIs)
is available as a data volume from 
[DockerHub](https://hub.docker.com/r/humburg/jurkat-only-rna-assembled/).

```
docker create --name jurkat_assembled humburg/jurkat-only-rna-assembled
``` 

This can then be mounted into the main container:

```
docker run --rm -ti --volumes-from jurkat_assembled humburg/healthhack-2016
```

This adds a (gzipped) FASTQ file with the sequences of 16,485,876 read pairs
that have been merged into a single sequence for each pair with 
[PEAR](http://sco.h-its.org/exelixis/web/software/pear/doc.html).
See [here](data/assembled/README.md) for further details.

### Adding source code
For development purposes it is convenient to mount a directory from the
host system that contains the source code. This way any changes made to
the code are immediate available inside the container for testing.

Under Windows it is necessary to enable access to the relevant hard drive
first. This option is available under *Settings...* > *Shared Drives* from
the Docker for Windows context menu.

Use the `-v` option to specify the path to mount:

```
docker run -ti --volumes-from jurkat_assembled -v /path/to/source/code:/code humburg/healthhack-2016
```

This will make the contents of `/path/to/source/code` available inside the container
under the `/code` directory.

On Windows the path should look something like `D:/path/to/source/code` (if the code is located on drive `D:`).

## Included software
* [Python (3.5.2)](https://docs.python.org/3/whatsnew/3.5.html)
  including support for [Cython](http://cython.org/) and [numpy](http://www.numpy.org/).
  The [biopython](http://biopython.org/wiki/Documentation), 
  [pysam](http://pysam.readthedocs.io/en/latest/)
  and [editdistance](https://github.com/aflc/editdistance) modules may be of use.
  Support for unit testing is provided by the [nose](http://nose.readthedocs.io/en/latest/)
  and [coverage](https://coverage.readthedocs.io/en/coverage-4.2/) modules.
* [Java (openjdk 1.8.0_91)](http://www.oracle.com/technetwork/java/javase/overview/java8-2100321.html)
* [FastQC (0.11.5)](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/)
  A quality control tool for high throughput sequence data.
  *Usage:* `fastqc --help`
* [PEAR (0.9.10)](http://sco.h-its.org/exelixis/web/software/pear/doc.html) A tool 
  designed for the efficient and accurate merging of overlapping paired-end
  reads.
  *Usage:* `pear`
* [GSNAP (2016-09-23)](http://research-pub.gene.com/gmap/) 
  Aligns RNA-seq reads to a reference genome.
  *Usage:* `gsnap --help`
* [Samtools (1.3.1)](http://www.htslib.org/) A suite of programs for interacting with
  high-throughput sequencing data. Includes bcftools and HTSlib. 
  *Usage:* `samtools`
* [Picard (2.2.4)](https://broadinstitute.github.io/picard/) 
  A set of command line tools for manipulating high-throughput sequencing data
  and formats. 
  *Usage:* `picard -h`

<a name="fn1">1</a>: Requires OS X Yosemite 10.10.3 or above.
For previous versions get [Docker Toolbox](https://www.docker.com/products/docker-toolbox). [↩](#a1)

<a name="fn2">2</a>: Requires Microsoft Windows 10 Professional or Enterprise 64-bit. 
For previous versions get [Docker Toolbox](https://www.docker.com/products/docker-toolbox). [↩](#a2)
