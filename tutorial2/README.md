# Tutorial 2: Find the highest formation energy of given isomers

* Spring School on Computational Chemistry 26-28 April 2023
* Atte Sillanpää, CSC - IT center for Science LTD

## Overview

1. Pick one molecule out of the 31 with the same composition (C11H14O2N2)
1. Mark your name on the row 
1. Set up a calculation to get the minimum energy conformation
1. Tabulate the total energy in the table above with the molecule names
1. Can you explain why some isomers are lower in energy than others?
1. **Bonus** Would the order change if you'd change the model? Improve basis set? Add an implicit solvent model? Include entropy estimate?

## Login to Mahti and pick a molecule for yourself

Open a terminal (right click at the background on Dogmi workstations) and login to Mahti

```bash
ssh -Y your-username@mahti.csc.fi
```

Load the TURBOMOLE module and navigate to the `scratch` area (disk area to perform computations)

```bash
module load turbomole/7.7
cd /scratch/project_2006657
```

Create a directory for yourself so that your results will not overwrite each other (remember
to change `your-username`to your actual username) 

```bash
mkdir your-username
```

The isomers are available in the directory `/scratch/project_2006657/xyz` In the same directory
there's a `table.csv` with the name of the molecule, and some other columns. Write your name on the
row of the molecule you want to calculate. Pick different molecules. Use e.g. `nano` editor.

```bash
nano /scratch/project_2006657/xyz/table.csv
```

## The job setup can be done from the command line using the tool `define`

This sequence with set up a similar geometry optimization as in tutorial 1

## Change into your own folder and create a new folder for your first isomer

On Mahti commmand line `cd` to the directory you just created above and
(replace the-number-of-... with the actual number you selected above)

```bash
mkdir the-number-of-isomer-you-selected # e.g. mkdir 447789
cd the-number-of-isomer-you-selected
```

Copy the structure in the current folder

```bash
cp /scratch/project_2006657/xyz/the-number-of-isomer-you-selected.xyz . # e.g. cp /scratch/project_2006657/xyz/447789.xyz .
```

Note the last dot!

## Convert the xyz-structure into TURBOMOLE format

```bash
xyz2coord the-number-of-isomer-you-selected.xyz
```

## Launch `define` to set up the job

```bash
define
```

which will print out information and prompts for us to react.

```bash
 define (mahti-login11.mahti.csc.fi) : TURBOMOLE rev. V7-7 compiled 19 Oct 2022 at 01:15:37
 Copyright (C) 2022 TURBOMOLE GmbH, Karlsruhe


    2023-04-23 16:57:07.897

                    HOST NAME = mahti-login11.mahti.csc.fi
             OPERATING SYSTEM = unix
   STANDARD BASIS SET LIBRARY = /appl/soft/chem/turbomole/7.7/TURBOMOLE/basen/
  ALTERNATE BASIS SET LIBRARY = /appl/soft/chem/turbomole/7.7/TURBOMOLE/basold/
 LIBRARY FOR RI-J  BASIS SETS = /appl/soft/chem/turbomole/7.7/TURBOMOLE/jbasen/
 LIBRARY FOR RI-JK BASIS SETS = /appl/soft/chem/turbomole/7.7/TURBOMOLE/jkbasen/
 LIBRARY FOR RIMP2/RICC2 SETS = /appl/soft/chem/turbomole/7.7/TURBOMOLE/cbasen/
 LIBRARY FOR RIR12 BASIS SETS = /appl/soft/chem/turbomole/7.7/TURBOMOLE/cabasen/
 LIBRARY FOR OEP   BASIS SETS = /appl/soft/chem/turbomole/7.7/TURBOMOLE/xbasen/
            STRUCTURE LIBRARY = /appl/soft/chem/turbomole/7.7/TURBOMOLE/structures/


 ***********************************************************
 *                                                         *
 *                       D E F I N E                       *
 *                                                         *
 *         TURBOMOLE'S  INTERACTIVE  INPUT  PROGRAM        *
 *                                                         *
 *  Quantum Chemistry Group       University of Karlsruhe  *
 *                                                         *
 ***********************************************************


 DATA WILL BE WRITTEN TO THE NEW FILE control

 IF YOU WANT TO READ DEFAULT-DATA FROM ANOTHER control-TYPE FILE,
 THEN ENTER ITS LOCATION/NAME OR OTHERWISE HIT >return<.

```

Accept this with `enter`.

Next, `define` will ask for a title

```bash

 INPUT TITLE OR
 ENTER & TO REPEAT DEFINITION OF DEFAULT INPUT FILE
```

