# Tutorial 1: Basic DFT calculations using TmoleX and TURBOMOLE

* Spring School on Computational Chemistry 17-19 April 2024
* Nino Runeberg, CSC - IT center for Science Ltd, based on the earlier work of Atte Sillanpää

## Overview

1. TmoleX is a graphical user interface to set up, launch and run jobs with TURBOMOLE
1. This tutorial is made for use with TmoleX 24 and TUBOMOLE 7.8
1. We'll use TmoleX to set up a formaldehyde geometry optimization
1. Configure and submit the job to be run on puhti.csc.fi supercomputer
1. Perform a frequency calculation on the resulting structure
1. Visualize results

## Remote usage setup at CSC

* TmoleX can be used either via a browser or by running it on your local 
  computer (see [Preparations](../preparations/README.md) )   
* The model system and input parameters for the TUBOMOLE job are specified  using TmoleX
* A connection to supercomputer puhti.csc.fi is set up
  in TmoleX using ssh. (The password is cleared upon closing the software)
* Installation directory and supercomputer specific options are set in TmoleX so that the GUI can
  launch the job for the queueing system (SLURM) on the Puhti
* TmoleX can follow progress and once the job can completed, dowload the results to the local computer
* The CSC TmoleX page has more information.
* The overall CSC supercomputer environment can be found in the [Docs CSC user guide](https://docs.csc.fi/computing/available-systems/)
  or in the [CSC Computing environment self learning course materials](https://csc-training.github.io/csc-env-eff/).
* An option is to use `ssh` and login directly on the Mahti login node, and prepare the job with `define`.
d, launch from icon/menu
!["Using TmoleX network scheme"](../screens_20/tmolex-and-mahti.svg "Using TmoleX network scheme")

## Task 1: Optimize the ground state for formaldehyde

We need an initial guess for the geometry specifying the 3N-6 internal
nuclear coordinates. This initial structure place the system on the
energy surface that is uniquely defined by the computational model
we are going to use (B-O approx.).  The performance of the model
often vary at different parts of the surface.

!["Potential energy surface"](../screens_20/pesurf.png "potential energy surface")

## Task 1: Launch TmoleX and create a new project

### Use via your browser

Go to [puhti.csc.fi](https://puhti.csc.fi/) using a web browser and login using
your CSC/Haka user account.

1. From there [launch a Desktop](https://docs.csc.fi/computing/webinterface/desktop/#launching). 
2. Open a `Terminal` and load the TURBOMOLE module `module load turbomole/7.8`.
3. Start TmoleX with the command `TmoleX24`.
4. Select `New Project` and define a suitable project in the `File Name` slot
   (e.g. `/scratch/project_2006657/$USER/SSCC24_project`).
5. Define your system and type of calculation. 
6. Small jobs can be run interactively: Start Job -> Run (local)
7. Larger jobs should be run as batch jobs: Start Job -> Run (network). Example
   settings are given below. Note that passwordless connection doesn't work via
   the browser. Remember to save the settings using `Save Machine`.
 
###  Use locally installed TmoleX

* If you have installed TmoleX on your own laptop, launch it from icon/menu
* On the CSC workstations TmoleX is already installed, launch it from icon/menu
* Note. TmoleX warns about not finding a license. This is ok. We'll use the TURBOMOLE license
on Mahti for the calculations. Accept the dialog.

!["Launch TmoleX GUI"](../screens_20/3.png "Launch TmoleX GUI")

## Task 1: Define your first turbomole job

!["update"](../screens_20/4.png "Launch TmoleX GUI")

A complete Turbomole job comprises the sequence:

* **Geometry - Atomic Attributes - Molecular Attributes - Method - Start Job - Results**

The TURBOMOLE philosophy or program structure is based on running different
"modules" one after another. The following diagram
[taken from a TURBOMOLE tutorial](https://www.turbomole.org/wp-content/uploads/2019/10/Tutorial_7-4.pdf)
highlights the most typical ones and their relation.

!["TURBOMOLE modules"](../screens_20/tmoleDefineFlow.png "TURBOMOLE modules")


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
   * **Note** The "Script" box doesn't show all the commands, see the instructions below the picture.

!["update"](../screens_20/10.png "upt")

Use these:

* Machine/IP: `mahti.csc.fi`
* User: `<your-username>`
* Work directory: `/scratch/project_2006657/<your-username>`
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

## Task 1: Results -- Gradients

The length of the arrows show how steep the energy surface is in that direction

!["update"](../screens_20/13.png "upt")

At the end of the geometry optimization we have reached a stationary point
(gradient smaller than a given threshold) that could correspond to:

!["Stationary points on potential energy surface"](../screens_20/stationaryPoints.png "Stationary points on potential energy surface")

* a minimum **A**
* inflection point **B**
* a maximum **C**

!["gradients"](../screens_20/gradient.png "upt")

The nature of the stationary point can be deduced from the curvature (Hessian).
A positive curvature corresponds to a minimum, a negative to a maximum.

## Task 1: Vibrational spectrum

In order to verify that the stationary point is a true minimum
(positive curvature in all directions = positive frequencies)

Start a frequency calculation (Reuse data by just hitting "Start new job by using current data as input" )

!["Start new job from previous results"](../screens_20/14.png "upt")

In the "Job typ" list select "Spectra & Excited States --> IR & vibrational frequencies"

!["Select of frequency calculation"](../screens_20/15.png "upt")

Select "Run (Network)" to launch the job.

Once the job finishes (you can refresh the view - wait for results to get downloaded).
You can also log in on the supercomputer

```bash
ssh -Y your-username@mahti.csc.fi
```

and follow the job status with slurm commands more on these e.g. in
(CSC Computing Environment self learning materials)[https://csc-training.github.io/csc-env-eff/hands-on/batch_resources/tutorial_sacct_and_seff.html]
directly, e.g. with:

```bash
squeue -u $USER # my current running or queuing jobs
sacct           # my ended jobs for the last day
sacct -X -o jobid,start,jobname,state,elapsed,alloc # last jobs with custom fields
```

A local terminal on Mahti also gives access to some additional tools and scripts
that come with TURBOMOLE that can be nice to follow or post process the data. You
can find these e.g.

```bash
module load turbomole
ls $TURBODIR/scripts/
cgnce -h # to get help on usage
actual -h 
```

All calculated frequencies are positive indicating that the structure corresponds to a true minimum.

!["Frequency calculation results"](../screens_20/16.png "upt")

The zero Kelvin minimum energy structure in a vacuum is often the starting point in solving
chemical problems. It often represents surprisingly well the molecular properties despite
all the approximations made. What could you use this information for? How to validate or improve
the model?

This ends the tutorial.







