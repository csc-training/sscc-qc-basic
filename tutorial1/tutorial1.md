# Tutorial 1: Basic DFT calculations using TmoleX and TURBOMOLE {.title}

* Spring School on Computational Chemistry 26-28 April 2023
* Atte Sillanpää, CSC - IT center for Science LTD, based on the work of Nino Runeberg

## Overview

1 TmoleX is a graphical user interface to set up, launch and run jobs with TURBOMOLE
1 This tutorial is made for use with TmoleX  23.1.0 and TUBOMOLE 7.7
1 We'll use a locally installed TmoleX to set up a formaldehyde geometry optimization
1 Configure and submit the job to be run on mahti.csc.fi supercomputer
1 Perform a frequency calculation on the resulting structure
1 Visualize results

## Remote usage setup

* kuva tmolexista läppärillä, login node, batch job, compute node, paluu
* define vaihtoehtona
* linkki env-effiin (ympäristö, batch stuff)

## Installing TmoleX

* Download from [Dassault Systemes web site](https://www.3ds.com/products-services/biovia/products/molecular-modeling-simulation/solvation-chemistry/turbomoler/)
  * Note, running TURBOMOLE requires a license, which CSC has for Puhti and Mahti.
  The GUI works for preparing/analysing inputs also without license, but please consult the license agreement.
  * On the CSC workstations TmoleX is already installed, launch from icon/menu
  * You can also use a version from your laptop
  * Note: if you don't want to use the TmoleX GUI, you can follow the tutorial2, which uses the `define` command line tool to prepare the inputs

## Task 1: Optimize the ground state for formaldehyde

We need an initial guess for the geometry specifying the 3N-6 internal
nuclear coordinates. This initial structure place the system on the
energy surface that is uniquely defined by the computational model
we are going to use (B-O approx.).  The performance of the model
often vary at different parts of the surface.

!["Potential energy surface"](../screens_20/2.png "potential energy surface")

## Task 1: Launch TmoleX and create a new project

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

Here we define the remote (supercomputer) configuration:
   * Which user account and project to use
   * Where TURBOMOLE is installed
   * etc.
   * Note, this will differ for every user and machine
   * In the Spring School 2023 we'll be using Mahti, but see here for the 
     general instructions for Puhti [docs.csc.fi/apps/tmolex/](https://docs.csc.fi/apps/tmolex/)

!["update"](../screens_20/10.png "upt")


!["update"](../screens_20/7.png "upt")
!["update"](../screens_20/7.png "upt")
!["update"](../screens_20/7.png "upt")


