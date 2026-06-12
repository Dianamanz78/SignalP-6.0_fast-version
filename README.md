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

**Flags**
```
-required
- `--fastafile`, `-ff`, specifies the fasta file with the sequences to be predicted.
To prevent invalid file paths, non-alphanumeric characters in fasta headers are replaced with "`_`" for saving the individual sequence output files.

- `--output_dir`, `-od`, speicifies the directory in which to save the outputs. If it does not exist, it will be created. Note that repeated calls with the same `--output_dir` will overwrite previous prediction results.
```

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

<br>

-include the line in the script file
```
singularity exec --bind $PWD "/net/dali/home/roth/shared/signalp/signalp_fast.sif" signalp6 --fastafile input_file.fasta --organism other --output_dir path_of_output_dir --format txt --mode fast
```
Then submit the script to SLURM using sbatch:
```
sbatch script.sh
```

