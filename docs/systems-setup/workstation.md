# Workstations

The lab uses Mac workstations running Mac OS X, which come pre-configured to
University standards by IT systems. We established a setup procedure to
standardise the work environment in the lab and streamline know-how sharing.

Upon receiving your machine, you should go through the setup procedure before
working on your projects.

## Update Mac OS X

Make sure that you are running latest Mac OS X version before installing any
tool. To do that, go to: `System Preferences > Software Update`. The system will
automatically check the presence of new updates and, if so, download and install
them by pressing the `Update Now` button.

## Install Developer Tools

Developers Tools is a package including the basic tools for development on Mac
OS X, including `gcc`, `python` and `git`.
To install `Developer Tools`, open a `Terminal` and run the following command:

```bash
xcode-select --install
```

press the `Install` button and agree to the license terms.

## Install Visual Studio Code

Visual Studio Code (VSC) is a powerful open-source IDE, which provides excellent
tools for code development, including autocomplete, online debug, and Latex live
compilation.

To install `VSC`, download the installer from:
[https://code.visualstudio.com](https://code.visualstudio.com).  Move the
application from the `Downloads` folder to the Applications folder, which you
can find in the `Finder`.

Recommended plugins to install:

- Better comments
- Code spell checker
- Docker
- Git Graph
- EditorConfig for VSCode
- Git History
- Latex workshop
- Nextflow
- Remote containers
- Remote - SSH
- Rewrap
- Todo Tree

## Install Docker

The lab uses Docker containers as the underpinning execution system for the projects.
You can install the latest release from [https://www.docker.com](https://www.docker.com).

## Install Nextflow

Nextflow is the default workflow orchestration system. You can install Nextflow
from the command line by running:

```bash
curl -s https://get.nextflow.io | bash
```

## Install miniconda

You should use `miniconda` to install any software you might need, since it
allows user-level installations of most common Unix software. If you cannot find
your software in the default channel, please check also the `conda-forge` and
`bioconda` channels.

To install `miniconda`, download the installer in your home directory as
follows:

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```

Then, run the installer as follows:

```bash
bash Miniconda3-latest-Linux-x86_64.sh
```

and answer 'yes' to each question including the default location. Finally,
activate your installation by running:

```bash
source ~/.bashrc
```

## Install Pipx

Many tools we use are written in Python and, depending on how your system and
projects are configured, they might create unwanted problems, which can be
easily avoided by using `Pipx`.

To install `Pipx`, open a `Terminal` and run the following command:

```bash
python3 -m pip install --user pipx
python3 -m pipx ensurepath
```

## Install cookiecutter

`cookiecutter` is a command-line utility that creates folder structures from
a template. We use cookiecutters to bootstrap all our projects.

To install `cookiecutter`, open a `Terminal` and run the following command:

```bash
pipx install cookiecutter
```

## Install bump2version

We manage semantic versioning using bump2version, which can be installed by
opening a `Terminal` and run the following command:

```bash
pipx install bump2version
```
