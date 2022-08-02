---
layout: post
title:  "Define regions and assign materials"
nav_order: 5
---
# Define regions and assign materials
<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---
Next, using the surfaces and volumes constructed in the previous steps, we define material zones that correspond to stratigraphic units of interest in our mesh. Defining zones in this way, allows the user to assign physical properties, boundary conditions, and query simulation outputs much easier that doing so element by element.

## Generalized Mesh

| **Filename** | **Input** | **Output** |
| --- | --- | --- |
| *3_set_region_truncate.lgi* | *1_surface_aq200.inp* | *3_hexDomain_defineZones.inp* |
|                       | *1_surface_aq800.inp* | *3_tetDomain_topConnected.inp*|
|                       | *1_surface.inp*       | *3_tetDomain_regionSet.inp*|
|                       | *1_surface_zero.inp*  | |
|                       | *2_hexRefine_octree.inp*| |

### Overview
In this step we set material zones, remove elements outside the area of interest, convert from hexahedral mesh to tetrahedral mesh, and ensure all zones are assigned correctly.

### Syntax
```
lagrit < 8_driver_high_resolution_clipped.lgi
```

### Set zones
For the general case, five zones are defined corresponding to the following surfaces based off the following conditionals:
- Zone 1: Represents a sediment zone, defined as less than or equal to the surface at z = 0 and greater than the initial cosine surface that defines the upper-most boundary of the domain.
- Zone 2: Represents a 200 m thick aquifer section, defined as defined as less than or equal to the initial cosine surface that defines the upper-most boundary of the domain and greater than the surface representing the transition of the 200 m thick aquifer layer to the 600 m thick aquifer layer.
- Zone 3: Represents a 600 m thick aquifer section, defined as less than or equal to the 200 m thick aquifer layer to the 600 m thick aquifer layer, and greater than the surface that represents the transition from the 600 m thick aquifer layer and the conductive boundary layer at the base of the domain.
- Zone 4: Represents the conductive layer at the base of the domain, defined as less than the surface that represents the transition from the 600 m thick aquifer layer and the conductive boundary layer at the base of the domain.
- Zone 5: Represents elements and points outside the mesh, defined as not belonging to zones 1-4

However, these zones must be set in the correct order because any subsquent assignment of zones overwrites previous assignments. Therefore the material zones are assigned in the following order:
1. Zone 5
2. Zone 1
3. Zone 2
4. Zone 3
5. Zone 4

<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/GeneralScene/";
    var file = "generalHexZones.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>

#### Figure 1
General case hexahedral mesh colored by material zones assigned above

