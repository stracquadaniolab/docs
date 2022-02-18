# Cell HPC

Cell is the lab High Performance Computing (HPC) system, which should be used
for high priority jobs, or jobs requiring resources hard to schedule on the
EDDIE university system.

## Available resources

###  Hardware resources

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

##  Account setup

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

```bash
source ~/.bashrc
```

### Install nextflow

You can install Nextflow from the command line by running:

```bash
curl -s https://get.nextflow.io | bash
```

####  Add your GitHub PAT

You need to add your PAT to the Nextflow configuration to pull from the lab
GitHub organization. To do that, create a new text file as follows:

```bash
nano ~/.nextflow/scm
```

and write the following configuration parameters, replacing the `user` and
`password` information with the your credentials.

```bash
providers {
    github {
        user = 'your-user-name'
        password = 'your-personal-access-token;'
    }
}
```

Finally, press `Ctrl+o` to write your configuration to file.

To pull Docker images from GitHub, you also have to configure the credentials
for `singularity` as environment variables. To do that, create a new config
file as follows:

```bash
nano ~/.nextflow/config
```

and write the following parameters, replacing the `SINGULARITY_DOCKER_PASSWORD`
and with the your PAT.

```bash
env {
        SINGULARITY_DOCKER_LOGIN='$oauthtoken'
        SINGULARITY_DOCKER_PASSWORD='your-personal-access-token;'
}
```

Finally, press `Ctrl+o` to write your configuration to file.

####  Test your installation

You can test your installation by creating a `test-workflow` in
your home directory as follows:  

```bash
mkdir ~/test-workflow && cd ~/test-workflow
```

Now pull the `boot-nf` workflow as follows:

```bash
nextflow pull stracquadaniolab/boot-nf
```

and run it on Cell as follows:

```bash
nextflow run stracquadaniolab/boot-nf -profile singularity,slurm,test
```

which will create a file `results/results.txt` containing your ip address and
location in JSON format.

###  Install cookiecutter

You can install `cookiecutter` by running the following command:

```bash
conda install -c conda-forge cookiecutter
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
