# Spring School on Computational Chemistry Basic QC hands-on

This repository contains hands-on tutorials to done
using TmoleX graphical user interface and the TURBOMOLE
engine running on a supercomputer. Note, that the latter
requires a license, which is available for academic
users on the CSC supercomputers.

## [Preparations](./preparations/README.md)

* Setting up TmoleX either via a Browser or by installing it on your own laptop 

## [Tutorial 1](./tutorial1/README.md)

* Simple DFT calculation using TmoleX to set up the geometry and job specifics
* Run the job remotely via TmoleX
* Visualize results locally

## [Tutorial 2](./tutorial2/README.md)

* Multiple geometry optimizations of a number of isomers
* Setting up the jobs with `define` directly on supercomputer
* Submit the job to slurm from the command line with a batch script
* Switch to a higher quality basis set for a single point energy calculation
* Benchmark resource usage by looking at how long the jobs take vs used resources

