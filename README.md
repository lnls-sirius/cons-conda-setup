# Miniconda installers contain the conda package manager and Python. 
In order to keep `~/.bashrc` clean the command `conda install` will no be issued,
we will be using `source <conda_dir>/etc/profile.d/conda.sh` instead. In an attempt to conform with the
[Filesystem Hierarchy Standard](https://www.pathname.com/fhs/pub/fhs-2.3.html#OPTADDONAPPLICATIONSOFTWAREPACKAGES) the conda distribution will be installed at `/opt`.

## Download the installer
Here we download **miniconda** and install at `/opt/Miniconda3`. Due to the install path of choice,
every install/update will require elevated privileges (aka sudo).
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
sudo bash Miniconda3-latest-Linux-x86_64.sh -b -p /opt/Miniconda3
rm -f Miniconda3-latest-Linux-x86_64.sh
```
Update conda:
```bash
sudo /bin/bash -c 'source /opt/Miniconda3/etc/profile.d/conda.sh &&\
	conda update -n base -c defaults conda'
```

Now that **miniconda** is installed and ready, we create the environment **cons** at `/opt` so it will be visible to every user. By default conda will create  environments locally at `~/...`:
```bash
sudo /bin/bash -c 'source /opt/Miniconda3/etc/profile.d/conda.sh &&\
	conda create python=3.6.10 -y -p /opt/Miniconda3/envs/cons'
```

In order to activate the **cons** environment in a posix compliant shell:
```bash
source /opt/Miniconda3/etc/profile.d/conda.sh
conda activate /opt/Miniconda3/envs/cons
```

## CONS Setup
As the root user:
```bash
conda config --add channels conda-forge
conda activate cons
conda install -y\
    qt==5.12.9 \
    pyqt==5.12.3 \
    pydm==1.10.1
git clone --recursive https://github.com/lnls-sirius/pydm-opi/ &&\
    cd pydm-opi/cons-common && pip install . && cd .. && pip install .
```

## CONS Usage
In order to launch without a previously activated conda env (using `python subprocess` or  `.desktop` files...):
```bash
sudo /bin/bash -c 'source /opt/Miniconda3/etc/profile.d/conda.sh &&\
	conda activate cons && sirius-hla-as-ap-conlauncher.py'
```

## Usefull commands...
|Command|Description|
|-------|-----------|
|conda info|General conda information|
|conda info --envs|List available conda environments|
|conda search python|List available Python versions|
