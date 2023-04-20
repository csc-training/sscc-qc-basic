# Tutorial 1: Basic DFT calculations using TmoleX and TURBOMOLE

* Spring School on Computational Chemistry 26-28 April 2023
* Atte Sillanpää, CSC - IT center for Science LTD, based on the earlier work of Nino Runeberg

## Overview

1. TmoleX is a graphical user interface to set up, launch and run jobs with TURBOMOLE
1. This tutorial is made for use with TmoleX  23.1.0 and TUBOMOLE 7.7
1. We'll use a locally installed TmoleX to set up a formaldehyde geometry optimization
1. Configure and submit the job to be run on mahti.csc.fi supercomputer
1. Perform a frequency calculation on the resulting structure
1. Visualize results

## Remote usage setup

* kuva tmolexista läppärillä, login node, batch job, compute node, paluu
* define vaihtoehtona
* linkki env-effiin (ympäristö, batch stuff)

## Installing TmoleX

* Download the DEMO from [Dassault Systemes web site](https://www.3ds.com/products-services/biovia/products/molecular-modeling-simulation/solvation-chemistry/turbomoler/)
  * Note, running TURBOMOLE requires a license, which CSC has for Puhti and Mahti.
  The GUI works for preparing/analysing inputs also without license, but please consult the license agreement.
* On the CSC workstations TmoleX is already installed, launch from icon/menu
* You can also use a version from your laptop
* Note: if you don't want to use the TmoleX GUI, you can follow the tutorial2, which uses the `define` command line tool to prepare the inputs

!["Dassault Systemes website"](../screens_20/1.png "Dassault Systemes website")

## Task 1: Optimize the ground state for formaldehyde

We need an initial guess for the geometry specifying the 3N-6 internal
nuclear coordinates. This initial structure place the system on the
energy surface that is uniquely defined by the computational model
we are going to use (B-O approx.).  The performance of the model
often vary at different parts of the surface.

!["Potential energy surface"](../screens_20/pesurf.png "potential energy surface")

## Task 1: Launch TmoleX and create a new project

* Note. TmoleX warns about not finding a license. This is ok. We'll use the TURBOMOLE license
on Mahti for the calculations. Accept the dialog.

!["Launch TmoleX GUI"](../screens_20/3.png "Launch TmoleX GUI")


## Task 1: Define your first turbomole job

!["update"](../screens_20/4.png "Launch TmoleX GUI")


A complete Turbomole job comprises the sequence:

* **Geometry - Atomic Attributes - Molecular Attributes - Method - Start Job - Results**


## Task 1: Geometry -- Build formaldehyde

Open the 3D builder, right-click on canvas and load formaldehyde from the library
!["update"](../screens_20/5.png "upt")

Close the builder and continue to Atomic Attributes

## Task 1: Atomic Attributes: Select basis set

Select the default def-SV(P) basis set

!["update"](../screens_20/6.png "upt")

Continue to Molecular Attributes

## Task 1: Molecular Attributes -- Generate initial guess MOs

Generate initial MOs by doing an extended Hückel calculation

!["update"](../screens_20/7.png "upt")

Continue to Method

## Task 1: Method -- Define your method

Select the default method (ri-dft BP86/m3)

!["update"](../screens_20/8.png "upt")

Continue to Start Job

## Task 1: Start Job -- Define your job type

We want to do a geometry optimization of the ground state.

*  **Note. How many CPUs?**

!["update"](../screens_20/9.png "upt")

Continue to run (network)

## Task 1: Run(network) -- Setup remote job

Click **save** as the first dialog prompts for the folder to use for the job files.

In the new dialog, we define the remote (supercomputer) configuration:
   * Which user account and project to use
   * Where TURBOMOLE is installed
   * etc.
   * Note, this will differ for every user and machine
   * In the Spring School 2023 we'll be using Mahti, but see here for the 
     general instructions for Puhti [docs.csc.fi/apps/tmolex/](https://docs.csc.fi/apps/tmolex/)
   * **important** Replace `your-username` with your actual username on CSC supercomputer! Also, in the `work-directory` field.

!["update"](../screens_20/10.png "upt")

Use these:

* Machine/IP: `mahti.csc.fi`
* User: `<your-username>`
* Work directory: `/scratch/project\_2006657/<your-username>`
* TURBOMOLE directory: `/appl/soft/chem/turbomole/7.7/TURBOMOLE`
* Use queuing system (tick)
* Submit with: `sbatch`
* Check status: `squeue -u $USER`

Script before job execution:

```bash
#SBATCH --partition=interactive
#SBATCH --account=project_2006657
#SBATCH --time=00:10:00
#SBATCH --ntasks=1
#SBATCH --reservation=sscc_thu_int
export MPI_USESRUN=1
export SLURM_CPU_BIND=none
```

Click **save machine** at the top right

Click **Start Job** at the bottom right

* For this tutorial, the default 1 minute interval to ping Mahti for the job status is ok, but for actual production jobs, that could be increased to e.g. 1 hour. The status can always be refreshed manually. This job should finish in seconds.

!["Refresh view"](../screens_20/11.png "upt")

## Task 1: Results -- structure

The geometry optimization needed 5 cycles to reach the stationary point on the energy surface.

!["Optimized geometry"](../screens_20/12.png "upt")

## Task 2: Results -- Gradients

The length of the arrows show how steep the energy surface is in that direction

!["update"](../screens_20/13.png "upt")

At the end of the geometry optimization we have reached a stationary point
(gradient smaller than a given threshold) that could correspond to:

* a minimum **A**
* inflection point **B**
* a maximum **C**

!["gradients"](../screens_20/gradient.png "upt")

The nature of the stationary point can be deduced from the curvature (Hessian).
A positive curvature corresponds to a minimum, a negative to a maximum.

## Task 1: Vibrational spectrum

In order to verify that the stationary point is a true minimum
(positive curvature in all directions = positive frequencies)
do a frequency calc (Reuse data \
by just hitting "Start new job by using current data as input" )

!["Start new job from previous results"](../screens_20/14.png "upt")

In the Job typ list select "Spectra & Excited States --> IR & vibrational frequencies"

!["Select of frequency calculation"](../screens_20/15.png "upt")

Select "Run (Network)" to launch the job.

Once the job finishes (you can refresh the view - wait for results to get downloaded).
You can also log in on the supercomputer and follow the job status with slurm commands
directly, e.g. with:

```bash
squeue -u $USER # my current running or queuing jobs
sacct           # my ended jobs for the last day
sacct -X -o jobid,start,jobname,state,elapsed,alloc # last jobs with custom fields
```

All calculated frequencies are positive indicating that the structure corresponds to a true minimum.

!["Frequency calculation results"](../screens_20/16.png "upt")






