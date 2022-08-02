---
layout: post
title:  "Pseudo code: North Pond"
nav_order: 8
---
# Pseudo code: North Pond
{: .no_toc}
## General files description
{: .no_toc}
| **Filename** | **File purpose** |
| --- | --- |
|1_surf_domain.lgi| Construct reference surfaces|
|2_surf_mid_grid.lgi|Construct reference surfaces|
|3_surf_mid_sb.lgi|Construct reference surfaces|
|4_surf_flat_zero.lgi|Construct reference surfaces|
|5_surf_aq100.lgi|Construct reference surfaces|
|6_surf_aq300_1k.lgi|Construct reference surfaces|
|7_surf_filt.lgi|Construct reference surfaces|
|8_driver_high_resolution_clipped.lgi| Mesh building and refinement|
|9_surf_north_pond_seafloor.lgi| Construct reference surfaces|
|10_surf_aq_volumes.lgi| Constructing reference volumes|
|11_region_set.lgi| Setting material regions based on surfaces/volumes |
|12_remove_top.lgi| Hex-to-tet mesh and removing top of mesh|
|13_top_region_set.lgi | Setting top most zone, cleaning up mesh, exporting FEHM files|

# **Files:**
## **Function6**
Bathymetric function.
## **2_mid_grid.csv**
Clipped bathymetry file representing North Pond and surrounding area. Used for high resolution filtering
## **3_middle_sb.csv**
Clipped bathymetry file representing just the sediment = basement interface.
<details markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---
## 1_surf_domain.lgi 

| **Input** | **Output** |
 --- | --- |
|function6|1_surf_domain.inp|

### **Overview**
Create mesh object for bathymetric surface.
### **Lines 1 - 11**
Assign coordinate system in meters from xmin, ymin, zmin = -30000,-75000, -5000 to xmax, ymax, zmax  = 30000,75000,0 and number of points in x,y,z = 301, 301,6 to determine spacing.
### **Lines 14 - 18**
Create a mesh object surface, *mo_tri*, that is the above x,y dimensions with singular z value (2D plane). Setting z coordinates to zero before connecting allows defining an explicit x,y coordinate system and connecting that surface without producing oddly shaped elements.  
### **Lines 23-24**
Creates mesh object (*sedbase*) from the topography function from R (*function6*)
### **Line 28**
Assign z coordinates from *sedbase* to connected triplane mesh object *mo_tri*
### **Lines 32 - 37**
Reset property values of mesh object to insure all the same value
### **Lines 39 - 41**
Dump surface to file 1_surf_domain.inp
### **File link:**

## 2_surf_mid_grid.lgi

| **Input** | **Output** |
 --- | --- |
2_middle_grid.csv|2_surf_mid_grid.inp|


## **Overview**
Create mesh object from trimmed area of interest created from truncated bathymetric surface representing North Pond and immediately adjacent region created outside of LaGriT
### **Lines 4 - 8**
Read in *2_middle_grid.csv,* create a mesh object (*sedbase*), set z coordinates to zero, then connect. Setting z coordinates to zero before connecting allows defining an explicit x,y coordinate system and connecting that surface without producing oddly shaped elements.  
### **Lines 10 - 14**
Read in *2_middle_grid.csv*, create a separate mesh object *temp*, then copy z coordinates from temp to connected surface *sedbase*
### **Lines 17 - 22**
Reset property values of mesh object to insure all the same value
### **Lines 26 - 28**
Dump surface to file *2_surf_mid_grid.inp*
### **File link:**

## 3_surf_mid_sb.lgi
| **Input** | **Output** |
 --- | --- |
3_middle_sb.csv|3_surf_mid_sb.inp|

## **Overview**
Create mesh object from trimmed area of interest created from truncated bathymetric surface representing sediment = basement interface beneath North Pond created outside of LaGriT
### **Lines 4 - 8**
Read in *3_middle_sb.csv,* create a mesh object (*sedbase*), set z coordinates to zero, then connect. Setting z coordinates to zero before connecting allows defining an explicit x,y coordinate system and connecting that surface without producing oddly shaped elements.  
### **Lines 10 - 13**
Read in *3_middle_sb.csv,* create a separate mesh object (*temp*), then copy z coordinates from temp to connected surface sedbase
### **Lines 17 - 22**
Reset property values of mesh object to insure all the same value
### **Lines 26 - 28**
Dump surface to file *3_surf_mid_sb.inp*
### **File link:**

## 4_surf_flat_zero.lgi

