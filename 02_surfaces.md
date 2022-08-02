---
layout: post
title:  "Constructing refining surfaces"
nav_order: 3
---
# Surfaces
<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---
The first step in the construction of a simulation domain is to construct surfaces that represent geologically realistic layers. These surfaces will define boundaries that allow assignment of specific material properties and act as surfaces to refine the simulation domain. Surfaces can be constructed in LaGriT or in an outside programming language such as R, Python, or Matlab and brought into LaGritT. In both cases in this tutorial, surfaces were constructed outside of LaGriT and brought in to define mesh objects. The general steps to construct surfaces are:

1. Save mesh coordinates (x,y,z) into a .csv file

2. Create mesh object that has coordinate system (x and y) of desired mesh resolution (have to define if different from .csv file coordinates)

3. Connect mesh objects with new coordinates in x and y.

4. Read in the .csv as a mesh object into LaGriT

5. Copy over z-coordinates from mesh in step 4 to connected mesh in step 2

This process forces LaGriT to make the “best” connections within an x-y plane to begin with and then the coordinates along the z-axis with relief to be defined, thereby preventing any connections outside the plane.

## Generalized Mesh

For the general case, there is one control file (.lgi), one input file (*surface.lgi*), and four output surfaces.

| **Filename** | **Input** | **Output** |
| --- | --- | --- |
| *1_create_surface.lgi* | *surface.csv* | *1_surface_aq200.inp* |
|                       |                       | *1_surface_aq800.inp* |
|                       |                       | *1_surface_zero.inp*|
|                       |                       | *1_surface.inp* |

### Overview

For the general case, *surface.csv* is the continuous function;

$$z(x,y) = -cos(x) - cos(y)$$

that replicates, but generalizes topography associated with topographic relief and a topographic low. The maxima of the function would be representative of seamounts or hills, whereas the minima would represent sediment ponds or a sedimented valley bottom, both common features on the earths surface.

### Syntax

First, navigate to the directory for the general case (i.e. GeologicGriddingData/GeneralLaGriT)

Next, execute the .lgi file
```
lagrit < 1_create_surface.lgi
```

### Figures
<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/GeneralScene/";
    var file = "1_surface.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>
