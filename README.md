# Miniconda installers contain the conda package manager and Python. 
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

In order to install at `/usr/local/share`:
```bash
sudo /bin/bash -c 'source /usr/local/share/Miniconda3/etc/profile.d/conda.sh &&\
	conda create python=3.6.10 -y -p /usr/local/share/Miniconda3/envs/cons'
```

In order to activate the **cons** environment:
```bash
source /usr/local/share/Miniconda3/etc/profile.d/conda.sh
conda activate /usr/local/share/Miniconda3/envs/cons
```

## Usefull
|Command|Description|
|-------|-----------|
|conda info|General conda information|
|conda info --envs|List available conda environments|
|conda search python|List available Python versions|

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
In order to launch without a previously activated conda env:
```bash
sudo /bin/bash -c 'source /usr/local/share/Miniconda3/etc/profile.d/conda.sh &&\
	conda activate cons && sirius-hla-as-ap-conlauncher.py'
```
