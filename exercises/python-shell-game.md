# Exercise 1: Python Shell Game

On your personal computer, you likely have a default installation of Python that came with your operating system.

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
python --version
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

But there you'll also find additional versions of Python in its software module environment.

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

You'll even find other versions of Python hidden within some modules like Anaconda.

*Command*
```

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

We can even use Python within a Docker container.

*Command* 
```
module load singularitypro
```

*Output*
```
[mkandes@login02 ~]$ module load singularitypro
[mkandes@login02 ~]$ module list

Currently Loaded Modules:
  1) shared            3) slurm/expanse/23.02.7   5) DefaultModules       7) python/3.8.12/7zdjza7       9) singularitypro/3.11
  2) cpu/0.17.3b (c)   4) sdsc/1.0                6) gcc/10.2.0/npcyll4   8) anaconda3/2021.05/q4munrg

  Where:
   c:  built natively for AMD Rome

[mkandes@login02 ~]$ singularity --version
SingularityPRO version 3.11-7.el8
[mkandes@login02 ~]$
```

*Command*
```
singularity shell docker://quay.io/jupyter/pyspark-notebook:latest
```

*Output*
```
[mkandes@login02 ~]$ singularity shell docker://quay.io/jupyter/pyspark-notebook:latest
INFO:    Using cached SIF image
Singularity> python --version
Python 3.13.13
Singularity>
```