| **Input** | **Output** |
 --- | --- |
|None|4_surf_flat_zero.inp|

## **Overview**
Construct a flat surface at z = 0 at the same spatial extent as *2_middle_grid.csv*.
### **Lines 2 - 5**
Specify extent of surface xmin, ymin = -15000, -6000 to xmax, ymax = 4600, 28500
### **Lines 7 - 9**
Specify number of x,y points (resolution) at 2 each
### **Lines 11 - 15**
Create mesh object *mo_tri*, create points on that mesh object, and connect
### **Lines 17 - 18**
Check quality of connections for oddly shaped elements
### **Lines 20 - 22**
Dump mesh object *mo_tri* to file *4_surf_flat_zero.inp*
### **File link:**

## 5_surf_aq100.lgi
| **Input** | **Output** |
 --- | --- |
|3_surf_mid_sb.inp|5_surf_aq100.inp|

## **Overview**
Construct a volume that represents a 100m thick aquifer section directly beneath North Pond.
### **Line 1**
Read in *3_surf_mid_sb.inp* to mesh object *mid_sb*
### **Line 4**
Extrude a volume that goes from sediment = basement interface to 100m below that interface.
### **Line 6**
Check the quality of the extrude
### **Lines 9 - 11**
Convert the volume from hexagonal elements to tetrahedral elements and check the quality of that conversion
### **Lines 13 - 15**
Dump *mo_tet* to file *5_surf_aq100.inp*
### **File link:**

## 6_surf_aq300_1k.lgi
| **Input** | **Output** |
 --- | --- |
|2_surf_mid_grid.inp|6_surf_aq300_1k.inp|

## **Overview**
Construct a volume that represents a 900 m thick aquifer section directly beneath the region representing North Pond.
### **Lines 2 - 4**
Read in *2_surf_mid_grid.inp* to mesh object *mo* and subtract 100m from all z coordinates.
### **Lines 6 - 8**
Extrude *mo* surface to a volume *mo_long*  that goes from 100 - 1000m below *2_surf_mid_grid.inp* surface representing local North Pond bathymetry.
### **Lines 10 - 13**
Convert the volume from hexagonal elements, *mo_long,* to tetrahedral elements, *mo_tet,* and check the quality of that conversion.
### **Lines 15 - 17**
Dump *mo_tet* to file *6_surf_aq300_1k.inp*
### **File link:**

## 7_surf_filt.lgi
| **Input** | **Output** |
 --- | --- |
|1_surf_domain.inp|7_surf_filt_aq100.inp|
||7_surf_filt_aq300.inp|
||7_surf_filt_aq600.inp|
||7_surf_filt_aq1k.inp|

## **Overview**
Construct four surfaces at -100, -300,-600, and -1000m at the extent of the entire domain.
### **Line 1**
Read in *7_surf_filt.lgi* to mesh object *mo*
### **Lines 4 - 7**
Create copies of *mo* to :

- *mo_aq100*
- *mo_aq300*
- *mo_aq600*
- *mo_aq1k*
### **Lines 12 - 15**
Subtract 100, 300, 600, and 1000m from the z coordinates of the copied mesh objects respectively and check the quality of those objects
### **Lines 20 - 25**
Dump mesh objects to files:

- *mo_aq100* to *7_surf_filt_aq100.inp*
- *mo_aq300* to *7_surf_filt_aq300.inp*
- *mo_aq600* to *7_surf_filt_aq600.inp*
- *mo_aq1k* to *7_surf_filt_aq1k.inp*
### **File link:**

## 8_driver_high_resolution_clipped.lgi
| **Input** | **Output** |
 --- | --- |
|1_surf_domain.inp|8_1_octree_NP.inp|
|3_surf_mid_sb.inp|8_2_octree_aq100.inp|
|4_surf_flat_zero.inp|8_3_octree_aq300.inp|
|5_surf_aq100.inp|8_4_octree_dom.inp|
|6_surf_aq300_1k.inp|8_4_octree_domaq100.inp|
|7_surf_filt_aq100.inp|8_5_octree_domaq1k.inp|
|7_surf_filt_aq300.inp|8_6_octree_domaq300.inp|
|7_surf_filt_aq600.inp|8_7_octree_domaq600.inp|
|7_surf_filt_aq1k.inp|8_hex_refine_octree.inp|

