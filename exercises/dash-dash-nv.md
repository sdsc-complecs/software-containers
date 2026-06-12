# Exercise 3: dash dash nv to CIFAR

Eventually, you'll want to use Singularity containers within your batch jobs on a supercomputer like Expanse. Within this repository, you'll find  an example batch job script that uses a containerized version of Tensorflow to run a simple AI model training run on one of Expanse's `compute` nodes.

```
#!/usr/bin/env bash

#SBATCH --job-name=tf2-train-cnn-cifar-compute
#SBATCH --account=use300
#SBATCH --partition=compute
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=128
#SBATCH --cpus-per-task=1
#SBATCH --mem=242G
#SBATCH --time=00:10:00
#SBATCH --output=%x.o%j.%N

declare -xr LUSTRE_PROJECT_DIR="/expanse/lustre/projects/${SLURM_ACCOUNT}/${USER}"
declare -xr LUSTRE_SCRATCH_DIR="/expanse/lustre/scratch/${USER}/temp_project"
declare -xr LOCAL_SCRATCH_DIR="/scratch/${USER}/job_${SLURM_JOB_ID}"

declare -xr SINGULARITY_MODULE='singularitypro/3.11'
declare -xr SINGULARITY_TMPDIR="${LOCAL_SCRATCH_DIR}"

module purge
module load "${SINGULARITY_MODULE}"
module list
export KERAS_HOME="${LOCAL_SCRATCH_DIR}"
printenv

time -p singularity exec --bind "${KERAS_HOME}:/tmp" docker://nvcr.io/nvidia/tensorflow:22.08-tf2-py3 \
  python3 -u tf2-train-cnn-cifar.py --classes 10 --precision fp32 --epochs 42 --batch_size 256
```


[SINGULAIRTY_TMPDIR](https://docs.sylabs.io/guides/latest/user-guide/build_env.html#temporary-folders)
