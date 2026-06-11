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

The Linux distribution on a supercomputer like Expanse also has a default installation of Python.

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

You might also have other versions of Python available in local conda environments installed on your personal computer.

*Command*
```
conda env list
```

*Output*
```
mkandes@hardtack:~$ conda env list

# conda environments:
#
# * -> active
# + -> frozen
base                     /home/mkandes/software/miniforge/26.3.2-1
hpcgpt-zendesk-clean-20260512     /home/mkandes/software/miniforge/26.3.2-1/envs/hpcgpt-zendesk-clean-20260512
hpcgpt-zendesk-explore-20260512     /home/mkandes/software/miniforge/26.3.2-1/envs/hpcgpt-zendesk-explore-20260512
icicle-tapis-20260522     /home/mkandes/software/miniforge/26.3.2-1/envs/icicle-tapis-20260522
rehs-doccano-20260607     /home/mkandes/software/miniforge/26.3.2-1/envs/rehs-doccano-20260607
rehs-labelstudio-20260608     /home/mkandes/software/miniforge/26.3.2-1/envs/rehs-labelstudio-20260608

mkandes@hardtack:~$
```

*Command*
```
conda activate rehs-doccano-20260607
```

*Output*
```
mkandes@hardtack:~$ conda activate rehs-doccano-20260607
(rehs-doccano-20260607) mkandes@hardtack:~$ 
```

*Command*
```
python --version
```

*Output*
```
mkandes@hardtack:~$ conda activate rehs-doccano-20260607
(rehs-doccano-20260607) mkandes@hardtack:~$ python --version
Python 3.8.20
(rehs-doccano-20260607) mkandes@hardtack:~$
```
