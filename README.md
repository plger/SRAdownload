# SRAdownload

Bash scripts to download fastq files from the SRA, with priority to european servers, grouping runs from the same experiments.

Since the NCBI servers are very slow (at least accessed from Europe), this first tries to download the fastq files directly from the ftp.sra.ebi.ac.uk server.
However, not all runs are available on that server; therefore, when not available the script falls back to fasterq-dump.

## Requirements

Requires awk, curl, pigz and fasterq-dump (form the [SRA tools](https://github.com/ncbi/sra-tools))

## Usage

1. Download the scripts in this repo.
2. Get a `SraRunTable.txt` file with your runs of interest using the [SRA Run selector](https://www.ncbi.nlm.nih.gov/Traces/study/) (also available at the bottom of a GSE* page on [GEO](https://www.ncbi.nlm.nih.gov/geo/)).
3. Preferably in a screen, run: `path/to/downloadFromSraRunTable.sh /path/to/SraRunTable.txt output_dir`

## Output

For each SRA Experiment, a folder will be created with both the SRX id and the GSM id (if any), containing the downloaded fastq files.

## Notes/disclaimers

- When falling back to fasterq-dump, depending on your configs the prefetch might be saved in `~/ncbi/public/sra/`, which will accumulate and become rather big...
- At the moment, some experiments fail a certain proportion of the time (not sure why). These result in empty folders. However, you can simply delete those folders and launch the script again, it will skip the experiments for which a folder is already present and try again the missing ones. This also means that multiple instances of the script can be launched on the same table and output dir.
- By default, two curl runs will be executed in parallel for paired-end files, and a maximum of 4 threads will be used when downloading via fasterq-dump. This can be changed at the top of the [downloadOneRun.sh](downloadOneRun.sh#L19) file.
- Developed using bash under unbutu, can't guarantee portability to other systems, or guarantee anything, for that matter.
