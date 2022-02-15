# Project management

A research project must follow a rigid organisation to make code and 
experiments reproducible. No exceptions are allowed.

A project is made of two components:

1. the workflow: that is all your code to perform analysis and run experiments.
2. the workspace: that is a directory on a cluster, where all your experiments
   take place.

We have defined templates, called cookiecutters, to setup a workflow and a
workspace according to lab standards.

Managing your project is a 3 step process:

1. **development**: writing your workflow;
2. **testing**: running your experiments;
3. **reporting**: writing reports.

### Development

1. Creating a workflow using the `cookiecutter-workflow-nf` cookiecutter
   available on the lab GitHub. Please, read how to prepare your workflow
   [here](project-workflow).
2. Write the code for your workflow.
3. Add test data, package required software and libraries with Docker.
4. Push your workflow to GitHub.

To facilitate using GiHub it is recommended to set up SSH connection to GitHub, please check for instructions: [Connecting to GitHub with SSH](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh). In addition to that you need to modify Nextflow SCM file as per instructions here: [SCM configuration](https://www.nextflow.io/docs/latest/sharing.html?highlight=scm). This will require setting up a private API access token (PAT). For instructions, see: [Creating a personal access token](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token).

### Testing

1. Create a project workspace using the `cookiecutter-workspace-nf` cookiecutter
   on one of the clusters. Please, read carefully the workspace structure by
   reading the document [here](project-workspace).
2. Pull your workflow from GitHub.
3. Set your experimental parameters.
4. Run your experiments.
5. Save a snapshot of your work with `duplicacy backup`.

### Reporting

Write a report describing:

- methods used and/or implemented;
- experimental setup, e.g. parameters, workflow version etc.
- tables and figures
