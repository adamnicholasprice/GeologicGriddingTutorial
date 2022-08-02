---
layout: post
title:  "Pseudo code: General case"
nav_order: 7
---

<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/GeneralScene/";
    var file = "1_surface.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>

# Pseudo code: General Case
{: .no_toc}
## General files description
{: .no_toc}
| **Filename** | **File purpose** |
| --- | --- |
|1_create_surface.lgi| Constructing reference surfaces|
|2_refine_domain.lgi|Mesh building and refinement|
|3_set_region_truncate.lgi|Setting material regions based on surfaces, hex-to-tet mesh, removing top of mesh, setting top most zone, cleaning up mesh|

<details markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---
## 1_create_surface.lgi

| **Input** | **Output** |
| --- | --- |
2_middle_grid.csv|2_surf_mid_grid.inp|


## **Overview**
Create all surfaces needed in construction, refinement, and material assignment of general case mesh.

### **Lines 1 - 6**
Assign coordinate system in meters from xmin, ymin, zmin = -5000,-5000 to xmax, ymax  = 5000,5000 and number of points in x,y = 201,201. 

### **Lines 9 - 13**
Create mesh object *mo_surface* that has the above x and y dimensions with z = 0 and connect the points in the surface

### **Lines 16 - 21**
Create mesh object *surface_plane* from surface *surface.csv*. Then copy over z coordinates from *surface_plane* to connected surface *mo_surface*

### **Lines 24 -  30**
Set properties assocaited with surface (mostly used for visualization in this case) and dump mesh object surface *mo_surface* to *1_surface.inp*

### **Lines 34 - 54**
Create copies of *mo_surface* named *mo_aq800* and *mo_aq200*. Subtract 200m and 800m from the z coordinate of surfaces *mo_aq200* and *mo_aq800*; respectively. Set properties assocaited with surface, and dump mesh objects to files
- *mo_aq200* to *1_surface_aq200.inp*
- *mo_aq800* to *1_surface_aq800.inp*

### **Lines 58 - 74**
Create mesh object *mo_zero* at z = 0 with a two points in x and y with the same extent as specified above. Set properties assocaited with surface and dump *mo_zero* to file *1_surface_zero.inp*.

### **File link:**


## 2_refine_domain.lgi

| **Input** | **Output** |
| --- | --- |
2_middle_grid.csv|2_surf_mid_grid.inp|


## **Overview**
First, we create an inital hexahedral mesh. Next, we read in all the surfaces created in the previous files. Finally, the subroutine *refine_object.mlgi* is called to perform octree refinement along the elements intersected with the surfaces called and files are created for each of those refinement iterations. 

### **Lines 2 -  14**
Define xmin, ymin, zmin = -5000,-5000, -4000 to xmax, ymax, zmax  = 5000,5000,1000 and number of points in x,y,z = 11,11,6. This sets the initial resolution at 1000m elements in x,y,z. Create the mesh object *domain_hex* and dump to file *2_initial_domain.inp*.

### **Lines 16 -  18**
Read in surfaces
- *1_surface.inp* = *mo_surface*
- *1_surface_aq200.inp* = *mo_aq200*
- *1_surface_aq800.inp* = *mo_aq800*

### **Line 23**
Define the initail hexahedral mesh *domain_hex* as the object to refine.

### **Lines 24 - 27**
This series of lines performs the octree refinement and will be performed many times in this file. The flow of these lines are:

- Line 24: Define the mesh object to refine with. Here is *mo_sb*
- Line 25: Perform octree refinement via the subroutine *refine_object.mlgi*
- Line 26: Dump the refined object to a file. Here *2_surface_octree.inp*
- Line 27: Check the quality of the refinement.

Henceforth, this series of steps will be denoted by: Refine {*name of mesh object*} - File: {*Output file*}

### **Lines 29 - 34**
Refine x2 *mo_aq200 -* File: *2_surfaceAQ200_octree.inp*

### **Lines 36 - 40**
Refine *mo_aq800 -* File: *2_surfaceAQ800_octree.inp*

