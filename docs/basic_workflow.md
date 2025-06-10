# Basic Workflow Guide

Let's say we are a user who needs to run a compute-intensive python script
called `my_script.py` in our cluster. Here is a step by step guide on how a
typical workflow could look like:

## 1. Create your job script

Each job you want to send to the cluster must include an additional file, your
`job.slurm`. Here's an example:

```bash
#!/bin/bash
#SBATCH --job-name=my_python_job    # Job name
#SBATCH --nodes=1                   # Run on one node
#SBATCH --ntasks=1                  # Run a single task
#SBATCH --cpus-per-task=4          # Number of CPU cores per task
#SBATCH --mem=16G                   # Memory limit
#SBATCH --time=01:00:00            # Time limit hrs:min:sec
#SBATCH --output=%j.log            # Standard output and error log
#SBATCH --mail-type=END            # Mail events (NONE, BEGIN, END, FAIL, ALL)
#SBATCH --mail-user=user@org.com   # Where to send mail

# Load any required modules
module load python/3.9

# Run the Python script
python my_script.py
```

### Explanation of directives:

- `--job-name`: Name that will appear in queue
- `--nodes`: Number of nodes to use
- `--ntasks`: Number of processes to run
- `--cpus-per-task`: CPU cores per process
- `--mem`: Memory per node
- `--time`: Maximum run time
- `--output`: Log file name (%j is replaced by job ID)

## 2. Connect to our bastion server

First step is to establish contact with our bastion server through SSH:

```bash
ssh -p xxxx dummyuser@123.456.789.012
```

## 3. Transfer your files

Use `scp` to copy your files to the cluster:

```bash
scp -P xxxx my_script.py job.slurm dummyuser@123.456.789.012:~/
```

## 4. Submit your job

Submit your job using the `sbatch` command:

```bash
sbatch job.slurm
```

## 5. Monitor your job

Check job status:

```bash
# View all your jobs in the queue
squeue -u $USER

# View detailed information about a specific job
scontrol show job <jobid>
```

## 6. Check results

Once your job is complete, you can find the output in the log file specified in
your job script (named as `<jobid>.log`).
