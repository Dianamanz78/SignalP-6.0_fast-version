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
-required:
```
- `--fastafile`, `-ff`, specifies the fasta file with the sequences to be predicted.
To prevent invalid file paths, non-alphanumeric characters in fasta headers are replaced with "`_`" for saving the individual sequence output files.

- `--output_dir`, `-od`, speicifies the directory in which to save the outputs. If it does not exist, it will be created. Note that repeated calls with the same `--output_dir` will overwrite previous prediction results.
```

-optional:
```
- `--organism`, `-org`, is either `other` or `eukarya`. Specifying `eukarya` triggers post-processing of the SP predictions to prevent spurious results (only predicts type Sec/SPI).   
Defaults to `other`.

- `--format`, `-fmt`, can take the values `txt`, `png`, `eps`, `all`, `none`. It defines what output files are created for individual sequences. `txt` produces a tabular `.gff` file with the per-position predictions for each sequence. `png`, `eps`, `all` additionally produce probability plots in the requested format. `none` only writes the summary prediction files. For larger prediction jobs, plotting will slow down the processing speed significantly.  
Defaults to `txt`.

- `--mode`, `-m`, is either `fast`, `slow` or `slow-sequential`. Default is `fast`, which uses a smaller model that approximates the performance of the full model, requiring a fraction of the resources and being significantly faster. `slow` runs the full model in parallel, which requires more than 14GB of RAM to be available. `slow-sequential` runs the full model sequentially, taking the same amount of RAM as `fast` but being 6 times slower. If the specified model is not installed, SignalP will abort with an error.   
Defaults to `fast`.

- `model_dir`, `-md` allows you to specify an alternative directory containing the SignalP 6.0 model weight files. Defaults to the location that is used by the installation commands. Does not need to be specified when following the default installation instructions.

#### Performance optimization

- `--bsize`, `-bs` is the integer batch size used for prediction. When running on GPU, this should be adjusted to maximize usage of the available memory. On CPU, the choice usually has only a limited effect on performance. Defaults to `10`.

- `--torch_num_threads`, `-tt` is the number of threads used by PyTorch. Defaults to `8`.

- `--write_procs`, `-wp` is the integer number of parallel processes launched for writing output files. Using multiple processes significantly speeds up writing the outputs for prediction jobs with many sequences. However, due to the way multiprocessing works in Python, this leads to increased memory usage. By setting to `1`, no additional processes are started. Defaults to the number of available CPUs with `8` processes maximum.
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

