# Cell cluster

Cell is the lab internal cluster, which should be used for high priority jobs,
or jobs requiring resources hard to schedule on EDDIE.

## Access

```
ssh UUN@cell.bio.ed.ac.uk
```

where UUN is your university UUN and the password is the one used for MyED and
other university services.


## Filesystem

- `/export/homes/home/UUN`: 20Gb space. Home directory to use for storing conda
  and basic tools.
- `/export/projects/lab`: up to 7Tb. This is the location where lab members must
  create their workspace directories.
- `/export/projects/collaborators`: up to 7Tb. This is the location where
  collaborators, visitors and UG students must create their workspace
  directories.

IMPORTANT! Although the projects are stored on RAID1, the cluster is not backed
up. Thus, it's mandatory to transfer your results on the group datastore once
your jobs are done. Datastore is accessible from the nodes using sftp. More info
on Datastore. 

## Setup miniconda 

Download the installer and install it on your home directory. 

```
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
``` 

Run the installer 

```
bash Miniconda3-latest-Linux-x86_64.sh
``` 
and answer 'yes' to each question including the default location. Then, activate
the installation by running

```
source ~/.bashrc
```

## Install nextflow

You can install Nextflow from the command line by running: 

```
curl -s https://get.nextflow.io | bash
```


## FAQs:

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