### Convert to tetrahedral mesh and remove excess simulation domain
It is important to note that many pre- and post-processing tools have been developed to assign properities, query data, and assign inital conditions. As part of North Pond simulations [(Price et al., 2022)](https://agupubs.onlinelibrary.wiley.com/doi/epdf/10.1029/2021JB023158) we converted elements in the mesh from hexahedral to tetrahedral to allow use of pre- and post-processors developed for use with earlier simulations [which can be found at this GitHub repository](https://github.com/UCSC-Hydrogeology/fehmToolkit).

the Since we started with a rectangular mesh but have refined our mesh to represent a topographic surface, we will remove any elements we do not want to simulation. In the above step, we set Zone 5 to represent this area of the mesh outside the area of interest.

<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/GeneralScene/";
    var file = "generalHexRemoveTop.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>

#### Figure 2
General case hexahedral mesh with removal of elements outside the area of interest (top zone)

### Reset top zone of domain
The final step is to correctly assign zones that represent the top, bottom, and side of the mesh. Since we removed the what was the top of the mesh in the previous step to reveal the refined topographic surface below, all the elements that were assigned as the top boundary no longer exist. We redefine the top zone of the mesh as elements belonging to the top zone and any material zone greater than one.

<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/GeneralScene/";
    var file = "generalTetSlice.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>

#### Figure 3
General case tetrahedral mesh with top material zone correctly defined with slice taken out at x,y = 0,0

## North Pond

| **Filename** | **Input** | **Output** |
| --- | --- | --- |
*11_region_set.lgi*|*8_hex_refine_octree.inp*|*11_hex_refine_octree.inp*|
||*1_surf_domain.inp*||
||*4_surf_flat_zero.inp*||
||*7_surf_filt_aq100.inp*||
||*7_surf_filt_aq300.inp*||
||*7_surf_filt_aq600.inp*||
||*7_surf_filt_aq1k.inp*||
||*3_surf_mid_sb.inp*||
||*9_surf_north_pond_seafloor.inp*||
||*10_surf_filt_mid_grid_aq100.inp*||
||*10_surf_filt_mid_grid_aq300.inp*||
||*10_surf_filt_mid_grid_aq600.inp*||
||*10_surf_filt_mid_grid_aq1k.inp*||

### Overview
Using the same steps as above we set material zones, remove elements outside the area of interest, and ensure all zones are assigned correctly for the North Pond mesh.

### Syntax
```
lagrit < 11_region_set.lgi
```

### Set zones
For the general case, five zones are defined corresponding to the following surfaces based off the following conditionals:
- Zone 1: Represents the sedimented pond of North Pond, defined as less than the seafloor surface (z=0)
- Zone 2: 100 m thick aquifer layer adjacent but not including North Pond
- Zone 3: 200 m thick aquifer layer adjacent but not including North Pond
- Zone 4: 300 m thick aquifer layer adjacent but not including North Pond
- Zone 5: 400 m thick aquifer layer adjacent but not including North Pond
- Zone 6: Conductive basement layer adjacent but not including North Pond
- Zone 7: 100 m thick aquifer layer immediately adjacent to and including North Pond
- Zone 8: 200 m thick aquifer layer immediately adjacent to and including North Pond
- Zone 9: 300 m thick aquifer layer immediately adjacent to and including North Pond
- Zone 10: 400 m thick aquifer layer immediately adjacent to and including North Pond
- Zone 11: Represents elements and points outside the mesh, defined as not belonging to zones 1-10


However, these zones must be set in the correct order because any subsquent assignment of zones overwrites previous assignments. Therefore the material zones are assigned in the following order:
1. Zone 11
2. Zone 1
3. Zone 2
4. Zone 3
5. Zone 4
6. Zone 5
7. Zone 6
8. Zone 7
9. Zone 8
10. Zone 9
11. Zone 10

<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/NorthPondScene/";
    var file = "12_hexZones.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>

#### Figure 4
North Pond hexahedral mesh with material zones defined. 4x vertical exaggeration.

| **Filename** | **Input** | **Output** |
| --- | --- | --- |
*12_remove_top.lgi*|*11_hex_refine_octree.inp*|*12_tmp_hex_mesh.inp*|
|||*12_tmp_tet_interp.inp*|

### Convert to tetrahedral mesh and remove excess simulation domain
As stated above, the first step is to convert the elements in the mesh from hexahedral  to a tetrahedral for use with legacy tools.

Since we started with a rectangular mesh but have refined our mesh to represent a topographic surface, we will remove any elements we do not want to simulation. In the above step, we set Zone 11 to represent this area of the mesh outside the area of interest.

### Syntax
```
lagrit < 12_remove_top.lgi
```

<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/NorthPondScene/";
    var file = "12_hexRemoveTop.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>

#### Figure 5
North Pond hexahedral mesh with top zone being removed. 4x vertical exaggeration.

| **Filename** | **Input** | **Output** |
| --- | --- | --- |
*13_top_region_set.lgi*|*12_tmp_tet_interp.inp*|*13_tet_reset_final.inp*|
||*{fehm.files}*|


### Reset top zone of domain
The final step is to correctly assign zones that represent the top, bottom, and side of the mesh. Since we removed the what was the top of the mesh in the previous step to reveal the refined topographic surface below, all the elements that were assigned as the top boundary no longer exist. We redefine the top zone of the mesh as elements belonging to the top zone and any material zone greater than one. Additionally, we export files used in [Finite Element Heat and Mass Transfer Code (FEHM)](https://fehm.lanl.gov/).

### Syntax
```
lagrit < 13_top_region_set.lgi
```

<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/NorthPondScene/";
    var file = "12_tetTopReset.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>

#### Figure 6
North Pond tetrahedral mesh with top region set. 4x vertical exaggeration.

## Conclusion
This concludes the walkthrough portion of the tutorial. The next pages will cover the final geologic meshes, pseudo code for both cases and an [executable roadmap for each case](http://adamnicholasprice.github.io/GeologicGriddingTutorial/08_executeScripts.html).

[Constructing and refining domain](http://adamnicholasprice.github.io/GeologicGriddingTutorial/03_domain.html){: .btn .btn-purple } [Final Mesh](http://adamnicholasprice.github.io/GeologicGriddingTutorial/05_finalMesh.html){: .btn .btn-purple }
