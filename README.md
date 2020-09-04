# Miniconda installers contain the conda package manager and Python. 
In order to keep `~/.bashrc` clean the command `conda install` will no be issued,
we will be using `source /usr/local/share/Miniconda3/etc/profile.d/conda.sh` instead.

## Download the installer
Here we download **miniconda** and install at `/usr/local/share/Miniconda3`. Due to the install path of choice,
every install/update will require elevated privileges (aka sudo).
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
sudo bash Miniconda3-latest-Linux-x86_64.sh -b -p /usr/local/share/Miniconda3
rm -f Miniconda3-latest-Linux-x86_64.sh
```
Update conda:
```bash
sudo /bin/bash -c 'source /usr/local/share/Miniconda3/etc/profile.d/conda.sh &&\
	conda update -n base -c defaults conda'
```

Now that **miniconda** is installed and ready, we create the environment **cons** at `/usr/local/share` so it will be visible to every user. By default conda will create local environments at `~/...`:
```bash
sudo /bin/bash -c 'source /usr/local/share/Miniconda3/etc/profile.d/conda.sh &&\
	conda create python=3.6.10 -y -p /usr/local/share/Miniconda3/envs/cons'
```

In order to activate the **cons** environment:
```bash
source /usr/local/share/Miniconda3/etc/profile.d/conda.sh
conda activate /usr/local/share/Miniconda3/envs/cons
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
git clone --recursive https://github.com/slaclab/pydm/archive/v1.10.3.tar.gz &&\
    cd pydm-opi/cons-common && pip install . && cd .. && pip install .
```

## CONS Usage
In order to launch without a previously activated conda env (using `python subprocess` or  `.desktop` files...):
```bash
sudo /bin/bash -c 'source /usr/local/share/Miniconda3/etc/profile.d/conda.sh &&\
	conda activate cons && sirius-hla-as-ap-conlauncher.py'
```

## Usefull commands...
|Command|Description|
|-------|-----------|
|conda info|General conda information|
|conda info --envs|List available conda environments|
|conda search python|List available Python versions|
