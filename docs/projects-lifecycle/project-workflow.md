# Project workflow 

We use a standardized pipeline structure based on Nextflow, which simplify
developing methods and run experiments across different environments.

## Create a new workflow structure

```
cookiecutter git@github.com:stracquadaniolab/cookiecutter-workflow-nf.git
```

## Directory structure

```
|-- bin
|   `-- main.py
|-- conf
|   |-- base.config
|   |-- ci.config
|   |-- sge.config
|   |-- test.config
|   `-- workstation.config
|-- containers
|   `-- Dockerfile
|-- environment.yml
|-- main.nf
|-- nextflow.config
`-- testdata
```

## The workflow file

The `main.nf` file contains the entrypoint for the workflow, and it uses DSL2 by
default. The workflow configuration is stored in the `nextflow.config` file and
under the `conf` directory; usually, you only have to define the parameters of
your specific pipeline, since the other configurations are profiles to run the
pipeline in different computing environment, e.g. SGE, GitHub. Please, refer to
the [Nextflow documentation](https://www.nextflow.io/docs/latest/index.html) for
an overview of the framework.

### Custom scripts management

Custom code (aka your scripts and classes) needed by the pipeline should be
added to the `bin` directory; the code in this directory is automatically added
to `$PATH` when running the pipeline, which makes custom scripts easily portable
and accessible. If you are using Python, you should have a file for each class
of operations, e.g. a file `plots.py` for all the plots, and use `typer` to have
standard Unix command line interface. See the auto-generated pipeline for an
example. 

### Software management

Third-party software is managed by `conda` and specified in a `environment.yml`
file; keep the `yml` file updated and specify the version of each software you
are using.

To ensure reproducibility and running experiments on local machines and HPC
clusters, it is strongly recommended to build a Docker image. The bundled
`Dockerfile` can be used to build an image with the software specified in your
`environment.yml` file. To do that, run:

```bash
docker build . -t ghcr.io/stracquadaniolab/<my-workflow> -f containers/Dockerfile
```

where `<my-workflow>` is the name of your workflow.

The template comes with an auto-generated `.devcontainer.json` file, which
allows developing your scripts inside a container with all the software
specified in `environment.yml` using `vscode`.

Sometimes you would want to pull a docker image from GitHub container registry:
```
docker pull ghcr.io/stracquadaniolab/<workflow_name>:<version>
```
In order to successfully pull an image, first you need to authenticate yourself
with personal access token, see here: [Authenticating with the container
registry](https://docs.github.com/en/packages/guides/migrating-to-github-container-registry-for-docker-images#authenticating-with-the-container-registryhttps://docs.github.com/en/packages/guides/migrating-to-github-container-registry-for-docker-images#authenticating-with-the-container-registry)

### Testing

It is important to build workflows that can be automatically tested; thus, you
will have to add small test data into the `testdata` directory, and modify the
`conf/test.config` configuration file to specify any parameter needed for your
workflow to run. See the auto-generated pipeline for an example.

### Versioning

All projects must follow a semantic version scheme. The format adopted is
`MAJOR.MINOR.PATCH`:

- MAJOR: drastic changes that make disruptive changes with a previous release. 
- MINOR: add functions to the workflow but keeps everything compatible within
  the MAJOR version.
- PATCH: bug fixes or settings update.

To update the version of your workflow, you should run the following command from 
the command line: 

```
bump2version <major|minor|patch>
```


### Documentation

Each workflow must have an updated `readme.md` file, describing:

* what the workflow does
* how to configure the workflow
* how to run the workflow
* a description of the output generated

A `readme.md` file with the required sections is automatically generated by this
cookiecutter.

### Continuous integration

Each pipeline comes with a pre-configured GitHub workflow to automatically test
the code and build a Docker image; the workflow is stored in
`.github/workflows/ci.yml`.