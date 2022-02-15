# Workstation setup 

The lab uses Mac workstations running Mac OS X, which come pre-configured to
University standards by IT systems. We established a setup procedure to
standardise the work environment in the lab and streamline know-how sharing.

Upon receiving your machine, you have to go through the setup procedure before
starting your projects.

## Update Mac OS X

Make sure that you are running latest Mac OS X version before installing any
tool. To do that, go to: System Preferences > Software Update. The system will
automatically check the presence of new updates and, if so, download and install
them by pressing the Update Now button. 

## Install Developer Tools 

Developers Tools is a package including the basic tools for development on Mac
OS X, including gcc, python and git.

To install Developer Tools, open a Terminal and run the following command:

```
xcode-select --install
```

press the Install button and agree to the license terms.

## Install Visual studio code

Visual studio code is a powerful open-source IDE that provides excellent tools
for development, including autocomplete, online debug, and Latex live
compilation and checks. To install Visual studio code, download the installer
from: https://code.visualstudio.com. Move the application from the download
folder to the Applications folder, which you can find in the Finder.

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

```
curl -s https://get.nextflow.io | bash
```

## Install Duplicacy

Duplicacy is the lab storage and de-duplication system. You can install it by
downloading the executable for your workstation here:
[https://github.com/gilbertchen/duplicacy/releases](https://github.com/gilbertchen/duplicacy/releases)

## Install GitHub CLI

GitHub CLI is required by all cookiecutters to initialise the repo in GitHub.
You can install the current version for your system from:
[https://github.com/cli/cli/releases](https://github.com/cli/cli/releases).

## Install miniconda

Miniconda is the default environment manager for all our projects, which allows
all the required tools and libs in an isolated environment. To
install Miniconda, first download the installer
from: https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86_64.sh


Open a Terminal, go to the directory where you downloaded the installer, and run
the following command: 

```
bash Miniconda3-latest-MacOSX-x86_64.sh
```

Accept the license and follow the on-screen instructions. When it comes to pick
the miniconda location, We recommend using the path:
`/Users/<username>/.miniconda3` where <username> is your username in the system.

## Install Pipx

Many tools we use are written in Python and depending on how your system and
projects are configured, they might create unwanted dependencies or problems, 
which can be avoided by using Pipx.

To install Pipx, open the terminal and run the following command:

```
python3 -m pip install --user pipx
python3 -m pipx ensurepath
```

## Install cookiecutter

cookiecutter is a command-line utility that folder structures from templates. We
use cookiecutters to bootstrap all our projects. 

To install cookiecutter, open a Terminal and run the following command: 

```
pipx install cookiecutter
```

## Install bump2version

We manage semantic versioning using bump2version, a tool that can be installed
from the command line by running:

```
pipx install bump2version
```
