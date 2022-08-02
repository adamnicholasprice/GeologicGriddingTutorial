---
layout: home
title:  "Introduction"
nav_order: 2
---

# Geologically Realistic Representations of Seafloor Crustal Relief and Patchy Sediment Cover
### Adam N. Price [adamnicholasprice@gmail.com](adamnicholasprice@gmail.com)
#### July 2022

<details markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

# Introduction

This tutorial is a resource that accompanies a robust set of resources including a publication, psuedo-code, [GitHub repository](https://github.com/adamnicholasprice/GeologicGriddingTutorial), and data repository for constructing a three-dimensional grid of geologically realistic representations of seafloor crustal relief. The three-dimensional simulation domain described here has been used in multiple publications focusing on the thermal and hydrogeologic regime of coupled fluxes beneath a marine sediment pond ([Price et al., 2022](https://agupubs.onlinelibrary.wiley.com/doi/epdf/10.1029/2021JB023158); Price et al., in-prep).

The tutorial can be broken down into a workflow consisting of 5 main sections: constructing surfaces, constructing a domain, refining the domain, defining and assigning material types to regions, and a general clean-up of the domain. There will be two examples included for each step. One example will use a general case that will introduce the concepts in a simple manner and the other example will be for construction of the North Pond domain.

This tutorial will not be an exhaustive tutorial on the functionality of LaGriT as there are many examples that can be found [here](https://lanl.github.io/LaGriT/pages/tutorial/index.html). Instead, this tutorial will serve as walkthrough; that allows the user to understand the steps used in construction of this simulation domain using the lessons and approaches explained below.

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

### Dockerfile
This tutorial is accompanied by a [GitHub repository](https://github.com/adamnicholasprice/GeologicGriddingTutorial) which includes a dockerfile that compiles LaGriT, clones this GitHub repo, and executes all gridding syntax to produce the geologically realistic grid described in this tutorial. Docker is a fairly easy program to use and takes away the issues of compiling LaGriT and "it works on your machine, but it won't work on my machine". If you have the time and knowledge, it is highly reccomended you use the dockerfile along with this tutorial. 

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
| _ ***.lgi** | LaGriT control file | LaGriT input control file, specifies mesh generation. |
| _ ***.mlgi** | LaGriT control file | LaGriT input control file used to specify a subroutine within *.lgi file. |
| _ ***.inp** | AVS UCD (Unstructured Cell Data) file | Created by LaGriT and contains information about mesh objects. Can be opened in Paraview to visualize data. |
| _ ***.zone, *.area, *.stor, *.fehmn** _ | FEHM files | LaGriT output files that are of general use but are specifically designed for the[FEHM](http://fehm.lanl.gov/) porous flow and transport code |
| _ **outx3dgen** | LaGriT output file | Output file containing commands and output from commands executed in LaGriT (overwritten every time LaGriT command is called) |
| _ **legx3dgen** | LaGriT log file | Log file containing commands executed in LaGriT (overwritten every time LaGriT command is called) |
| _ ***.csv** | Text file (comma separated values) | In this tutorial, these are text files containing coordinates in meters for surfaces. Columns are x, y, z coordinates respectively. |

## Getting started

The first step in starting this tutorial is making sure that LaGriT is executable within the working directory of this tutorial and ensuring all needed materials have been synced from github. The directory structure should be:

```

GeologicGriddingData/

├── GeneralLaGriT
│ ├── 1_create_surface.lgi
│ ├── 2_refine_domain.lgi
│ ├── 3_set_region_truncate.lgi
│ ├── refine_object.mlgi
│ └── surface
└── NorthPondLaGriT
    ├── 10_price_region_set.lgi
    ├── 11_price_remove_top.lgi
    ├── 12_price_top_region_set.lgi
    ├── 13_price_surf_aq300_bubbles.lgi
    ├── 14_price_aq300_set.lgi
    ├── 1_price_surf_domain.lgi
    ├── 2_middle_grid.csv
    ├── 2_price_surf_mid_grid.lgi
    ├── 3_middle_sb.csv
    ├── 3_price_surf_mid_sb.lgi
    ├── 4_price_surf_flat_zero.lgi
    ├── 5_price_surf_aq100.lgi
    ├── 6_price_surf_aq300_1k.lgi
    ├── 7_price_surf_filt.lgi
    ├── 8_price_driver_high_resolution_clipped.lgi
    ├── 9_price_surf_north_pond_seafloor.lgi
    └── function6

```
[Constructing refining surfaces](http://adamnicholasprice.github.io/GeologicGriddingTutorial/02_surfaces.html){: .btn .btn-purple }