## **Overview**
This file is the key file in building the domain. First, we read in all the surfaces created in the previous files. Next, the subroutine *refine_object.mlgi* is called to perform octree refinement along the elements intersected with the surfaces called and files are created for each of those refinement iterations. The final file with refinements along all desired surfaces is dumped to *8_hex_refine_octree.inp*
### **Lines 1 - 14**
Define xmin, ymin, zmin = -24000,-40000, -6000 to xmax, ymax, zmax  = 16000,55000,1500 and number of points in x,y,z = 41, 96,11. This sets the initial resolution at 1000m elements in x,y and 750m in z. Create the mesh object *mo_hex* for refinement.
### **Lines 17 - 25**
Read in surfaces:

- *1_surf_domain.inp = domain*
- *4_surf_flat_zero.inp = sf_zero*
- *5_surf_aq100.inp = aq100*
- *6_surf_aq300_1k.inp =aq300_1k*
- *3_surf_mid_sb.inp = mo_sb*
- *7_surf_filt_aq100.inp = domain_aq100*
- *7_surf_filt_aq1k.inp = domain_aq1k*
- *7_surf_filt_aq600.inp = domain_aq600*
- *7_surf_filt_aq300.inp = domain_aq300*
### **Line 28**
Define the mesh object *mo_hex* as the object to perform octree refinement on
### **Lines 32 - 36**
This series of lines performs the octree refinement and will be performed many times in this file. The flow of these lines are:

- Line 32: Define the mesh object to refine with. Here is *mo_sb*
- Line 33: Perform octree refinement via the subroutine *refine_object.mlgi*
- Line 34: Dump the refined object to a file. Here *8_1_octree_NP.inp*
- Line 36: Check the quality of the refinement.

Henceforth, this series of steps will be denoted by: Refine {*name of mesh object*} - File: {*Output file*}
### **Lines 40 - 43**
Refine *aq100 -* File: *8_2_octree_aq100.inp*
### **Lines 47 - 52**
Refine x2 *aq300_1k -* File: *8_3_octree_aq300.inp*
### **Lines 57 - 60**
Refine x2 *domain* - File: *8_4_octree_dom.inp*
### **Lines 64 - 75**
Refine *domain_aq100* - File: *8_4_octree_domaq100.inp*

This refinement has a few additional steps outside the subroutine but before dumping the mesh object.

- Line 68: Create a mesh object attribute named *in_inter*  that contains the intersection of the mesh object being refined and mesh object being used to refine in this step
- Line 69: Create an element set named *erefine1* based on where *in_inter* is greater than 0 (i.e. where the meshes intersect)
- Line 70: Create an element set named *num_ref1*  based on elements that have been refined less than 2 times.
- Line 71: Create an element set named *combined1* that is the union of elements from *erefine1* and *num_ref1*
- Line 72: Refine the element set *combined1*

These lines are very similar to the subroutine *refine_object.mlgi* with the caveat that we are also imposing an additional condition that we only want to refine elements that have 1 level of refinement. This ensures that we are not over-refining elements creating a computationally intensive simulation grid. Henceforth, we will refer to this process as *itet-refine*
### **Lines 79 - 91**
Refine *domain_aq1k* - File: *8_5_octree_domaq1k.inp* plus *itet-refine*
### **Lines 96 - 108**
Refine *domain_aq300* - File: *8_6_octree_domaq300.inp* plus *itet-refine*
### **Lines 112 - 126**
Refine *domain_aq600* - File: *8_7_octree_domaq600.inp* plus *itet-refine*
### **Lines 128 - 142**
Read in surface *2_surf_mid_grid.inp* to mesh object *mid_grid*. Perform a itet-refine on elements with less than 4  refinements (itetlev < 4).
### **Line 144**
Reset all elements to color 7
### **Lines 146 - 151**
Dump *mo_hex* to *8_hex_refine_octree.inp*
### **File link:**


| **Input** | **Output** |
| --- | --- |
|refine_object.mlgi|{Element to be refined}|{Refining element}|

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




## 9_surf_north_pond_seafloor.lgi
| **Input** | **Output** |
|--- | --- |
|3_middle_sb.csv|9_surf_north_pond_seafloor.inp|

## **Overview**
Make a surface corresponding to x,y coordinates of North Pond sediment with z coordinates = 0
### **Lines 4 - 8**
Create triplane mesh object *sedbase*. Read in *3_middle_sb.csv* as the x,y,z coordinates of *sedbase*. Set z-coordinates = 0 and connect points.
### **Lines 11 - 15**
Reset all material and color properties associated with *sedbase*
### **Lines 19 - 21**
Dump mesh object *sedbase* to *9_surf_north_pond_seafloor.inp*
### **File link:**


