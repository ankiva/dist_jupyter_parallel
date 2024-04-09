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
I use conda library to configure python environment and install required python libraries. It is possible to register a python environment as a kernel for UT HPC Jupyter.

### Running default IPython cluster on UT HPC Jupyter 

### Running MPI IPython cluster on UT HPC Jupyter

### Running SLURM cluster on UT HPC Jupyter using Rocket cluster

## Running SLURM IPython cluster on Rocket cluster from local Jupyter Notebook

## Running MPI IPython cluster on Rocket cluster from local Jupyter Notebook


## References

[1]: https://docs.hpc.ut.ee/public/services/jupyter.hpc.ut.ee/
[2]: https://hpc.ut.ee/
[3]: https://hpc.ut.ee/services/HPC-services/Rocket
