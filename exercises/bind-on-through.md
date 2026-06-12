# Exercise 2: bind on through (to the other side)

[Filesystem](https://en.wikipedia.org/wiki/File_system) isolation in containers prevents processes from accessing or modifying the host operating system’s files and directories. It segments file access and restricts what the containerized application can see.

You'll typically use your [NFS](https://en.wikipedia.org/wiki/Network_File_System)-based $HOME directory on Expanse to transfer small amonts data to and from the system, install software applications, and write, store, and submit batch job scripts to the Slurm scheduler. 

Here, let's take a look at my $HOME directory outside of a container. 

*Command*
```
ls
```

*Output*
```
[mkandes@login02 ~]$ hostname -f
login02.expanse.sdsc.edu
[mkandes@login02 ~]$ pwd
/home/mkandes
[mkandes@login02 ~]$ ls
ai-training-examples          mlruns
ciml-summer-institute-2025    mlruns.db
complecs                      nameofmycontinaer.sif
containers                    ollama-latest-expanse_latest.sif
data                          projects
e4s-cuda80-x86_64-23.05.sif   pyspark-notebook_spark-4.0.1.sif
ExpanseAIR                    run-mlflow.sh
gpse                          scratch
hardware                      scripts
mlc-run-script-versions.json  software
mlflow.db                     spark_4.0.1-python3.sif
mlflow-local-sqlite.ipynb     Untitled.ipynb
mlflow-nfs.ipynb              version_info.json
[mkandes@login02 ~]$
```