## 10_surf_aq_volumes.lgi
|**Input** | **Output** |
|--- | --- |
|2_surf_mid_grid.inp|10_surf_filt_mid_grid_aq100.inp|
|||10_surf_filt_mid_grid_aq300.inp|
|||10_surf_filt_mid_grid_aq600.inp|
|||10_surf_filt_mid_grid_aq1k.inp|

## **Overview**
Construct volumes corresponding to the aquifer units beneath North Pond and the adjacent region.
### **Line 2**
Read in *2_surf_mid_grid.inp* as mesh object *mo*
### **Lines 5 - 11**
Create copies of mesh object *mo*. Check quality and status.
### **Lines 13 - 28**
Create volumes corresponding to aquifer layers beneath North Pond. Example syntax is:

- Line 13 - Subtract 100m from z-coordinate of mesh object *mo_aq100*
- Line 14 -  Extrude *mo_aq100* in the positive z-coordinate direction 100m and name *mo_aq100long*

Repeat these steps for layers mo_aq300long (100m - 300m below *mo*), mo_aq600long (300m - 600m below *mo*), and mo_aq1klong (600m - 1000m below *mo*). Check status and quality of volumes.

### **Lines 30 - 33**
Set attribute icr for volumes

### **Lines 35 - 38**
Dump mesh objects to files:

- *mo_aq100long* to *13_surf_filt_mid_grid_aq100.inp*
- *mo_aq300long* to *13_surf_filt_mid_grid_aq300.inp*
- *mo_aq600long* to *13_surf_filt_mid_grid_aq600.inp*
- *mo_aq1klong* to *13_surf_filt_mid_grid_aq1k.inp*
### **File link:**


## 11_region_set.lgi
| **Input** | **Output** |
| **Filename** | **Input** | **Output** |
| --- | --- | --- |
|*8_hex_refine_octree.inp*|*11_hex_refine_octree.inp*|
|*1_surf_domain.inp*||
|*4_surf_flat_zero.inp*||
|*7_surf_filt_aq100.inp*||
|*7_surf_filt_aq300.inp*||
|*7_surf_filt_aq600.inp*||
|*7_surf_filt_aq1k.inp*||
|*3_surf_mid_sb.inp*||
|*9_surf_north_pond_seafloor.inp*||
|*10_surf_filt_mid_grid_aq100.inp*||
|*10_surf_filt_mid_grid_aq300.inp*||
|*10_surf_filt_mid_grid_aq600.inp*||
|*10_surf_filt_mid_grid_aq1k.inp*||


## **Overview**
Read in previously made surfaces and set regions corresponding to layered seafloor units
### **Lines 1 - 30**
Read in hexahedral mesh *8_hex_refine_octree.inp* as *mo_hex*

Read in surfaces and convert to sheets surfaces in order to intersect with elements and filter material zones:
- *1_surf_domain.inp = domain*
- *4_surf_flat_zero.inp = sf_zero*
- *7_surf_filt_aq100.inp = mo_aq100*
- *7_surf_filt_aq300.inp = mo_aq300*
- *7_surf_filt_aq600.inp = mo_aq600*
- *7_surf_filt_aq1k.inp = mo_aq1k*
- *3_surf_mid_sb.inp = mo_sb*
- *9_surf_north_pond_seafloor.inp = np_sea*
- *10_surf_filt_mid_grid_aq100.inp = mo_aq100b*
- *10_surf_filt_mid_grid_aq300.inp = mo_aq300b*
- *10_surf_filt_mid_grid_aq600.inp = mo_aq600b*
- *10_surf_filt_mid_grid_aq1k.inp = mo_aq1kb*


### **Lines 33 - 34**
Select the refined simulation domain as the mesh object and attribute itetclr = 11. This attribute is what we will use to define material zones

### **Lines 37 - 84**
This section details the intersecting of sheets above with the refined mesh object inorder to define individual zones that correspond to geologic units within the grid. The general workflow of this section takes example from Lines 33 - 35:

- Line 37 - Define the region of interest and name it *r1* which is less than or equal to the sheet zero and greater than the surface *s_sb*. (s_sb < r1 ≤ zero)
- Line 38 - Define an element set, *e_r1*, from the region *r1*.
- Line 39 - See the itetclr of *mo_hex*  = 1

This process repeats for different conditions until line 81. The layers that correspond to the itetclr set in these lines are:

