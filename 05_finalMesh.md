---
layout: post
title:  "Final mesh"
nav_order: 6
---
# Final mesh
<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---
## Final Generalized Mesh
The general case mesh reproduces common topographic features with targeted refinement along areas of possible simulation intertest. This is a general case and therefore represents a proof of concept.

<script>
  
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/GeneralScene/";
    var file = "generalTetFinal.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>

#### Figure 1
Final tetrahedral general case mesh

<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/GeneralScene/";
    var file = "generalTetSlice.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>

#### Figure 2
Final tetrahedral general case mesh with cutaway showing topograpic low and material zones.

## North Pond
The final North Pond mesh reproduces important characteristics that we sought to reproduce in the mesh.

First, a layered system with separate regions representing seafloor sediments, upper-volcanic crustal aquifer of varying thicknesses, and a lower volcanic unit of conductive crust. Using the aquifer thickness as a “free parameter” in numerical simulations, allows assignment of a thicker or thinner unit in order to better represent heat and fluid flux processes. [Price et al., 2022](https://agupubs.onlinelibrary.wiley.com/doi/epdf/10.1029/2021JB023158).

<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/NorthPondScene/";
    var file = "13_tetAquiferNearFeild.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>

#### Figure 1
North Pond mesh with layers represnting sediment, conductive basement, and aquifer layers with varying thickness.

Second, geologic geometry that accurately represents the regional and local bathymetry relief. Which we see in the figure below, matches the wavelength and amplitude described in the accompanying publication and [Price et al., 2022](https://agupubs.onlinelibrary.wiley.com/doi/epdf/10.1029/2021JB023158).

<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/NorthPondScene/";
    var file = "12_tetTopReset.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>

#### Figure 2
Final North Pond tetrahedral mesh. 4x vertical exaggeration.

Finally, realistic geometry of the sedimented pond with varying sediment thicknesses, asymmetric slopes along the sediment / basement contacts, and sediment “pinchouts” or thinning sediments along the margins of the sedimented pond.

<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/NorthPondScene/";
    var file = "13_tetAssymSlice.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>

#### Figure 3
Final North Pond tetrahedral mesh with sediment pond offset from aquifer layer to show assymetric slopes and sediment thickness. 4x vertical exaggeration.

 [Define regions and assign materials](http://adamnicholasprice.github.io/GeologicGriddingTutorial/04_defineRegions.html){: .btn .btn-purple } [Pseudo code: General case](http://adamnicholasprice.github.io/GeologicGriddingTutorial/06_dockerfile.html){: .btn .btn-purple }
