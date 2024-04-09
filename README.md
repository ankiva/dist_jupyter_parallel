# dist_jupyter_parallel
Guide how to utilize parallel algorithms in Jupyter Notebook and optionally execute them in UTHPC

## Introduction to Jupyter Notebook, ipyparallel library, UT HPC, Slurm

### Introduction to Jupyter Notebook

#### Jupyter Notebook classic

#### Jupiter LAB

#### Jupyter HUB

#### Binder

#### nbgrader

### Introduction to ipyparallel library

### Introduction to UT HPC

### Introduction to SLURM

## Running cluster locally on default profile

## Running MPI IPython cluster locally

## Running IPython clusters on UT HPC Jupyter Notebook

[UT HPC Jupyter][1] is a service provided by [UT HPC][2].
It runs as a SLURM job inside [Rocket][3] cluster. Since runtime environment is your user home directory at UT it is easier to configure various aspects.

### Configure environment
I use conda package and environment management tool to configure python environment and install required python libraries. It is possible to register a python environment as a kernel for UT HPC Jupyter.

    module load any/python/3.8.3-conda
    conda create -n conda_venv_parallel
    conda activate conda_venv_parallel
    conda install ipykernel
    conda install ipyparallel
    python -m ipykernel install --user --name=conda_venv_parallel

Configure kernel:
see your configured environment variables:

    echo $PATH
    echo $LIBRARY_PATH
    echo $LD_LIBRARY_PATH

And insert the values to kernel JSON configuration file ~/.local/share/jupyter/kernels/conda_venv_parallel/kernel.json

Restart HPC Jupyter. Select `conda_venv_parallel` from kernel list in HPC Jupyter.

### Running default IPython cluster on UT HPC Jupyter 

### Running MPI IPython cluster on UT HPC Jupyter

### Running SLURM cluster on UT HPC Jupyter using Rocket cluster

#### Configuration

#### Execution

## Running SLURM IPython cluster on Rocket cluster from local Jupyter Notebook

## Running MPI IPython cluster on Rocket cluster from local Jupyter Notebook


## References

[1]: https://docs.hpc.ut.ee/public/services/jupyter.hpc.ut.ee/ "UT HPC Jupyter"
[2]: https://hpc.ut.ee/ "UT HPC docs"
[3]: https://hpc.ut.ee/services/HPC-services/Rocket "UT HPC rocket"
[4]: https://docs.hpc.ut.ee/public/cluster/Software/python_envs "Python environments in HPC"