- 1 - Sediment
- 2 - Aquifer layer 0 - 100m
- 3 - Aquifer layer 100 - 300m
- 4 - Aquifer layer 300 - 600m
- 5 - Aquifer layer 600 - 1000m
- 6 - Conductive basement
- 7 - Aquifer volumne 100m (*mo_aq100b*)
- 8 - Aquifer volumne 300m (*mo_aq300b*)
- 9 - Aquifer volumne 600m (*mo_aq600b*)
- 10 - Aquifer volumne 1km (*mo_aq1kb*)


### **Lines 89 - 97**
Specify conditionals to set itetclr = 1 for all elements above *zero* and *sf* sheets. This is done very similarly to lines 27 - 58 and quality of elements is checked. This step makes sure no elements are taken out of the seafloor of North Pond.

### **Line 99**
Dump mesh object *mo_hex* to file  *11_hex_refine_octree.inp*
### **File link:**


## 12_remove_top.lgi
| **Input** | **Output** |
|--- | --- |
|11_hex_refine_octree.inp|12_tmp_hex_mesh.inp|
||12_tmp_tet_interp.inp|

## **Overview**
Translate mesh with hexahedral elements to mesh with tetrahedral elements. Filter and remove domain area above North Pond and bathymetric surface. Translate regions set in *11_region_set.lgi* to tetrahedral mesh.
### **Line 1**
Read in hexagonal mesh file *11_hex_refine_octree.inp* to mesh object *mo_octree*
### **Line 2**
Convert hexagonal mesh *mo_octree* to “clean” hexagonal mesh *mo_hex*, and check quality.  From lagrit manual:

“Parent elements in octree_mesh will be removed, so only the leaf elements remain. The result will be stored in new_mesh. This is often used to ensure a single valid mesh for other commands or for use outside of LaGriT. The octree attributes itetpar, itetkid, and itetlev will be updated in the new_mesh.”
### **Line 6**
Dump *mo_hex* to file *12_tmp_hex_mesh.inp*
### **Line 8 - 14**
Create new tetrahedral mesh object *mo_tet* and copy points from *mo_hex*. Set imt and itp attributes as constant for *mo_tet* and connect points
### **Lines 18 - 19**
Interpolate the cell colors from *mo_hex* to *mo_tet*.(i.e., material zones)
### **Lines 21 - 22**
Remove all of itetclr = 11 (i.e. region above North Pond and refined bathymetry) and remove any points without connections resulting from the removal.
### **Lines 26 - 27**
Replace the node material (imt) with the corresponding cell color (itetclr) as well as set node type (itp) based on itetclr.
### **Lines 29 - 34**
Check the quality of the mesh. Dump *mo_tet* to file *12_tmp_tet_interp.inp*.
### **File link:**


## 13_top_region_set.lgi
| **Input** | **Output** |
|--- | --- |
|12_tmp_tet_interp.inp|13_tet_reset_final.inp|
||{zone files}|
||{fehm files}|

## **Overview**
Set top region of North Pond to correct for removal of top of domain in *11_price_remove_top.lgi*. Reorder and sort nodes within simulation grid
### **Lines 1 - 4**
Read in *12_tmp_tet_interp.inp* as mesh object *mo* and check status
### **Line 6**
Dump outside zone numbers to zone files *tmp_test_outside_vor.area* and *tmp_test_outside.zone*
### **Lines 8 - 9**
Define attribute node_num from node numbers from mesh object *mo* and check status
### **Lines 11 - 15**
Filter points that lie on the top of the simulation domain given conditionals:

- Line 11 - Find points with “imt” attribute greater than 1 (i.e., points not in conductive basement) name them *todos*
- Line 12 - Find points with “top” attribute greater than 1 (i.e., points belonging to top zone) name them *ttop*
- Line 13 - Find the points that below to both of these point sets, name them *filter*
- Line 14 - Set imt = 2 to *filter* points in *mo*

Check the status and quality of steps above
### **Lines 19 - 21**
Sort nodes based on the rank of the node number, then x-, y-, and z-coordinate respectively. Then reorder the nodes based on the sorting.
### **Lines 23 - 24**
Dump the mesh object *mo* to file *13_tet_reset_final.inp*
### **File link:**


[Pseudo code: General Case](http://adamnicholasprice.github.io/GeologicGriddingTutorial/07_pseudoGeneral.html){: .btn .btn-purple } [Execute Scripts](http://adamnicholasprice.github.io/GeologicGriddingTutorial/08_executeScripts.html){: .btn .btn-purple }