#### Figure 1
Interactive figure is made using the file *1_surface.inp* in a program called [Kitware Glance](https://kitware.github.io/glance/app/) which is very useful for sharing and visualizing Paraview data. The surface is outlined by the eventual domain and shows the extent and relief of our initial surface.
<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/GeneralScene/";
    var file = "1_surface_zero.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>
#### Figure 2
All the surfaces output from _1_surface.inp_. Surfaces represent the upper most topography, layers at 200 and 800m below surface, and a surface at 0m. These surfaces will surface as boundaries for refinement as well as assigning materials to the simulation domain.

## North Pond

For the North Pond case, the first seven .lgi files define surfaces. The initial surface that represents the regional bathymetry of North Pond, was constructed outside of LaGriT in R. The details of sampling data for surface construction can be found in section 3 of this publication or in the supplimental information here [Price et al., 2022](https://agupubs.onlinelibrary.wiley.com/action/downloadSupplement?doi=10.1029%2F2021JB023158&file=2021JB023158-sup-0001-Supporting+Information+SI-S01.docx). From samples of bathymetric data surrounding North Pond, a continuous function that best describes the region is:
    $$
    z(x,y)=  \frac{1}{t}\arctan\left (\frac{t*\sin(x)}{1-t*\cos(x)}\right) - \frac{1}{t}\arctan\left (\frac{t*\sin(y)}{1-t*\cos(y)}\right)
    $$
where t determines the “tilt” of the function, x is the strike-normal distance, and y is the strike-parallel distance. The inital surface is defined from -10 to 10 in the x and y directions. 

| **Filename** | **Input** | **Output** |
| --- | --- | --- |
| *1_surf_domain.lgi* | *function6.csv* | *1_surf_domain.inp* |

### Overview
Create mesh object surface representing North Pond bathymetry using function6.csv. The surface created above is brought into LaGriT and assigned a new coordinate system that assigns the correct slopes to the areas of interest. This new coordinate system is from -75 km to 75 km along-strike, -30 km to 30 km in the across-strike direction, with an amplitude of 600 m. 


### Syntax
Navigate to the directory for the North Pond mesh case and 

```
lagrit < 1_surf_domain.lgi
```
<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/NorthPondScene/";
    var file = "1_surface_domain.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>
#### Figure 3
Inital North Pond surface with 4x vertical exaggeration

| **Filename** | **Input** | **Output** |
| --- | --- | --- |
| *2_surf_mid_grid.lgi* | *2_middle_grid.csv* | *2_surf_mid_grid.inp* |

### Overview
Create mesh object surface from trimmed area of interest created from truncated bathymetric surface (_2_middle_grid.csv_) representing North Pond and immediately adjacent region.

### Syntax

```
lagrit < 2_surf_mid_grid.lgi
```
<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/NorthPondScene/";
    var file = "2_surf_mid_grid.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>
#### Figure 4
Surface *2_surf_mid_grid.inp*, representing a subset of the inital surface above just in the area and immediately adjacent to North Pond with 4x vertical exaggeration.

| **Filename** | **Input** | **Output** |
| --- | --- | --- |
| *3_surf_mid_sb.lgi* | *3_middle_sb.csv*  | *3_surf_mid_sb.inp* |

### Overview
Create mesh object surface from trimmed area of interest created from truncated bathymetric surface (*3_middle_sb.csv* ) representing sediment / basement interface beneath North Pond.

### Syntax

```
lagrit < 3_surf_mid_sb.lgi
```
<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/NorthPondScene/";
    var file = "3_surf_mid_sb.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>
#### Figure 5
Surface *3_surf_mid_sb.inp*, representing the sediment-basement contact directly below the sedimented pond in North Pond with 4x vertical exaggeration

| **Filename** | **Input** | **Output** |
| --- | --- | --- |
| *4_surf_flat_zero.lgi* | *None* | *4_surf_flat_zero.inp* |

### Overview
Construct a flat surface at z = 0 at the same spatial extent as *2_middle_grid.csv* just in the area of and immediately adjacent to North Pond.

### Syntax

```
lagrit < 4_surf_flat_zero.lgi
```
<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/NorthPondScene/";
    var file = "4_surf_flat_zero.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>
#### Figure 6
Surface *4_surf_flat_zero.inp*, in the area of and immediately adjacent to North Pond.

| **Filename** | **Input** | **Output** |
| --- | --- | --- |
| *5_surf_aq100.lgi* | *3_surf_mid_sb.inp* | *5_surf_aq100.inp* |

### Overview
Construct a volume that represents a 100m thick aquifer section directly beneath the sediment-basement contact of North Pond with the same extent as *3_surf_mid_sb.inp* 

### Syntax

```
lagrit < 5_surf_aq100.lgi
```
<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/NorthPondScene/";
    var file = "5_surf_aq100.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>
#### Figure 7
Volume  *5_surf_aq100.inp*, representing a 100m thick aquifer section below North Pond 10x vertical exaggeration.


| **Filename** | **Input** | **Output** |
| --- | --- | --- |
| *6_surf_aq300_1k.lgi* | *2_surf_mid_grid.inp* | *6_surf_aq300_1k.inp* |

### Overview
Construct a volume that represents a 900 m thick aquifer section from the base of the 100m thick aquifer to the base of the 1000m thick aquifer directly beneath and adjacent to the region representing North Pond.

### Syntax

```
lagrit < 6_surf_aq300_1k.lgi
```
<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/NorthPondScene/";
    var file = "6_surf_aq300_1k.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>
#### Figure 8
Volume 6_surf_aq300_1k.inp, representing a 900m thick aquifer section 4x vertical exaggeration.

| **Filename** | **Input** | **Output** |
| --- | --- | --- |
| *7_surf_filt.lgi* | *1_surf_domain.inp* | *7_surf_filt_aq100.inp* |
|                       |                       |*7_surf_filt_aq300.inp* |
|                       |                       |*7_surf_filt_aq600.inp* |
|                       |                       | *7_surf_filt_aq1k.inp* |


### Overview
Construct sufaces that represent the transitions from:
- aquifer 100 m thickness to 200 m thickness (7_surf_filt_aq100.inp)
- aquifer 200 m thickness to 300 m thickness (7_surf_filt_aq300.inp)
- aquifer 300 m thickness to 400 m thickness (7_surf_filt_aq600.inp)
- aquifer 400 m thickness to 500 m thickness (7_surf_filt_aq1k.inp)
<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/NorthPondScene/";
    var file = "7_surf_filt.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>
#### Figure 9
Surfaces *7_surf_filt_aq100.inp* (brown), *7_surf_filt_aq300.inp* (tan),*7_surf_filt_aq600.inp* (light purple), and *7_surf_filt_aq1k.inp* (dark purple) with 4x vertical exaggeration.

## Conclusion

Making surfaces to define material zones corresponding to stratigraphic sections and provide refinement boundaries is the most important and time consuming step in this process. The final figure shows the 10 surfaces and volumes constructed to define and refined the North Pond mesh.
<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/NorthPondScene/";
    var file = "all_surfs.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>
#### Figure 10
All surfaces and volumes constructed for North Pond mesh.
[Introduction](http://adamnicholasprice.github.io/GeologicGriddingTutorial/01_intro.html){: .btn .btn-purple } [Constructing and refining domain](http://adamnicholasprice.github.io/GeologicGriddingTutorial/03_domain.html){: .btn .btn-purple }
