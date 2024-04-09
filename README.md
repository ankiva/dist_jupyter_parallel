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

Create new IPython profile

    ipython profile create --parallel --profile=slurm

Add template for controller in HPC user home directory with filename slurm.controller.template

    #!/bin/sh
    #SBATCH --output={output_file}
    #SBATCH --export=ALL
    #SBATCH --job-name=ipcontroller-{cluster_id}
    #SBATCH --ntasks=1
    #SBATCH --time=30
    {program_and_args}

Add template for engines in HPC user home directory with name slurm.engine.template

    #!/bin/bash
    #SBATCH --time=30
    #SBATCH --export=ALL
    #SBATCH --output={output_file}
    #SBATCH --job-name=ipengine-{cluster_id}
    #SBATCH --ntasks={n}
    #SBATCH --nodes={n//5 + 1}
    srun {program_and_args} --debug --profile-dir={profile_dir} --cluster-id={cluster_id}

Configure ~/.ipython/profile_slurm/ipcluster_config.py

    c.SlurmControllerLauncher.batch_template_file = 'slurm.controller.template'
    c.SlurmEngineSetLauncher.batch_template_file = 'slurm.engine.template'
    c.Cluster.controller_launcher_class = 'ipyparallel.cluster.launcher.SlurmControllerLauncher'
    c.Cluster.engine_launcher_class = 'ipyparallel.cluster.launcher.SlurmEngineSetLauncher'

#### Execution

    import ipyparallel as ipp
    from datetime import datetime as dt
    
    def hostname_example():
        import platform
        return f"Hello World from hostname {platform.node()}."
        
    cluster = ipp.Cluster(profile="slurm_example", n=12, timelimit="30", engine_timeout=80, debug=True, log_level=10, delay=120, send_engines_connection_env=True)
    
    # send connection info: connection files are created later than timeout 60s
    try:
        print(f"---starting cluster {dt.now()}")
        cluster.start_cluster_sync()
        
        print(f"---connecting client {dt.now()}")
        rc = cluster.connect_client_sync()
        print(f"---waiting for engines {dt.now()}")
        rc.wait_for_engines(n=12, timeout=1000)
    
        rc.ids
        print(f"---apply {dt.now()}")
        r = rc[:].apply_sync(hostname_example)
    
        print(f"---joining {dt.now()}")
        print("\n".join(r))
    finally:
        print(f"---closing {dt.now()}")
        # will also remove log files and connection files
        cluster.stop_cluster_sync()

## Running SLURM IPython cluster on Rocket cluster from local Jupyter Notebook

## Running MPI IPython cluster on Rocket cluster from local Jupyter Notebook


## References

[1]: https://docs.hpc.ut.ee/public/services/jupyter.hpc.ut.ee/ "UT HPC Jupyter"
[2]: https://hpc.ut.ee/ "UT HPC docs"
[3]: https://hpc.ut.ee/services/HPC-services/Rocket "UT HPC rocket"
[4]: https://docs.hpc.ut.ee/public/cluster/Software/python_envs "Python environments in HPC"
[5]: https://ipyparallel.readthedocs.io/en/latest/tutorial "IPyParallel tutorial"
[6]: https://ipyparallel.readthedocs.io/en/latest/api/ipyparallel.html "IPyParallel source"
