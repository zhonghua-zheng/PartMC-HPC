# Multiple Scenarios with Scheduler

Zhonghua Zheng (zhonghua.zheng@outlook.com)

Last update: 2021/05/23



Note: If you turn mosaic off, you can’t have "do_optical" setup in the ".spec" file.

## Introduction

Here we would run multiple scenarios using NCSA's Scheduler

We can name the script as "case_mcm_300.sh". This script would launch 300 PartMC scenarios in parallel.

Here we have to modify the script accordingly (See INSTRUCTION within the script), then submit the job by executing `sbatch case_mcm_300.sh `

```bash
#!/bin/bash
#SBATCH --job-name=case_mcm_300
#SBATCH -n 301
#SBATCH -p sesebig 
#SBATCH --time=24:00:00
#SBATCH --mem-per-cpu=4000
# Email if failed run
#SBATCH --mail-type=FAIL
# Email when finished
#SBATCH --mail-type=END
# My email address
#SBATCH --mail-user=your_email@illinois.edu

# the parent_path contains cases/ folder
export SLURM_SUBMIT_DIR=/data/keeling/a/zzheng25/scenario_generator
# case folder name under cases/case_*
export case=case_mcm_300
# number of (scenarios+1)
export scenario_num_plus_1=301
# PartMC path
export PMC_PATH=/data/keeling/a/zzheng25/partmc-mcm/PartMC
# Path for results
export WORK_DIR=/data/keeling/a/zzheng25/d/mcm_test

## INSTRUCTION
# module load gnu/openmpi-3.1.6-gnu-9.3.0
# cd Scheduler
# mpif90 -o scheduler.x scheduler.F90
# mv scheduler.x ..
# chmod 744 partmc_submit.sh
# sed -i 's/\r//g' partmc_submit.sh 
# 
# Within partmc_submit.sh:
# - define your job name, e.g., "SBATCH --job-name=case_mcm_300"
# - define "#SBATCH -n XXX" (XXX should be the same as "scenario_num_plus_1")
# - define the user email
# - define the variables in export XXX
# 
# type: sbatch case_mcm_300.sh

######## Do not Change ########
# The job script can create its own job-ID-unique directory 
# to run within.  In that case you'll need to create and populate that 
# directory with executables and inputs
mkdir -p $WORK_DIR/$SLURM_JOB_ID
cd $WORK_DIR/$SLURM_JOB_ID
cp -r $PMC_PATH/build build
cp -r $PMC_PATH/src src

# Copy the scenario directory that holds all the inputs files
cp -r $SLURM_SUBMIT_DIR/cases/$case/scenarios .

# Copy things to run this job
# Need the scheduler and the joblist
cp $SLURM_SUBMIT_DIR/scheduler.x .
cp $SLURM_SUBMIT_DIR/cases/$case/joblist .

# Run the library. One core per job plus one for the master.
mpirun -np $scenario_num_plus_1 ./scheduler.x joblist /bin/bash -noexit -nostdout > log
```
