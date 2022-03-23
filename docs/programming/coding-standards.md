
## Programming languages

The default programming language for all our project is `Python` version 3.7 or
newer. Your code should be compliant to `PEP8` and formatted using `black`.
Object Oriented Programming (OOP) should be used whenever possible.

For specific tasks, such as preparing figures for publication or differential
expression analysis, we might rely on `R`.

## Programming environments

The default integrated development environment (IDE) is [Visual Studio
Code](https://code.visualstudio.com) (aka `vscode`). All code must be developed
using `vscode`.

## Workflow systems

Project workflows must be implemented in [Nextflow](https://nextflow.io). You
are encouraged to use [Tower](https://tower.nf) to monitor your experiments.

## Software environments

It is highly recommended to develop your code in isolated `Docker` containers
using `vscode` and install required software using `conda`. The workflow
cookiecutter comes preconfigured to run on Docker/Singularity containers.

## Readme and manifest files

Readme and manifest files MUST be written in Markdown.

## Backup your project workspace

A project workspace must be backed up using `duplicacy` right after you finish
your experiments. Duplicacy performs versioning and bit-level deduplication, in
order to minimise storage space and data transfers.