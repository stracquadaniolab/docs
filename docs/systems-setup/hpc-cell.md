# Cell HPC

Cell is the lab High Performance Computing (HPC) system, which should be used
for high priority jobs, or jobs requiring resources hard to schedule on the
EDDIE university system.

## Available resources

### Hardware resources

- 128 CPU cores, 2.4Ghz-3.1GHz
- 64 or 128 Gb ECC RAM per node
- 24Tb storage
- 1 Nvidia TitanX 16GB GPU

### Software resources

- Nvidia GPU drivers 
- Docker
- Singularity
- Slurm

## Obtaining access

Access to Cell is a granted by informing the Dr Stracquadanio and filling a
ticket with the [IS Helpline](https://edin.ac/launch-edhelp), specifying 
that you will need to be added to the `lab` group.

## Login

Cell is available only through SSH within the University of Edinburgh network, 
including UoE VPN, UoE Eduroam, and office ethernet outlets.

To access the login node, use SSH as follows:

```
ssh UUN@cell.bio.ed.ac.uk
```

where UUN is your university UUN and the password is the one used for MyED and
the other University services.

## Filesystems

| Directory                       | Description                                     | Size  | Backup |
| --------------------------------| ------------------------------------------------|-------|--------|
| `/localdisk/storage/home/<UUN>` | Home directory to store conda or basic tools    | 50Gb  |   No   |
| `/localdisk/storage/projects`   | Directory to store teams' projects              | 10Tb  |   Yes  |
| `/localdisk/storage/datasets`   | Directory to store raw data                     | 5Tb   |   Yes  |


**WARNING** Although the projects are regularly backed up, Cell is not intended
for long term storage. Therefore, it's mandatory to transfer your raw data and
final results on the group Datastore regularly.


## Account setup

### Install miniconda 

You should use `miniconda` to install any software you might need, since it
allows user-level installations of most common Unix software. If you cannot find
your software in the default channel, please check also the `conda-forge` and
`bioconda` channels.

To install `miniconda`, download the installer in your home directory as
follows:
```
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
``` 

Then, run the installer as follows: 
```
bash Miniconda3-latest-Linux-x86_64.sh
``` 
and answer 'yes' to each question including the default location. Finally,
activate your installation by running:
```
source ~/.bashrc
```

### Install nextflow

You can install Nextflow from the command line by running: 

```
curl -s https://get.nextflow.io | bash
```


## FAQ

- **I tried to pull a docker image, but it gives me an auth error**: 1) you
  should have a PAT (personal access token) added so that you can access github
  images [github
  help](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token).
  2) you should be added to the docker group by an admin, contact the IS
  helpline to have your used enable 3) your docker image should be built and
  pushed to the gh repository (check the github action for the workflow). These
  three steps should solve 95% of problems. 

- **Pipeline suddenly fails with 137 error**: 1) Most likely you haven't
  allocated enough memory for your job. Check [Nextflow configuration
  file](https://www.nextflow.io/docs/latest/config.html#) for details
  (particularly _executor_ scope: memory setting).

- **GPU is not used in training Neural Networks (NNs)**: 1) In order to enable
  GPU for NN training it is not enough to push model and training data to GPU
  (i.e. model = model.cuda() or model.to(device) where device is "cuda"). The
  stable and error-proof way for using GPU is to run your nextflow (NF) under a
  specific profile (i.e. nextflow run stracquadaniolab/<your_workflow_name>
  -profile dockergpu). Here is a necessary configuration:
```
dockergpu {
    docker.enabled = true
    docker.runOptions = "--gpus all --ipc=host"
  }
```
As you can see the docker is enabled,  which means that a proper docker image
must be built in order to allow GPU to be used in NN training. This brings us to
a particular configuration of Dockerfile:

```
# to allow GPU inherit from pytorch-geometric
FROM ghcr.io/stracquadaniolab/pytorch-geometric:latest

LABEL org.stracquadaniolab.name="<workflow_name>"
LABEL org.stracquadaniolab.description="<workflow_description>"

RUN apt-get update \
    && apt-get install --yes rename procps curl\
    && apt-get autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /
COPY environment.yml /
RUN conda env update -n base --file environment.yml && conda clean --all --yes 

ENV TINI_VERSION v0.19.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini

ENTRYPOINT ["tini", "--"]
CMD ["/bin/bash"]
```
After these changes are incorporated you should be able to run you workflow.
Command will look like (also pay attention to FAQ section about 137 error and
modify _sge_ profile or supply your own):
```
nextflow run stracquadaniolab/<your_workflow_name> -profile test, dockergpu, sge
```