### **Lines 45 - 46**
Reset the *itetclr* variable. Dump the refined mesh *domain_hex* to file *2_hexRefine_octree.inp*


## Refine_object.mlgi

| **Input** | **Output** |
| --- | --- |
|{Element to be refined}|{Refining element}|

## **Overview**
Subroutine used to perform octree refinement of a mesh object along a specified surface or volume
### **Lines 1 - 2**
Clear any variables from previous use of subroutine
### **Line 4**
Create a mesh object attribute named *if_inter*  that contains the intersection of the mesh object being refined and the mesh object being used to refine in this step. These are defined outside the subroutine
### **Line 5**
Create an element set named *erefine* based on where *if_inter* is greater than 0 (i.e. where the meshes intersect)
### **Line 6**
The selected set, *erefine,* is refined.
### **File link:**

### **File link:**


## 3_set_region_truncate.lgi

| **Input** | **Output** |
| --- | --- |
|2_hexRefine_octree.inp |3_hexDomain_defineZones.inp|
|1_surface.inp |{outside zone files}|
|1_surface_aq200.inp|3_tetDomain_regionSet.inp|
|1_surface_aq800.inp ||
|1_surface_zero.inp||


## **Overview**

### **Line 1**
Read in refined mesh *2_hexRefine_octree.inp* as mesh object *domain_hex*

### **Lines 3 - 11**
Read in surfaces as sheet surfaces as follows:
- *1_surface.inp* as sheet surface *surf*
- *1_surface_aq200.inp* as sheet surface *s_aq200*
- *1_surface_aq800.inp* as sheet surface *s_aq800*
- *1_surface_zero.inp* as sheet surface *zero*

### **Lines 13 - 14**
Set attribute *itetclr* = 5 for mesh *domain_hex*


### **Lines 17 - 37**
This section details the intersecting of sheets above with the refined mesh object inorder to define individual zones that correspond to geologic units within the grid. The general workflow of this section takes example from Lines 33 - 35:

- Line 33 - Define the region of interest and name it *r1* which is less than or equal to the sheet zero and greater than the surface *surf*. (surf < r1 ≤ zero)
- Line 34 - Define an eltset (element set), *e_r1*, from the region *r1*.
- Line 35 - Set the itetclr attribut of *domain_hex*  = 1

This process repeats for different conditions until line 37. The layers that correspond to the itetclr set in these lines are:

- 1 - Sediment
- 2 - Aquifer layer 0 - 200m
- 3 - Aquifer layer 200 - 800m
- 4 - Conductive basement < 800 m 
- 5 - Everything else (i.e., the area above the refined topographic surface)

### **Lines 39 - 43**
Check the quality of *domain_hex* and dump to file *3_hexDomain_defineZones.inp*

### **Line 45 - 49**
Convert hexahedral mesh *domain_hex* to tetrehedral mesh *domain_tet* for ease of use for legacy tools from UCSC hydrogeology lab [GitHub](https://github.com/UCSC-Hydrogeology/fehmToolkit)

### **Line 52 - 60**
Reset imt and itp attributes for mesh *domain_tet*. Interpolate material regions from *domain_hex* to *domain_tet*. Dump mesh *domain_tet* to file *3_tetDomain_topConnected.inp*

### **Line 63 - 64**
Remove material zone 5 corresponding to elements above refined topography.

### **Line 67 - 70**
Reset attributes *cell_color* and *itp*. Check status of mesh

### **Line 73 - 78**
Dump outside zone files to read back in later steps. Select points with material zone (imt) greater than 1 and in the top zone for mesh *domain_tet* and set attribute imt to zone 2. 

### Line 81
Dump completed mesh *domain_tet* to file *3_tetDomain_regionSet.inp*

### **File link:**

 [Cleanup mesh, materials, and boundaries](http://adamnicholasprice.github.io/GeologicGriddingTutorial/05_finalMesh.html){: .btn .btn-purple }  [Pseudo code: North Pond](http://adamnicholasprice.github.io/GeologicGriddingTutorial/06_pseudoNorthPond.html){: .btn .btn-purple }
