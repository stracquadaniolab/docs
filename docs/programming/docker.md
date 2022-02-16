# Docker 

**GPU is not used in training Neural Networks (NNs)**: 1) In order to enable
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