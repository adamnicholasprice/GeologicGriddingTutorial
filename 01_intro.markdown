---
layout: post
title:  "Introduction"
nav_order: 2
has_children: true
---

# Geologically Realistic Representations of Seafloor Crustal Relief and Patchy Sediment Cover

Adam N. Price (adamnicholasprice@gmail.com); July 2022

# Introduction

This tutorial is a resource that accompanies a robust set of resources including a publication, psuedo-code, github repository, and data repository for constructing a three-dimensional grid of geologically realistic representations of seafloor crustal relief. The three-dimensional simulation domain described here has been used in multiple publications focusing on the thermal and hydrogeologic regime of coupled fluxes beneath a marine sediment pond (Price et al., 2022; Price et al., in-prep).

The tutorial can be broken down into a workflow consisting of 5 main sections: constructing surfaces, constructing a domain, refining the domain, defining and assigning material types to regions, and a general clean-up of the domain. There will be two examples included for each step. One example will use a general case that will introduce the concepts in a simple manner and the other example will be for construction of the North Pond domain.

This tutorial will not be an exhaustive tutorial on the functionality of LaGriT as there are many examples that can be found [here](https://lanl.github.io/LaGriT/pages/tutorial/index.html). Instead, this tutorial will serve as a &quot;cookbook&quot; that allows the user to understand the steps used in construction of this simulation domain using the lessons and approaches explained below.

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
lagrit \&lt; _filename_
```

This will be denoted with the phrase execute _filename.lgi_ in LaGriT.

Steps within the tutorial will be broken down by LaGriT file and have necessary inputs and outputs specified as such:

| **FIlename** | **Input** | **Output** |
| --- | --- | --- |
| _ **filename.lgi** _ | _inputFilename_ | _outputFilename_ |

### File types

| **Extension** | **Filetype** | **Purpose** |
| --- | --- | --- |
| _ **\*.lgi** _ | LaGriT control file | LaGriT input control file, specifies mesh generation. |
| _ **\*.mlgi** _ | LaGriT control file | LaGriT input control file used to specify a subroutine within \*.lgi file. |
| _ **\*.inp** _ | AVS UCD (Unstructured Cell Data) file | Created by LaGriT and contains information about mesh objects. Can be opened in Paraview to visualize data. |
| _ **\*.zone, \*.area, \*.stor, \*.fehmn** _ | FEHM files | LaGriT output files that are of general use but are specifically designed for the[FEHM](http://fehm.lanl.gov/) porous flow and transport code |
| _ **outx3dgen** _ | LaGriT output file | Output file containing commands and output from commands executed in LaGriT (overwritten every time LaGriT command is called) |
| _ **legx3dgen** _ | LaGriT log file | Log file containing commands executed in LaGriT (overwritten every time LaGriT command is called) |
| _ **\*.csv** _ | Text file (comma separated values) | In this tutorial, these are text files containing coordinates in meters for surfaces. Columns are x, y, z coordinates respectively. |

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

# Surfaces

The first step in the construction of a simulation domain is to construct surfaces that represent geologically realistic layers. These surfaces will define boundaries that allow assignment of specific material properties and act as surfaces to refine the simulation domain. Surfaces can be constructed in LaGriT or in an outside programming language such as R, Python, or Matlab and brought into LaGritT. In both cases in this tutorial, surfaces were constructed outside of LaGriT and brought in to define mesh objects. The general steps to do this are:

1. Save mesh coordinates (x,y,z) into a .csv file

2. Create mesh object that has coordinate system (x and y) of desired mesh resolution (have to define if different from .csv file coordinates)

3. Connect mesh objects with new coordinates in x and y.

4. Read in the .csv as a mesh object into LaGriT

5. Copy over z-coordinates from mesh in step 4 to connected mesh in step 2

This process forces LaGriT to make the &quot;best&quot; connections within an x-y plane to begin with and then the coordinates along the z-axis with relief to be defined, thereby preventing any connections outside the plane.

## Generalized Mesh

## North Pond

For the North Pond case, the first seven files focus on defining surfaces.

| **FIlename** | **Input** | **Output** |
| --- | --- | --- |
|
### 1\_price\_surf\_domain.lgi
 | _function6.csv_ | _1\_surf\_domain.inp_ |

#### Overview

Create mesh object surface representing North Pond bathymetry using function6.csv

| **FIlename** | **Input** | **Output** |
| --- | --- | --- |
|
# 2\_price\_surf\_mid\_grid.lgi
 | 2\_middle\_grid.csv | 2\_surf\_mid\_grid.inp |

## **Overview**

Create mesh object surface from trimmed area of interest created from truncated bathymetric surface (_2\_middle\_grid.csv_) representing North Pond and immediately adjacent region.

| **FIlename** | **Input** | **Output** |
| --- | --- | --- |
|
# 3\_price\_surf\_mid\_sb.lgi
 | 3\_middle\_sb.csv | 3\_surf\_mid\_sb.inp |

## **Overview**

Create mesh object surface from trimmed area of interest created from truncated bathymetric surface (_3\_middle\_sb.csv_) representing sediment / basement interface beneath North Pond.

| **FIlename** | **Input** | **Output** |
| --- | --- | --- |
|
# 4\_price\_surf\_flat\_zero.lgi
 | None | 4\_surf\_flat\_zero.inp |

## **Overview**

Construct a flat surface at z = 0 at the same spatial extent as _2\_middle\_grid.csv_.

| **FIlename** | **Input** | **Output** |
| --- | --- | --- |
|
# 5\_price\_surf\_aq100.lgi
 | 3\_surf\_mid\_sb.inp | 5\_surf\_aq100.inp |

## **Overview**

Construct a volume that represents a 100m thick aquifer section directly beneath North Pond.

###

| **FIlename** | **Input** | **Output** |
| --- | --- | --- |
|
# 6\_price\_surf\_aq300\_1k.lgi
 | 2\_surf\_mid\_grid.inp | 6\_surf\_aq300\_1k.inp |

## **Overview**

## Construct a volume that represents a 900 m thick aquifer section directly beneath the region representing North Pond.

| **FIlename** | **Input** | **Output** |
| --- | --- | --- |
|
# 7\_price\_surf\_filt.lgi
 | 1\_surf\_domain.inp | 7\_surf\_filt\_aq100.inp |
| 7\_surf\_filt\_aq300.inp |
| 7\_surf\_filt\_aq600.inp |
| 7\_surf\_filt\_aq1k.inp |

## **Overview**

Construct four surfaces at -100, -300,-600, and -1000m at the extent of the entire domain.

# Domain

[Placeholder]

# Refinement

[Placeholder]

# Define regions

[Placeholder]

# Cleanup

[Placeholder]

# PseudoCode

- General

- North Pond

# Links

[https://lagrit.lanl.gov/docs/Tutorial.shtml](https://lagrit.lanl.gov/docs/Tutorial.shtml)
