Sometimes, highly similar amplicons get reported as a single collapsed amplicon - in July 2025, [amplicon_sorter](https://github.com/avierstr/amplicon_sorter) reports a "limit for separating closely related species within a sample around 95 - 96%". AmpliconSplitter splits these amplicons to recover all the amplicons. AmpliconSplitter is based on [HairSplitter](github.com/rolandfaure/hairsplitter)

# Installation

You can install AmpliconSplitter through conda `conda install -c bioconda ampliconsplitter`

## Manual installation
### Quick conda dependencies

The recommended way to manually install AmpliconSplitter is to create and activate a conda environment with all dependencies: 
```
conda create -c bioconda -c conda-forge -c anaconda -n ampliconsplitter cmake gxx gcc python scipy numpy minimap2 minigraph=0.20 racon "samtools>=1.16" raven-assembler openmp
conda activate ampliconsplitter

conda install -c bioconda -c conda-forge medaka #only if you specifically want to use medaka /!\ Very heavy installation
```
### List of dependencies

- [minimap2](https://github.com/lh3/minimap2)
- [racon](https://github.com/isovic/racon) and/or [medaka](https://github.com/nanoporetech/medaka)
- [samtools](www.htslib.org)
- [raven](github.com/lbcb-sci/raven)
- CMake >= 3.8.12, make, gcc >= 11, g++ >= 11
- Openmp
- Python3 with numpy and scipy
- gzip

If Minimap2, Racon, Medaka or samtools are not in the PATH, their location should be specified through the `--path-to-minimap2`, `--path-to-racon`, `path-to-medaka` or `--path-to-samtools` options.
 
## Download & Compilation

To download and compile, run
```
git clone https://github.com/RolandFaure/ampliconsplitter.git
cd ampliconsplitter/src/
mkdir build && cd build
cmake ..
make
cd ../../ && chmod +x ampliconsplitter.py
```

# Usage

## Quick start

Let's say `reads.fastq` (ONT reads) were used to build amplicons stored in `amplicons.fa` (with any assembler)(the assembly can be in gfa or fasta format). To recover all amplicons, run
```
python ampliconsplitter.py -f reads.fastq -r amplicons.fa -o ampliconsplitter_out/
```

In the folder ampliconsplitter\_out, you will find the new amplicons, named `ampliconsplitter_final_amplicons.fa`.

## Options

```bash
usage: ampliconsplitter.py [-h] -r REF -f FASTQ [-p POLISHER] [-t THREADS] -o OUTPUT [-u RESCUE_SNPS]
                           [-q MIN_READ_QUALITY] [--resume] [-P] [-F] [-l] [--no_clean]
                           [--path_to_medaka PATH_TO_MEDAKA] [--path_to_python PATH_TO_PYTHON]
                           [--path_to_raven PATH_TO_RAVEN] [-v] [-d]

options:
  -h, --help            show this help message and exit
  -r, --ref REF         Reference amplicon(s) to separate in several amplicon(s) (required)
  -f, --fastq FASTQ     Sequencing reads fastq or fasta (required)
  -p, --polisher POLISHER
                        {racon, medaka} medaka is more accurate but much slower [racon]
  -t, --threads THREADS
                        Number of threads [1]
  -o, --output OUTPUT   Output directory
  -u, --rescue_snps RESCUE_SNPS
                        Consider automatically as true all SNPs shared by proportion u of the reads [0.33]
  -q, --min-read-quality MIN_READ_QUALITY
                        If reads have an average quality below this threshold, filter out (fastq input
                        only) [0]
  --resume              Resume from a previous run
  -F, --force           Force overwrite of output folder if it exists
  -l, --low-memory      Turn on the low-memory mode (at the expense of speed)
  --no_clean            Don't clean the temporary files
  --path_to_medaka PATH_TO_MEDAKA
                        Path to the executable medaka [medaka]
  --path_to_python PATH_TO_PYTHON
                        Path to python [python]
  --path_to_raven PATH_TO_RAVEN
                        Path to raven [raven]
  -v, --version         Print version and exit
  -d, --debug           Debug mode
```

# Issues
 Most installation issues that we have seen yet stem from the use of too old compilers. ampliconsplitter has been developed using gcc=11.2.0. Sometimes the default version of the compiler is too old (especially on servers). Specify gcc versions manually to cmake using `-DCMAKE_CXX_COMPILER=/path/to/modern/g++` and `-DCMAKE_C_COMPILER=/path/to/modern/gcc`.
 
 <a name="work">
</a>
 
# Citation
 If you use AmpliconSplitter, please cite HairSplitter. HairSplitter is published in the Peer-Community Journal (PCJ): Faure, Roland; Lavenier, Dominique; Flot, Jean-François. HairSplitter: haplotype assembly from long, noisy reads. Peer Community Journal, Volume 4 (2024), article no. e96. doi : 10.24072/pcjournal.481. [https://peercommunityjournal.org/articles/10.24072/pcjournal.481/](https://peercommunityjournal.org/articles/10.24072/pcjournal.481/)
 
 




