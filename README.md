# Conda package manager and Python. 
In order to keep `~/.bashrc` clean the command `conda install` will no be issued,
we will be using `source <conda_dir>/etc/profile.d/conda.sh` instead. In an attempt to conform with the
[Filesystem Hierarchy Standard](https://www.pathname.com/fhs/pub/fhs-2.3.html#OPTADDONAPPLICATIONSOFTWAREPACKAGES) the conda distribution will be installed at `/opt`.

## Debian based
Public GPG key to trusted store
```bash
curl https://repo.anaconda.com/pkgs/misc/gpgkeys/anaconda.asc | gpg --dearmor > conda.gpg
install -o root -g root -m 644 conda.gpg /usr/share/keyrings/conda-archive-keyring.gpg

# Check whether fingerprint is correct (will output an error message otherwise)
gpg --keyring /usr/share/keyrings/conda-archive-keyring.gpg --no-default-keyring --fingerprint 34161F5BF5EB1D4BFBBB8F0A8AEB4F8B29D82806

# Add our Debian repo
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/conda-archive-keyring.gpg] https://repo.anaconda.com/pkgs/misc/debrepo/conda stable main" > /etc/apt/sources.list.d/conda.list

apt update
apt install conda
```

## Manual installation - Download the installer
Here we download **miniconda** and install at `/opt/conda`. Due to the install path of choice,
every install/update will require elevated privileges (aka sudo).
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
sudo bash Miniconda3-latest-Linux-x86_64.sh -b -p /opt/conda
rm -f Miniconda3-latest-Linux-x86_64.sh
```

## Creating the conda environment
```bash
sudo /bin/bash -c 'source /opt/conda/etc/profile.d/conda.sh && conda update -y -n base -c defaults conda'
```

Now that **conda** is installed and ready, we create the environment **cons** at `/opt` so it will be visible to every user. By default conda will create  environments locally at `~/...`:
```bash
sudo /bin/bash -c 'source /opt/conda/etc/profile.d/conda.sh &&\
    conda create python=3.6.10 -y -p /opt/conda/envs/cons'
sudo /bin/bash -c 'source /opt/conda/etc/profile.d/conda.sh &&\
    conda activate cons && conda config --add channels conda-forge && conda config --set channel_priority strict'
```

In order to activate the **cons** environment in a posix compliant shell:
```bash
source /opt/conda/etc/profile.d/conda.sh
conda activate cons
```

Installing `pydm-opi` and dependencies:
```bash
sudo /bin/bash -c 'source /opt/conda/etc/profile.d/conda.sh && conda activate cons &&\
    conda install -y \
        qt==5.12.9 \
        pyqt==5.12.3 \
        pydm==1.10.2 &&\
    git clone --recursive https://github.com/lnls-sirius/pydm-opi/ /tmp/pydm-opi &&\
    cd /tmp/pydm-opi/cons-common && pip install . && cd .. && pip install . && rm -rf /tmp/pydm-opi'
```

## CONS Usage
In order to launch without a previously activated conda env (using `python subprocess` or  `.desktop` files...):
```bash
/bin/bash -c 'source /opt/conda/etc/profile.d/conda.sh &&\
    conda activate cons && sirius-hla-as-ap-conlauncher.py'
```

## Usefull commands...
|Command|Description|
|-------|-----------|
|conda info|General conda information|
|conda info --envs|List available conda environments|
|conda search python|List available Python versions|
