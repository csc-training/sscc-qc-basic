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

!["Potential energy surface"](../screens_20/1.png "potential energy surface")

## Task 1: Launch TmoleX and create a new project

!["Launch TmoleX GUI"](../screens_20/3.png "Launch TmoleX GUI")


## Task 1: Define your first turbomole job

!["update"](../screens_20/4.png "Launch TmoleX GUI")

#!["Some water"](img/10km.jpg "10000 water molecules"){width=30%}

Remember to add both title AND alt-text for screen reader
and accessibility!

# Columns 1

<div class="column">
- Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Sed posuere
  interdum sem.
    - quisque ligula eros ullamcorper quis
    - lacinia quis facilisis sed sapien
- Mauris varius diam vitae arcu.
</div>

<div class="column">
- Sed arcu lectus auctor vitae, consectetuer et venenatis eget velit.
- Sed augue orci, lacinia eu tincidunt et eleifend nec lacus.
</div>

# Columns 2

<div class="column">
- Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Sed posuere
  interdum sem.
    - quisque ligula eros ullamcorper quis
    - lacinia quis facilisis sed sapien
- Mauris varius diam vitae arcu.
</div>

<div class="column">
![Water](img/10km.jpg "10000 water molecules"){width=50%}
</div>

