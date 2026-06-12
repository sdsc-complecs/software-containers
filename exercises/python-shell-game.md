# Exercise 1: Python Shell Game

On your personal computer, you likely have a default installation of [Python](https://en.wikipedia.org/wiki/Python_(programming_language)) that came with your operating system.

*Command*
```
python --version
```

*Output*
```
mkandes@hardtack:~$ python --version
Python 3.12.3
mkandes@hardtack:~$
```

The Linux distribution on a supercomputer like [Expanse](https://www.sdsc.edu/systems/expanse/index.html) also has its own default installation of Python.

*Command*
```
ssh mkandes@login.expanse.sdsc.edu
```

*Output*
```
mkandes@hardtack:~$ ssh mkandes@login.expanse.sdsc.edu
(mkandes@login.expanse.sdsc.edu) TOTP code for mkandes: 230935
Welcome to Bright release         9.0

                                                         Based on Rocky Linux 8
                                                                    ID: #000002

--------------------------------------------------------------------------------

                                 WELCOME TO
                  _______  __ ____  ___    _   _______ ______
                 / ____/ |/ // __ \/   |  / | / / ___// ____/
                / __/  |   // /_/ / /| | /  |/ /\__ \/ __/
               / /___ /   |/ ____/ ___ |/ /|  /___/ / /___
              /_____//_/|_/_/   /_/  |_/_/ |_//____/_____/

--------------------------------------------------------------------------------

Use the following commands to adjust your environment:

'module avail'            - show available modules
'module add <module>'     - adds a module to your environment for this session
'module initadd <module>' - configure module to be loaded at every login

-------------------------------------------------------------------------------
Last login: Thu Jun 11 08:56:01 2026 from 69.196.44.249
[mkandes@login02 ~]$ python --version
Python 3.6.8
[mkandes@login02 ~]$
```

But there you'll also find additional versions of Python in its software [environment modules](https://en.wikipedia.org/wiki/Environment_Modules_(software)).

*Command*
```
module spider python
```

*Output*
```
[mkandes@login02 ~]$ module spider python

--------------------------------------------------------------------------------------------------------------------------------
  python: python/3.8.5
--------------------------------------------------------------------------------------------------------------------------------

     Other possible modules matches:
        py-python-dateutil, python/3.8.12, python3, python37

    You will need to load all module(s) on any one of the lines below before the "python/3.8.5" module is available to load.

      cpu/0.15.4  gcc/10.2.0
      cpu/0.15.4  gcc/9.2.0
      cpu/0.15.4  intel/19.1.1.217
      gpu/0.15.4
 
    Help:
      The Python programming language.

--------------------------------------------------------------------------------------------------------------------------------
  To find other possible module matches execute:

      $ module -r spider '.*python.*'

[mkandes@login02 ~]$ module load gcc/10.2.0
[mkandes@login02 ~]$ module load python/3.8.12
[mkandes@login02 ~]$ module list

Currently Loaded Modules:
  1) shared            3) slurm/expanse/23.02.7   5) DefaultModules       7) python/3.8.12/7zdjza7
  2) cpu/0.17.3b (c)   4) sdsc/1.0                6) gcc/10.2.0/npcyll4

  Where:
   c:  built natively for AMD Rome

[mkandes@login02 ~]$ python --version
Python 3.8.12
[mkandes@login02 ~]$
```

You'll even find additional versions of Python hidden within the modules themselves (e.g., like [Anaconda](https://en.wikipedia.org/wiki/Anaconda_(Python_distribution))). 

*Command*
```
module load anaconda3/2021.05
```

*Output*
```
[mkandes@login02 ~]$ module load anaconda3/2021.05
[mkandes@login02 ~]$ module list

Currently Loaded Modules:
  1) shared            3) slurm/expanse/23.02.7   5) DefaultModules       7) python/3.8.12/7zdjza7
  2) cpu/0.17.3b (c)   4) sdsc/1.0                6) gcc/10.2.0/npcyll4   8) anaconda3/2021.05/q4munrg

  Where:
   c:  built natively for AMD Rome

[mkandes@login02 ~]$ which python
/cm/shared/apps/spack/0.17.3/cpu/b/opt/spack/linux-rocky8-zen/gcc-8.5.0/anaconda3-2021.05-q4munrgvh7qp4o7r3nzcdkbuph4z7375/bin/python
[mkandes@login02 ~]$ python --version
Python 3.8.8
[mkandes@login02 ~]$
```

But even with all of these different version of Python loaded simultaneously, we can isolate the use of a specific version of Python we want to use within a [Docker](https://en.wikipedia.org/wiki/Docker_(software)) container using [Singularity](https://en.wikipedia.org/wiki/Apptainer)! 

Here, we first create an interactive session on one of Expanse's compute nodes and load SingularityPRO ...

*Command* 
```
srun --partition=debug --account=use300 --nodes=1 --ntasks-per-node=1 --cpus-per-task=4 --mem=8G --time=00:30:00 --pty --wait=0 /bin/bash
```

*Output*
```
srun --partition=debug --account=use300 --nodes=1 --ntasks-per-node=1 --cpus-per-task=4 --mem=8G --time=00:30:00 --pty --wait=0 /bin/bash
srun: job 50732862 queued and waiting for resources
srun: job 50732862 has been allocated resources
[mkandes@exp-9-55 ~]$ module load singularitypro
[mkandes@exp-9-55 ~]$ module list

Currently Loaded Modules:
  1) shared                      6) gcc/10.2.0/npcyll4
  2) cpu/0.17.3b           (c)   7) python/3.8.12/7zdjza7
  3) slurm/expanse/23.02.7       8) anaconda3/2021.05/q4munrg
  4) sdsc/1.0                    9) singularitypro/3.11
  5) DefaultModules

  Where:
   c:  built natively for AMD Rome

 

[mkandes@exp-9-55 ~]$ singularity --version
SingularityPRO version 3.11-7.el8
[mkandes@exp-9-55 ~]$
```

... and then spawn an interactive [shell](https://docs.sylabs.io/guides/latest/user-guide/cli/singularity_shell.html) into our target container. 

*Command*
```
singularity shell docker://quay.io/jupyter/pyspark-notebook:latest
```

*Output*
```
[mkandes@login02 ~]$ singularity shell docker://quay.io/jupyter/pyspark-notebook:latest
[mkandes@exp-9-55 ~]$ singularity shell docker://quay.io/jupyter/pyspark-notebook:latest
INFO:    Converting OCI blobs to SIF format
WARNING: 'nodev' mount option set on /scratch, it could be a source of failure during build process
INFO:    Starting build...
Getting image source signatures
Copying blob ef288a9a5382 done   | 
Copying blob ef288a9a5382 done   | 
Copying blob 4f4fb700ef54 done   | 
Copying blob 7ff1c663bf46 done   | 
Copying blob 4f4fb700ef54 skipped: already exists
...
2026/06/11 17:05:33  info unpack layer: sha256:4f4fb700ef54461cfa02571ae0db9a0dc1e0cdb5577484a6d75e68dc38e8acc1
2026/06/11 17:05:33  info unpack layer: sha256:700fee56564cd98400351f9fbcc6022ea446a864ee1d7054e9c061c0a36513d9
2026/06/11 17:05:33  warn rootless{opt/conda/share/gdb/auto-load/opt/conda/lib/libarrow.so.2400.0.0-gdb.py} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2026/06/11 17:05:33  info unpack layer: sha256:4f4fb700ef54461cfa02571ae0db9a0dc1e0cdb5577484a6d75e68dc38e8acc1
INFO:    Creating SIF file...
Singularity> python --version
Python 3.13.13
Singularity>
```
