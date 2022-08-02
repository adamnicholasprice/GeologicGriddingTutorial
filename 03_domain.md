---
layout: post
title:  "Constructing and refining domain"
nav_order: 4
---
# Constructing and refining domain
<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---
The next step in constructing a mesh is to determine the size, shape, and type of elements that will make up the mesh. In both of the general case, we start with a retangular mesh composed of hexahedral elements with a course geometry. Next, we use the surfaces constructed in the last section to refine the mesh along boundaries of interest. This allows the mesh to have a nested structure that has a high-resolution area in the near-feild representing the geologic structure we are interested in simulating and lower-resolution in the far-feild. This nested feature also increases computational effeciency of numerical models used to simulate processes using the mesh.

## Generalized Mesh

| **Filename** | **Input** | **Output** |
| --- | --- | --- |
| *2_refine_domain.lgi* | *1_surface_aq200.inp* | *2_initial_domain.inp* |
|                       | *1_surface_aq800.inp* | *2_hexRefine_octree.inp*|
|                       | *1_surface.inp*       | *2_surface_octree.inp*|
|                       |                       | *2_surfaceAQ200_octree.inp*|
|                       |                       | *2_surfaceAQ800_octree.inp* |

### Overview
For the general case, a rectangular mesh with dimensions 10km x 10km x 5km comprised of hexahedral elements resoultion of 1km x 1km x 1km is defined as the inital mesh. Next, the mesh is refined along the surfaces of interest using the following conditionals:
1. One refiment step along the surface, *1_surface.inp* (inital surface, top of domain)
2. Two refinement steps are performed along the surface *1_surface_aq200.inp* (transition of the 200 m thick aquifer layer to the 800 m thick aquifer layer)
3. One refinement step along the surface *1_surface_aq800.inp* (transition from the 800 m thick aquifer layer and the conductive boundary layer at the base of the domain)


### Syntax
```
lagrit < 2_refine_domain.lgi
```
<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/GeneralScene/";
    var file = "generalInitalDomain.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>
#### Figure 1
Inital general case hexahedral mesh
<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/GeneralScene/";
    var file = "generalRefineSlice.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>
#### Figure 2
General domain slice at x=0, showing levels of refinement in areas of interest.

# North Pond

| **Filename** | **Input** | **Output** |
| --- | --- | --- |
|*8_price_driver_high_resolution_clipped.lgi*	|*1_surf_domain.inp*	|*8_inital_hex_domain.inp* |
||*3_surf_mid_sb.inp*|*8_1_octree_NP.inp*|
||*4_surf_flat_zero.inp*|*8_2_octree_aq100.inp*|
||*5_surf_aq100.inp*|	*8_3_octree_aq300.inp*|
||*6_surf_aq300_1k.inp*|	*8_4_octree_dom.inp*|
||*7_surf_filt_aq100.inp*|	*8_4_octree_domaq100.inp*|
||*7_surf_filt_aq300.inp*|	*8_5_octree_domaq1k.inp*|
||*7_surf_filt_aq600.inp*|	*8_6_octree_domaq300.inp*|
||*7_surf_filt_aq1k.inp*|	*8_7_octree_domaq600.inp*|
|||	*8_hex_refine_octree.inp*|

### Overview
For North Pond, a rectangular mesh with 40km x 90km x 7.5km comprised of hexahedral elements with an inital resolution of 1km x 1km x 750m is defined. Refiment using the same subroutine as in the general case is used here with a much more sophisticated set of refinement conditionals. The following steps are taken in the refinement step:

1. One refinement of elements intersecting surface *3_surf_mid_sb.inp* (sediment-basement interface just in North Pond, the area of interest)
2. One refinement of elements intersecting volume *5_surf_aq100.inp* (100 m thick aquifer from seafloor to 100m below surface just below North Pond)
3. Two refinements of elements intersecting volume *6_surf_aq300_1k.inp* (900m thick aquifer layer from 100m below surface to 1000m below surface just below North Pond)
4. Two refinements of elements intersecting surface *1_surf_domain.inp* (bathymetric relief and sediment basement contact the across the entire domain)
5. One refinement of elements with less than two refinements intersecting the volume *7_surf_filt_aq100.inp* (transition from 100 m thick aquifer and 200 m thick aquifer just in North Pond)
6. One refinement of elements with less than two refinements intersecting the surface *7_surf_filt_aq1k.inp* (transition from 1000 m thick aquifer and the conductive basement layer just in North Pond)
7. One refinement of elements with less than two refinements intersecting the surface *7_surf_filt_aq300.inp* (transition from 200 m thick aquifer and 300 m thick aquifer layer just in North Pond)
8. One refinement of elements with less than two refinements intersecting the surface *7_surf_filt_aq600.inp* (transition from 300 m thick aquifer and 400 m thick aquifer layer just in North Pond)
9. One refinement of elements with less than four refinements intersecting the surface *2_surf_mid_grid.inp* (bathymetric relief and sediment basement contact just in and adjacent to North Pond)


### Syntax
```
lagrit < 8_driver_high_resolution_clipped.lgi
```
<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/NorthPondScene/";
    var file = "8_initalHexDomain.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>
#### Figure 4
Inital North Pond hexahedral mesh
<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/NorthPondScene/";
    var file = "8_refineHexDomain.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>
#### Figure 5
Refined North Pond mesh subset at x = -6000m, colored by cell refinement level.

## Conclusion

Now that we have a mesh that has the correct resolutions in the near- and far-feild, we will move on to defining material zones.
[Constructing refining surfaces](http://adamnicholasprice.github.io/GeologicGriddingTutorial/02_surfaces.html){: .btn .btn-purple } [Define regions and assign materials](http://adamnicholasprice.github.io/GeologicGriddingTutorial/04_defineRegions.html){: .btn .btn-purple }
