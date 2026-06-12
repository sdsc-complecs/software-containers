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

What does it look like from within a Singularity container?

*Command*
```
singularity shell docker://quay.io/jupyter/pyspark-notebook:latest
```

*Output*
```
[mkandes@login02 ~]$ singularity shell docker://quay.io/jupyter/pyspark-notebook:latest
INFO:    Using cached SIF image
Singularity> ls
ExpanseAIR		      mlflow.db
Untitled.ipynb		      mlruns
ai-training-examples	      mlruns.db
ciml-summer-institute-2025    nameofmycontinaer.sif
complecs		      ollama-latest-expanse_latest.sif
containers		      projects
data			      pyspark-notebook_spark-4.0.1.sif
e4s-cuda80-x86_64-23.05.sif   run-mlflow.sh
gpse			      scratch
hardware		      scripts
mlc-run-script-versions.json  software
mlflow-local-sqlite.ipynb     spark_4.0.1-python3.sif
mlflow-nfs.ipynb	      version_info.json
Singularity>
```

It looks the same, no? That's because Singularity will [bind mount](https://en.wikipedia.org/wiki/Mount_(Unix)#Bind_mounting) your $HOME directory into the container by default. That's great since you'll likely need access to some input data in your HOME directory at some point when running your containers. Similariy, it'll allow you to write any output back to your HOME directory as well. 

However, supercomputers like Expanse also typically have additional specialized filesystems for different uses. For example, Expanse has a [Lustre filesystem](https://en.wikipedia.org/wiki/Lustre_(file_system)) that containers both project storage and scratch spaces. 

So, what does my `/expanse/lustre/scratch` space look like from within a container?

*Command*
```
ls /expanse/lustre/scratch/$USER/temp_project
```

*Output*
```
Singularity> ls /expanse/lustre/scratch/$USER/temp_project
ls: cannot access '/expanse/lustre/scratch/mkandes/temp_project': No such file or directory
Singularity>
```

What's wrong? Well, unlinke $HOME, there is no default bind mount to `/expanse/lustre/scratch`. As such, you'll need to use the `--bind` option on the `shell` command to make it visible and accessible from within the container.

*Command*
```
singularity shell --bind /expanse/lustre/scratch/$USER/temp_project docker://quay.io/jupyter/pyspark-notebook:latest
```

*Output*
```
[mkandes@login02 ~]$ ls /expanse/lustre/scratch/$USER/temp_project
100G.dat                             ICICLE-ML-BENCHMARK
cp2k_2025.2_openmpi_x86_64_psmp.sif  ptl-cuda-12-1.sif
cp2k-h2o                             rstudio-server-conda.sh
[mkandes@login02 ~]$ singularity shell docker://quay.io/jupyter/pyspark-notebook:latest
INFO:    Using cached SIF image
Singularity> ls /expanse/lustre/scratch/$USER/temp_project
ls: cannot access '/expanse/lustre/scratch/mkandes/temp_project': No such file or directory
Singularity> exit
exit
[mkandes@login02 ~]$ singularity shell --bind /expanse/lustre/scratch/$USER/temp_project docker://quay.io/jupyter/pyspark-notebook:latest
^[[AINFO:    Using cached SIF image
Singularity> ls /expanse/lustre/scratch/$USER/temp_project
100G.dat	     cp2k_2025.2_openmpi_x86_64_psmp.sif
ICICLE-ML-BENCHMARK  ptl-cuda-12-1.sif
cp2k-h2o	     rstudio-server-conda.sh
Singularity>
```

See the [Bind Paths and Mounts](https://docs.sylabs.io/guides/latest/user-guide/bind_paths_and_mounts.html) section of the Singularity User Guide for more information. 
