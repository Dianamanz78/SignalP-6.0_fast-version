# SignalP-6.0_Fast-Version
The SignalP 6.0 container is accessible in the cluster. The signalp_fast.sif file is located in the shared folder in the roth lab directory.

# Usage
<br>

Using the .sif file is reliant on the container platform, singularity. To see if singularity is available in the cluster, enter the following in the command line: 

```
module avail
```

If singularity is listed, enable access by entering:
```
module lode singularity
```

<br>

**Interactive Mode**

<br>

Run the srun command with additional flags such as the following:
```
srun --pty -p dept_cpu /bin/bash
```
Then enter the following line into the command prompt:
```
singularity exec "/net/dali/home/roth/shared/signalp/signalp_fast.sif" signalp6 --fastafile input_file.fasta --organism other --output_dir path_of_output_dir --format txt --mode fast

```
**Batch Mode**