which you can give or skip by another `enter`. Then we'll get into business.


## Setting up geometry 

Define will ask the same questions that were given on the TmoleX GUI. We start with the geometry. To load the geometry, give

The relevant line in the options is

` a <file>         : ADD ATOMIC COORDINATES FROM FILE <file>`

and the input to load the already prepared coordinates from the file `coord` is

```bash
a coord
```

## Coordinate representation

Let's choose and create redundant internal coordinates, which are often the most robust and efficient way to use in geometry optimizations. This is the relevant line in the options:

` ired             : REDUNDANT INTERNAL COORDINATES`

and it's activated simply with

```bash
ired
```

You can view the analysis of the structure and resulting coordinates.
If there were no
errors, we're done with the geometry and can proceed to the next "dialog" by `*` and `enter`.

```
 *                : TERMINATE MOLECULAR GEOMETRY SPECIFICATION
                     AND WRITE GEOMETRY DATA TO CONTROL FILE
```

`*` will be the equivalent of `next` in define screens. You can get to the previous menu with `&`.

## Choose the basis set

As the first geometry optimization let's use a simple and quick basis set `def-SV(P)`. The relevat option is

```
 bb   : b RESTRICTED TO BASIS SET LIBRARY
```

To select that for all atoms

```bash
bb all def-SV(P)
```

Move to next step with `*`.

## Create start values for orbital coefficients

Select `eht` from the menu to perform an Extended Hückel Guess.

You will be displayed with atomic orbitals will be used to
set up the guess and asked if you want to with the default values

```
 DO YOU WANT THE DEFAULT PARAMETERS FOR THE EXTENDED HUECKEL CALCULATION ?
 DEFAULT=y   HELP=?
```

Reply `y` (or just enter).

You will be asked for the total charge:

```
ENTER THE MOLECULAR CHARGE  (DEFAULT=0)
```

The default is 0, which we will use also (enter).

You will be displayed the results and occupations around HOMO, which you can accept.

You will be taken to th method selection.

## Choose the job type from the general menu

We want to run a Density Functional Calculation and use the "resolution of identity" approximation
which speeds up the calculation with minimal compromise in accuracy. The relevant section in the menu

```
 dft    : DFT Parameters
 ri     : RI Parameters
```

First choose `dft` , enter and then in the new menu select `on`. Hit enter once more to get
back to the general menu. Then select `ri` and similarly in the new menu `on` and enter to
get back to the general menu.

Now we have set up the input and are ready to exit from `define` with `*`.

## Create a batch job and launch the computation

