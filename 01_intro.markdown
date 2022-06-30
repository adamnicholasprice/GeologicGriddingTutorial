---
layout: home
title:  "Introduction"
nav_order: 2
has_children: true
---

# Geologically Realistic Representations of Seafloor Crustal Relief and Patchy Sediment Cover

### Adam N. Price [adamnicholasprice@gmail.com] (adamnicholasprice@gmail.com);
#### July 2022


# Introduction

This tutorial is a resource that accompanies a robust set of resources including a publication, psuedo-code, github repository, and data repository for constructing a three-dimensional grid of geologically realistic representations of seafloor crustal relief. The three-dimensional simulation domain described here has been used in multiple publications focusing on the thermal and hydrogeologic regime of coupled fluxes beneath a marine sediment pond (Price et al., 2022; Price et al., in-prep).

The tutorial can be broken down into a workflow consisting of 5 main sections: constructing surfaces, constructing a domain, refining the domain, defining and assigning material types to regions, and a general clean-up of the domain. There will be two examples included for each step. One example will use a general case that will introduce the concepts in a simple manner and the other example will be for construction of the North Pond domain.

This tutorial will not be an exhaustive tutorial on the functionality of LaGriT as there are many examples that can be found [here](https://lanl.github.io/LaGriT/pages/tutorial/index.html). Instead, this tutorial will serve as a &quot;cookbook"&quot"; that allows the user to understand the steps used in construction of this simulation domain using the lessons and approaches explained below.

Happy gridding!

## Data and software requirements

To complete this tutorial you will need a stable version of LaGriT (version) and optionally Paraview.

### LaGriT

LaGriT is available at [https://github.com/lanl/LaGriT/releases](https://github.com/lanl/LaGriT/releases) and can be downloaded as a compiled version (preferred) or the source code for compiling. After downloading LaGriT, add the file to a location within your path, and modify the file permissions to executable.

### Paraview (optional)

Paraview is a powerful visualization platform used for the primary grid viewer in this tutorial. Graphics here will be provided inline so there is no need to download for the purpose of this tutorial but it is recommended to explore the full functionality of this workflow. Paraview can be downloaded at[https://www.paraview.org/download/](https://www.paraview.org/download/)

### Data files

This tutorial utilizes scripts and data files from a github repository that can be found at [https://github.com/adamnicholasprice/GeologicGriddingData](https://github.com/adamnicholasprice/GeologicGriddingData). This repository can be cloned via the command line or downloaded directly from the github website. There are four directories:

• GeneralLaGriT - LaGriT scripts and csv surfaces for building the general grid case

• GeneralScene - Paraview scene files for general grid case used in this tutorial

• NorthPondLaGriT - LaGriT scripts and csv surfaces for building the North Pond grid case

• NorthPondScene - Paraview scene files for North Pond grid case used in this tutorial

## Document styling, commands, and files

### Document style

Throughout the document italicized words willdenote either a file name or a variable within LaGrit.

Code blocks will be denoted by the following style:

```
Code here

# Comments will be denoted with an octothorpe (pound sign)
```
### Commands
In lieu of repeating this code chunk in each of the following steps in the tutorial, any LaGriT input files (.lgi) that are run in the tutorial are done so by executing the following command in the command line:
```
lagrit < filename
```

This will be denoted with the phrase execute *filename.lgi* in LaGriT.

Steps within the tutorial will be broken down by LaGriT file and have necessary inputs and outputs specified as such:

| **Filename** | **Input** | **Output** |
| --- | --- | --- |
| **filename.lgi* |*inputFilename* | *outputFilename*|

### File types

| **Extension** | **Filetype** | **Purpose** |
| --- | --- | --- |
| _ **\*.lgi** | LaGriT control file | LaGriT input control file, specifies mesh generation. |
| _ **\*.mlgi** | LaGriT control file | LaGriT input control file used to specify a subroutine within \*.lgi file. |
| _ **\*.inp** | AVS UCD (Unstructured Cell Data) file | Created by LaGriT and contains information about mesh objects. Can be opened in Paraview to visualize data. |
| _ **\*.zone, \*.area, \*.stor, \*.fehmn** _ | FEHM files | LaGriT output files that are of general use but are specifically designed for the[FEHM](http://fehm.lanl.gov/) porous flow and transport code |
| _ **outx3dgen** | LaGriT output file | Output file containing commands and output from commands executed in LaGriT (overwritten every time LaGriT command is called) |
| _ **legx3dgen** | LaGriT log file | Log file containing commands executed in LaGriT (overwritten every time LaGriT command is called) |
| _ **\*.csv** | Text file (comma separated values) | In this tutorial, these are text files containing coordinates in meters for surfaces. Columns are x, y, z coordinates respectively. |

##

## Getting started

The first step in starting this tutorial is making sure that LaGriT is executable within the working directory of this tutorial and ensuring all needed materials have been synced from github. The directory structure should be:

```

GeologicGriddingData/

├── GeneralLaGriT

│ ├── 1\_create\_surface.lgi

│ ├── 2\_refine\_domain.lgi

│ ├── 3\_set\_region\_truncate.lgi

│ ├── refine\_object.mlgi

│ └── surface

└── NorthPondLaGriT

├── 10\_price\_region\_set.lgi

├── 11\_price\_remove\_top.lgi

├── 12\_price\_top\_region\_set.lgi

├── 13\_price\_surf\_aq300\_bubbles.lgi

├── 14\_price\_aq300\_set.lgi

├── 1\_price\_surf\_domain.lgi

├── 2\_middle\_grid.csv

├── 2\_price\_surf\_mid\_grid.lgi

├── 3\_middle\_sb.csv

├── 3\_price\_surf\_mid\_sb.lgi

├── 4\_price\_surf\_flat\_zero.lgi

├── 5\_price\_surf\_aq100.lgi

├── 6\_price\_surf\_aq300\_1k.lgi

├── 7\_price\_surf\_filt.lgi

├── 8\_price\_driver\_high\_resolution\_clipped.lgi

├── 9\_price\_surf\_north\_pond\_seafloor.lgi

└── function6

```
[Constructing refining surfaces](http://adamnicholasprice.github.io/GeologicGriddingTutorial/02_surfaces.html){: .btn .btn-purple }
