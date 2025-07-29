Sometimes, highly similar amplicons get reported as a single collapsed amplicon. AmpliconSplitter splits these amplicons to recover all the amplicons. AmpliconSplitter is based on [HairSplitter](github.com/rolandfaure/hairsplitter)

# Installation

You can install HairSplitter through conda `conda install -c bioconda hairsplitter`

## Dependencies

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
 
### Quick conda dependencies

The recommended way to install HairSplitter is to create and activate a conda environment with all dependencies: 
```
conda create -c bioconda -c conda-forge -c anaconda -n hairsplitter cmake gxx gcc python scipy numpy minimap2 minigraph=0.20 racon "samtools>=1.16" raven-assembler openmp
conda activate hairsplitter

conda install -c bioconda -c conda-forge medaka #only if you specifically want to use medaka /!\ Very heavy installation
```
 
## Download & Compilation

To download and compile, run
```
git clone https://github.com/RolandFaure/Hairsplitter.git
cd Hairsplitter/src/
mkdir build && cd build
cmake ..
make
cd ../../ && chmod +x hairsplitter.py
```

# Usage

## Quick start

Let's say `reads.fastq` (ONT reads) were used to build assembly `assembly.gfa` (with any assembler)(the assembly can be in gfa or fasta format). To improve/phase the assembly using `Hairsplitter`, run
```
python /path/to/hairsplitter/folder/hairsplitter.py -f reads.fastq -i assembly.gfa -x ont -o hairsplitter_out/
```

In the folder hairsplitter\_out, you will find the new assembly, named `hairsplitter_final_assembly.gfa`. Another generated file is `hairsplitter_summary.txt`, in which are written which contigs are duplicated and merged.

You can test the installation on the mock instance provided and check that HairSplitter exits without problems.
```
python hairsplitter.py -i test/simple_mock/assembly.gfa -f test/simple_mock/mock_reads.fasta -o test_hairsplitter/ -F
```

## Options

```bash
usage: hairsplitter.py [-h] -i ASSEMBLY -f FASTQ [-c HAPLOID_COVERAGE]
                       [-x USE_CASE] [-p POLISHER] [--correct-assembly]
                       [-t THREADS] -o OUTPUT [-u RESCUE_SNPS]
                       [-q MIN_READ_QUALITY] [--resume] [-s] [-P] [-F] [-l]
                       [--no_clean]
                       [--rarest-strain-abundance RAREST_STRAIN_ABUNDANCE]
                       [--minimap2-params MINIMAP2_PARAMS]
                       [--path_to_minigraph PATH_TO_MINIGRAPH]
                       [--path_to_medaka PATH_TO_MEDAKA]
                       [--path_to_python PATH_TO_PYTHON]
                       [--path_to_raven PATH_TO_RAVEN] [-v] [-d]

options:
  -h, --help            show this help message and exit
  -i ASSEMBLY, --assembly ASSEMBLY
                        Original assembly in GFA or FASTA format (required)
  -f FASTQ, --fastq FASTQ
                        Sequencing reads fastq or fasta (required)
  -c HAPLOID_COVERAGE, --haploid-coverage HAPLOID_COVERAGE
                        Expected haploid coverage. 0 if does not apply [0]
  -x USE_CASE, --use-case USE_CASE
                        {ont, pacbio, hifi, amplicon} [ont]
  -p POLISHER, --polisher POLISHER
                        {racon, medaka} medaka is more accurate but much
                        slower [racon]
  --correct-assembly    Correct structural errors in the input assembly (time-
                        consuming)
  -t THREADS, --threads THREADS
                        Number of threads [1]
  -o OUTPUT, --output OUTPUT
                        Output directory
  -u RESCUE_SNPS, --rescue_snps RESCUE_SNPS
                        Consider automatically as true all SNPs shared by
                        proportion u of the reads [0.33]
  -q MIN_READ_QUALITY, --min-read-quality MIN_READ_QUALITY
                        If reads have an average quality below this threshold,
                        filter out (fastq input only) [0]
  --resume              Resume from a previous run
  -s, --dont_simplify   Don't merge the contig
  -P, --polish-everything
                        Polish every contig with racon, even those where there
                        is only one haplotype
  -F, --force           Force overwrite of output folder if it exists
  -l, --low-memory      Turn on the low-memory mode (at the expense of speed)
  --no_clean            Don't clean the temporary files
  --rarest-strain-abundance RAREST_STRAIN_ABUNDANCE
                        Limit on the relative abundance of the rarest strain
                        to detect (0 might be slow for some datasets) [0.01]
  --minimap2-params MINIMAP2_PARAMS
                        Parameters to pass to minimap2
  --path_to_minigraph PATH_TO_MINIGRAPH
                        Path to the executable minigraph [minigraph]
  --path_to_medaka PATH_TO_MEDAKA
                        Path to the executable medaka [medaka]
  --path_to_python PATH_TO_PYTHON
                        Path to python [python]
  --path_to_raven PATH_TO_RAVEN
                        Path to raven [raven]
  -v, --version         Print version and exit
```

# Issues
 Most installation issues that we have seen yet stem from the use of too old compilers. Hairsplitter has been developed using gcc=11.2.0. Sometimes the default version of the compiler is too old (especially on servers). Specify gcc versions manually to cmake using `-DCMAKE_CXX_COMPILER=/path/to/modern/g++` and `-DCMAKE_C_COMPILER=/path/to/modern/gcc`.
 
 <a name="work">
</a>
 
# Citation
 If you use AmpliconSplitter, please cite HairSplitter. HairSplitter is published in the Peer-Community Journal (PCJ): Faure, Roland; Lavenier, Dominique; Flot, Jean-François. HairSplitter: haplotype assembly from long, noisy reads. Peer Community Journal, Volume 4 (2024), article no. e96. doi : 10.24072/pcjournal.481. [https://peercommunityjournal.org/articles/10.24072/pcjournal.481/](https://peercommunityjournal.org/articles/10.24072/pcjournal.481/)
 
 