All the input files TURBOMOLE needs are now in the current folder. We still need to
tell the queuing system what kind of resources we want to use. Example batch scripts
for TURBOMOLE can be found on [CSC's user guide](https://docs.csc.fi/apps/turbomole/)
for both Puhti and Mahti and for some typical different more detailed use cases.

To be used in the Spring School 2023, please use the template below. Open a new
file (e.g. `jobscript.sh`) with some editor (e.g. `nano`)in the directory where
you have the TURBOMOLE input files, paste the below
template and edit to match your needs.

```bash
#!/bin/bash
#SBATCH --partition=interactive
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=4         # MPI tasks per node
#SBATCH --account=project_2006657   # the project to be billed
#SBATCH --reservation=sscc_thu_int  # available only on Thursday 27.4.2023
#SBATCH --time=00:10:00             # time as `hh:mm:ss`

export PARA_ARCH=MPI          # use MPI
module load turbomole/7.7
export SLURM_CPU_BIND=none
# This setting of TURBOTMPDIR assumes that the job is
# submitted from a directory below /scratch/<project>
export TURBOTMPDIR=`echo $PWD |cut -d'/' -f1-3`"/TM_TMPDIR/"$SLURM_JOB_ID
mkdir -p $TURBOTMPDIR
export PARNODES=$SLURM_NTASKS  # for MPI
export PATH=$TURBODIR/bin/`$TURBODIR/scripts/sysname`:$PATH
jobex -ri -c 50 > jobex.out
```

Submit the job from the command line with:

```bash
sbatch jobscript.sh
```

You can follow the progress of the job from the files in the directory
or with the slurm commands `sacct` or `squeue -u $USER`. Note, the slurm
commands report twice the number of CPU cores since in Mahti it's possible
to run two virtual cores on a physical core (hyperthreading).

Once the job has finished look at the final SCF energy from the end
of the `energy` file or `total energy` towards the end of the `job.last` file.

Insert the total energy in the table for your molecule. The file is in
`/scratch/project_200657/xyz/table.csv`

## Visualize the structure

### TmoleX

Copy the whole folder to your local computer and open it with TmoleX. If you want
only to look at the final structure, you can copy only that one.

On your local (linux or MAC) computer (or some terminal program on Windows)

```bash
scp -r your-username@mahti.csc.fi:/scratch/project_200657/your-username/XXX .
```

where XXX is the folder with the results. In the TmoleX GUI, select "File" -->
"Open Job/Control File".

### Directly on Mahti

If you can tunnel graphics from Mahti, you can convert the TURBOMOLE `coord` file
to xyz-format:

```bash
t2x -c > final.xyz
```

Initialize the Molden -program

```bash
module load molden
```

and use it to look at the geometry

```bash
molden final.xyz
```

To quit Molden, click on the skull-icon at the center of the control menu.


## Task 2: Improve the basis set

You can continue from the end of the previous calculation by editing
the `control` file with new instructions using the `define` tool.

TURBOMOLE can use the final molecular orbitals, potential energy curvature
information etc. to speed up the new calculation.

To change the basis set, run `define` and 

Accept the first three questions, but to change the basis sets
select `y` in this point:

```
 ATOMIC ATTRIBUTE DATA (BASES,CHARGES,MASSES,ECPS) HAVE BEEN
 TAKEN FROM THE DEFAULT INPUT FILE control.
 DO YOU WANT TO CHANGE THESE DATA ? DEFAULT=n  GOBACK=&
```

Let's select a triple zeta valence basis set with additional
polarization functions. Give

```bash
bb all def-TZVP
```

Proceed to next screen with `*`. You will get a question about the auxiliary basis
sets, which correspond to the previously used basis set, which are still referred
to in the input. Agree to delete them with `d` and we'll add the matching ones
later.

Perform the `eht` calculation with the defaults and keep the energy, grad & dipole data.

In the general menu select `ri` and accept the suggested auxiliary basis files for
each element.

Exit `define` with `*`.

Before you launch the job, let's take a copy of the current input files:

```bash
tar czf ../tzp-input.tar *
```

The idea is to be easily able to rerun the job with a different number of cores later
to check how much resources can be efficiently used.

Submit the job with

```bash
sbatch jobscript.sh
```

While the job is running, you can prepare another. Make a new directory and untar the
contents there:

```bash
cd ..
mkdir scaling-8
cd scalning-8
tar xf ../tzp-input.tar
```

You should now have the same situation as just before the launch of the previous job.

Edit the slurm parameters to use twice the amount of CPU cores.

Open the jobscript.sh file in editor and edit 4 --> 8, or 8 --> 16, but also 1 and 2. Each in
a new directory so that you don't overwrite your results **and** that you always start
from the same initial quess.

Also, run a single point energy instead of a geometry optimization. Change the


```
jobex -ri -c 50 > jobex.out
```
to
```
ridft > singlepoint
```

Submit the job(s) with

```bash
sbatch jobscript.sh
```

## Examine how long it took to run the job

You will find many numbers. Ones you can get with `sacct`. Find the JOBID
(a long number that identifies your job ((tip: look at the slurm output
or use `sacct` ... or more brutally leave out `-j JOBID`)) and give

```bash
sacct -X -j JOBID -o state,start,alloc,elapsed,cputime
     State               Start  AllocCPUS    Elapsed    CPUTime
---------- ------------------- ---------- ---------- ----------
 COMPLETED 2023-04-23T19:03:40          4   00:00:13   00:00:52

```

In the pentultimate column you'll see how long the queuing system was keeping the resources for you
(Elapsed == the walltime) and in the final column that times the number of reserved cores.

Two other numbers you'll get from the `singlepoint` output log file. Look for the word `time`
and you'll see something like this

```bash
grep time singlepoint
         total  cpu-time :   6.93 seconds
         total wall-time :   8.21 seconds
```

Tabulate the values into a table. You can calculate the "speedup" by
dividing the 1 core time with N core time. Linear speedup would equal
the number of cores used.


|  cores     |   walltime (slurm) | cpu-time from logfile | wall-time from logfile | speedup slurm | speedup cpu-time from logfile |
| ---------- | ----------- | ----------: | --------: | ------: | ------: |
|    1       |             |             |           |         |         |
|    2       |             |             |           |         |         |
|    4       |             |             |           |         |         |
|    8       |             |             |           |         |         |


## Discussion

* Why do the numbers differ?
* Why do these number matter?
* Are these numbers accurate?
* How to get more accurate information?
* What else affects the performance?
* Is Mahti the right place for these kinds of calculations?

This ends the tutorial.







