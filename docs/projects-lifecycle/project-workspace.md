# Project workspace


## Directory structure

```
.
├── conf
├── data
├── logs
├── resources
├── results
└── readme.md
```

The `conf` directory contains Nextflow config files to run a pipeline; you must
define the parameters of each experiment in a config file rather than passing
them on the command line.

The `data` directory contains data to be processed by a pipeline. This directory
usually contains the raw data (e.g. data from sequencing experiments). You
should take some time to organize it in a meaningful and consistent way.

The `resources` directory contains data retrieved from external
sources/repositories, like annotation files (e.g. genome GFF) or geneset GMT
files.

The `logs` directory contains the log of each pipeline run.

The `results` directory contains the result of experiments. You should take some
time to organize it in a meaningful and consistent way. It is strongly
recommended to results in a directory named like `2022-01-01-my-first-test`.

The `readme.md` file contains a description of the project and how the `data`
and `results` folder are organized.  

## Naming guidelines

The team MUST use the Google naming guidelines, specifically: 

- Make file and directory names lowercase. 
- Separate words with hyphens, not underscores. 
- Use only standard ASCII alphanumeric characters in file and directory names.

**IMPORTANT**: Raw data or external resources are allowed to keep their naming
standard if it makes them easier to identify.