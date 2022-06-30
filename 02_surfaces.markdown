---
layout: post
title:  "Constructing refining surfaces"
nav_order: 3
has_children: true
---
# Surfaces

The first step in the construction of a simulation domain is to construct surfaces that represent geologically realistic layers. These surfaces will define boundaries that allow assignment of specific material properties and act as surfaces to refine the simulation domain. Surfaces can be constructed in LaGriT or in an outside programming language such as R, Python, or Matlab and brought into LaGritT. In both cases in this tutorial, surfaces were constructed outside of LaGriT and brought in to define mesh objects. The general steps to construct surfaces are:

1. Save mesh coordinates (x,y,z) into a .csv file


    2. Create mesh object that has coordinate system (x and y) of desired mesh resolution (have to define if different from .csv file coordinates)


    3. Connect mesh objects with new coordinates in x and y.

	4. Read in the .csv as a mesh object into LaGriT


    5. Copy over z-coordinates from mesh in step 4 to connected mesh in step 2

This process forces LaGriT to make the “best” connections within an x-y plane to begin with and then the coordinates along the z-axis with relief to be defined, thereby preventing any connections outside the plane.


## Generalized Mesh

For the general case, there is one control file (.lgi), one input file (_surface.lgi*), and four output surfaces.

| **Filename** | **Input** | **Output** |
| --- | --- | --- |
| *1_create_surface.lgi* | *surface.csv* | *1_surface_aq200.inp* |
| *1_surface_aq800.inp* |
| *1_surface_zero.inp*|
| *1_surface.inp* |

### Overview
For the general case, *surface.csv* is a continuous function;
$$ z(x,y) = -cos(x) - cos(y) $$
that replicates, but generalizes topography and structures observed in seafloor bathymetric data. The maxima of the function would be representative of seamounts, whereas the minima would represent sediment ponds, both common features on the seafloor. These relationships may be difficult to observe but they will become clear throughout the tutorial.

### Steps

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

#### Figure 1: Interactive figure is made using the file *1_surface.inp* in a program called [Kitware Glance](https://kitware.github.io/glance/app/) which is very useful for sharing and visualizing Paraview data. The surface is outlined by the eventual domain and shows the extent and relief of our initial surface.

<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/GeneralScene/";
    var file = "1_surface_zero.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>

#### Figure 2: All the surfaces output from _1_surface.inp_. Surfaces represent the upper most topography, layers at 200 and 800m below surface, and a surface at 0m. These surfaces will surface as boundaries for refinement as well as assigning materials to the simulation domain.


## North Pond

For the North Pond case, the first seven .lgi files define surfaces. The initial surface that represents the regional bathymetry of North Pond, was constructed outside of LaGriT in R. The details of sampling data for surface construction can be found in the supplemental information [Price et al., 2022](https://agupubs.onlinelibrary.wiley.com/action/downloadSupplement?doi=10.1029%2F2021JB023158&file=2021JB023158-sup-0001-Supporting+Information+SI-S01.docx). From samples of bathymetric data surrounding North Pond, a continuous function that best describes the region is:
    $$z(x,y)=  \frac{1}{t}\arctan\left (\frac{t*\sin(x)}{1-t*\cos(x)}\right) - \frac{1}{t}\arctan\left (\frac{t*\sin(y)}{1-t*\cos(y)}\right)$$
where t determines the “tilt” of the function, x is the strike-normal distance, and y is the strike-parallel distance.



| **Filename** | **Input** | **Output** |
| --- | --- | --- |
| 1_price_surf_domain.lgi | _function6.csv_ | _1_surf_domain.inp_ |

### Overview
Create mesh object surface representing North Pond bathymetry using function6.csv

| **Filename** | **Input** | **Output** |
| --- | --- | --- |
| _2_price_surf_mid_grid.lgi_ | _2_middle_grid.csv_ | _2_surf_mid_grid.inp_ |

### Overview
Create mesh object surface from trimmed area of interest created from truncated bathymetric surface (_2_middle_grid.csv_) representing North Pond and immediately adjacent region.

| **Filename** | **Input** | **Output** |
| --- | --- | --- |
| _3_price_surf_mid_sb.lgi_ | _3_middle_sb.csv_ | _3_surf_mid_sb.inp_ |

### Overview
Create mesh object surface from trimmed area of interest created from truncated bathymetric surface (_3_middle_sb.csv_) representing sediment / basement interface beneath North Pond.

| **Filename** | **Input** | **Output** |
| --- | --- | --- |
| _4_price_surf_flat_zero.lgi_ | _None_ | _4_surf_flat_zero.inp_ |

### Overview
Construct a flat surface at z = 0 at the same spatial extent as _2_middle_grid.csv_.

| **Filename** | **Input** | **Output** |
| --- | --- | --- |
| _5_price_surf_aq100.lgi_ | _3_surf_mid_sb.inp_ | _5_surf_aq100.inp_ |

### Overview
Construct a volume that represents a 100m thick aquifer section directly beneath North Pond.


| **Filename** | **Input** | **Output** |
| --- | --- | --- |
| _6_price_surf_aq300_1k.lgi_ | _2_surf_mid_grid.inp_ | _6_surf_aq300_1k.inp_ |

### Overview
Construct a volume that represents a 900 m thick aquifer section directly beneath the region representing North Pond.


| **Filename** | **Input** | **Output** |
| --- | --- | --- |
| 7_price_surf_filt.lgi | _1_surf_domain.inp_ | _7_surf_filt_aq100.inp_ |
| _7_surf_filt_aq300.inp_ |
| _7_surf_filt_aq600.inp_ |
| _7_surf_filt_aq1k.inp_ |





[Introduction](http://adamnicholasprice.github.io/GeologicGriddingTutorial/01_intro.html){: .btn .btn-purple } [Constructing and refining domain](http://adamnicholasprice.github.io/GeologicGriddingTutorial/03_domain.html){: .btn .btn-purple }
