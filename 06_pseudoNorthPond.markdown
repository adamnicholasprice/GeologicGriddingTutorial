---
layout: post
title:  "Pseudo code: North Pond"
nav_order: 8
---

# Pseudo code: North Pond

## **Abstract**
General flow:

Files 1 - 3: reading in files and making reference surfaces

Files 5 - 7: making separate surfaces for refining aquifer layers

File 8: Grid refinement

File 9: Setting seafloor at North Pond

File 10: Setting regions based on surfaces

File 11: Removing the top of the grid, going from hex elements to tet elements

File 12: Set outside zones

File 13: Construct volumes for area of interest to define zones with

File 14: Set material zones for area of interest and dump FEHM files



|**FIlename**|**Input**|**Output**|
| :-: | :-: | :-: |
|<h1>**1\_price\_surf\_domain.lgi**</h1>|function6|1\_surf\_domain.inp|
## **Overview**
Create mesh object for bathymetric surface.
### **Lines 1 - 11**
Assign coordinate system in meters from xmin, ymin, zmin = -30000,-75000, -5000 to xmax, ymax, zmax  = 30000,75000,0 and number of points in x,y,z = 301, 301,6 to determine spacing.
### **Lines 14 - 18**
Create a mesh object surface, *mo\_tri*, that is the above x,y dimensions with singular z value (2D plane). Setting z coordinates to zero before connecting allows defining an explicit x,y coordinate system and connecting that surface without producing oddly shaped elements.  
### **Lines 23-24**
Creates mesh object (*sedbase*) from the topography function from R (*function6*)
### **Line 28**
Assign z coordinates from *sedbase* to connected triplane mesh object *mo\_tri*
### **Lines 32 - 37**
Reset property values of mesh object to insure all the same value
### **Lines 39 - 41**
Dump surface to file 1\_surf\_domain.inp
### **File link:**



|**FIlename**|**Input**|**Output**|
| :-: | :-: | :-: |
|<h1>**2\_price\_surf\_mid\_grid.lgi**</h1>|2\_middle\_grid.csv|2\_surf\_mid\_grid.inp|
## **Overview**
Create mesh object from trimmed area of interest created from truncated bathymetric surface representing North Pond and immediately adjacent region created outside of LaGriT
### **Lines 4 - 8**
Read in *2\_middle\_grid.csv,* create a mesh object (*sedbase*), set z coordinates to zero, then connect. Setting z coordinates to zero before connecting allows defining an explicit x,y coordinate system and connecting that surface without producing oddly shaped elements.  
### **Lines 10 - 14**
Read in *2\_middle\_grid.csv*, create a separate mesh object *temp*, then copy z coordinates from temp to connected surface *sedbase*
### **Lines 17 - 22**
Reset property values of mesh object to insure all the same value
### **Lines 26 - 28**
Dump surface to file *2\_surf\_mid\_grid.inp*
### **File link:**



|**FIlename**|**Input**|**Output**|
| :-: | :-: | :-: |
|<h1>**3\_price\_surf\_mid\_sb.lgi**</h1>|3\_middle\_sb.csv|3\_surf\_mid\_sb.inp|
## **Overview**
Create mesh object from trimmed area of interest created from truncated bathymetric surface representing sediment = basement interface beneath North Pond created outside of LaGriT
### **Lines 4 - 8**
Read in *3\_middle\_sb.csv,* create a mesh object (*sedbase*), set z coordinates to zero, then connect. Setting z coordinates to zero before connecting allows defining an explicit x,y coordinate system and connecting that surface without producing oddly shaped elements.  
### **Lines 10 - 13**
Read in *3\_middle\_sb.csv,* create a separate mesh object (*temp*), then copy z coordinates from temp to connected surface sedbase
### **Lines 17 - 22**
Reset property values of mesh object to insure all the same value
### **Lines 26 - 28**
Dump surface to file *3\_surf\_mid\_sb.inp*
### **File link:**



|**FIlename**|**Input**|**Output**|
| :-: | :-: | :-: |
|<h1>**4\_price\_surf\_flat\_zero.lgi**</h1>|None|4\_surf\_flat\_zero.inp|
## **Overview**
Construct a flat surface at z = 0 at the same spatial extent as *2\_middle\_grid.csv*.
### **Lines 2 - 5**
Specify extent of surface xmin, ymin = -15000, -6000 to xmax, ymax = 4600, 28500
### **Lines 7 - 9**
Specify number of x,y points (resolution) at 2 each
### **Lines 11 - 15**
Create mesh object *mo\_tri*, create points on that mesh object, and connect
### **Lines 17 - 18**
Check quality of connections for oddly shaped elements
### **Lines 20 - 22**
Dump mesh object *mo\_tri* to file *4\_surf\_flat\_zero.inp*
### **File link:**


|**FIlename**|**Input**|**Output**|
| :-: | :-: | :-: |
|<h1>**5\_price\_surf\_aq100.lgi**</h1>|3\_surf\_mid\_sb.inp|5\_surf\_aq100.inp|
## **Overview**
Construct a volume that represents a 100m thick aquifer section directly beneath North Pond.
### **Line 1**
Read in *3\_surf\_mid\_sb.inp* to mesh object *mid\_sb*
### **Line 4**
Extrude a volume that goes from sediment = basement interface to 100m below that interface.
### **Line 6**
Check the quality of the extrude
### **Lines 9 - 11**
Convert the volume from hexagonal elements to tetrahedral elements and check the quality of that conversion
### **Lines 13 - 15**
Dump *mo\_tet* to file *5\_surf\_aq100.inp*
### **File link:**

|**FIlename**|**Input**|**Output**|
| :-: | :-: | :-: |
|<h1>**6\_price\_surf\_aq300\_1k.lgi**</h1>|2\_surf\_mid\_grid.inp|6\_surf\_aq300\_1k.inp|
## **Overview**
## Construct a volume that represents a 900 m thick aquifer section directly beneath the region representing North Pond.
### **Lines 2 - 4**
Read in *2\_surf\_mid\_grid.inp* to mesh object *mo* and subtract 100m from all z coordinates.
### **Lines 6 - 8**
Extrude *mo* surface to a volume *mo\_long*  that goes from 100 - 1000m below *2\_surf\_mid\_grid.inp* surface representing local North Pond bathymetry.
### **Lines 10 - 13**
Convert the volume from hexagonal elements, *mo\_long,* to tetrahedral elements, *mo\_tet,* and check the quality of that conversion.
### **Lines 15 - 17**
Dump *mo\_tet* to file *6\_surf\_aq300\_1k.inp*
### **File link:**


|**FIlename**|**Input**|**Output**|
| :-: | :-: | :-: |
|<h1>**7\_price\_surf\_filt.lgi**</h1>|1\_surf\_domain.inp|7\_surf\_filt\_aq100.inp|
|||7\_surf\_filt\_aq300.inp|
|||7\_surf\_filt\_aq600.inp|
|||7\_surf\_filt\_aq1k.inp|
## **Overview**
Construct four surfaces at -100, -300,-600, and -1000m at the extent of the entire domain.
### **Line 1**
Read in *7\_price\_surf\_filt.lgi* to mesh object *mo*
### **Lines 4 - 7**
Create copies of *mo* to :

- *mo\_aq100*
- *mo\_aq300*
- *mo\_aq600*
- *mo\_aq1k*
### **Lines 12 - 15**
Subtract 100, 300, 600, and 1000m from the z coordinates of the copied mesh objects respectively and check the quality of those objects
### **Lines 20 - 25**
Dump mesh objects to files:

- *mo\_aq100* to *7\_surf\_filt\_aq100.inp*
- *mo\_aq300* to *7\_surf\_filt\_aq300.inp*
- *mo\_aq600* to *7\_surf\_filt\_aq600.inp*
- *mo\_aq1k* to *7\_surf\_filt\_aq1k.inp*
### **File link:**


|**FIlename**|**Input**|**Output**|
| :-: | :-: | :-: |
|<h1>**8\_price\_driver\_high\_resolution\_clipped.lgi**</h1>|1\_surf\_domain.inp|8\_1\_octree\_NP.inp|
||3\_surf\_mid\_sb.inp|8\_2\_octree\_aq100.inp|
||4\_surf\_flat\_zero.inp|8\_3\_octree\_aq300.inp|
||5\_surf\_aq100.inp|8\_4\_octree\_dom.inp|
||6\_surf\_aq300\_1k.inp|8\_4\_octree\_domaq100.inp|
||7\_surf\_filt\_aq100.inp|8\_5\_octree\_domaq1k.inp|
||7\_surf\_filt\_aq300.inp|8\_6\_octree\_domaq300.inp|
||7\_surf\_filt\_aq600.inp|8\_7\_octree\_domaq600.inp|
||7\_surf\_filt\_aq1k.inp|8\_hex\_refine\_octree.inp|
## **Overview**
This file is the key file in building the domain. First, we read in all the surfaces created in the previous files. Next, the subroutine *refine\_object.mlgi* is called to perform octree refinement along the elements intersected with the surfaces called and files are created for each of those refinement iterations. The final file with refinements along all desired surfaces is dumped to *8\_hex\_refine\_octree.inp*
### **Lines 1 - 14**
Define xmin, ymin, zmin = -24000,-40000, -6000 to xmax, ymax, zmax  = 16000,55000,1500 and number of points in x,y,z = 41, 96,11. This sets the initial resolution at 1000m elements in x,y,z. Create the mesh object *mo\_hex* for refinement.
### **Lines 17 - 25**
Read in surfaces:

- *1\_surf\_domain.inp = domain*
- *4\_surf\_flat\_zero.inp = sf\_zero*
- *5\_surf\_aq100.inp = aq100*
- *6\_surf\_aq300\_1k.inp =aq300\_1k*
- *3\_surf\_mid\_sb.inp = mo\_sb*
- *7\_surf\_filt\_aq100.inp = domain\_aq100*
- *7\_surf\_filt\_aq1k.inp = domain\_aq1k*
- *7\_surf\_filt\_aq600.inp = domain\_aq600*
- *7\_surf\_filt\_aq300.inp = domain\_aq300*
### **Line 28**
Define the mesh object *mo\_hex* as the object to perform octree refinement on
### **Lines 32 - 36**
This series of lines performs the octree refinement and will be performed many times in this file. The flow of these lines are:

- Line 32: Define the mesh object to refine with. Here is *mo\_sb*
- Line 33: Perform octree refinement via the subroutine *refine\_object.mlgi*
- Line 34: Dump the refined object to a file. Here *8\_1\_octree\_NP.inp*
- Line 36: Check the quality of the refinement.

Henceforth, this series of steps will be denoted by: Refine {*name of mesh object*} - File: {*Output file*}
### **Lines 40 - 43**
Refine *aq100 -* File: *8\_2\_octree\_aq100.inp*
### **Lines 47 - 52**
Refine x2 *aq300\_1k -* File: *8\_3\_octree\_aq300.inp*
### **Lines 57 - 60**
Refine x2 *domain* - File: *8\_4\_octree\_dom.inp*
### **Lines 64 - 75**
Refine *domain\_aq100* - File: *8\_4\_octree\_domaq100.inp*

This refinement has a few additional steps outside the subroutine but before dumping the mesh object.

- Line 68: Create a mesh object attribute named *in\_inter*  that contains the intersection of the mesh object being refined and mesh object being used to refine in this step
- Line 69: Create an element set named *erefine1* based on where *in\_inter* is greater than 0 (i.e. where the meshes intersect)
- Line 70: Create an element set named *num\_ref1*  based on elements that have been refined less than 2 times.
- Line 71: Create an element set named *combined1* that is the union of elements from *erefine1* and *num\_ref1*
- Line 72: Refine the element set *combined1*

These lines are very similar to the subroutine *refine\_object.mlgi* with the caveat that we are also imposing an additional condition that we only want to refine elements that have 1 level of refinement. This ensures that we are not over-refining elements creating a computationally intensive simulation grid. Henceforth, we will refer to this process as *itet-refine*
### **Lines 79 - 91**
Refine *domain\_aq1k* - File: *8\_5\_octree\_domaq1k.inp* plus *itet-refine*
### **Lines 96 - 108**
Refine *domain\_aq300* - File: *8\_6\_octree\_domaq300.inp* plus *itet-refine*
### **Lines 112 - 126**
Refine *domain\_aq600* - File: *8\_7\_octree\_domaq600.inp* plus *itet-refine*
### **Lines 128 - 142**
Read in surface *2\_surf\_mid\_grid.inp* to mesh object *mid\_grid*. Perform a itet-refine on elements with less than 4  refinements (itetlev < 4).
### **Line 144**
Reset all elements to color 7
### **Lines 146 - 151**
Dump *mo\_hex* to *8\_hex\_refine\_octree.inp*
### **File link:**


|**FIlename**|**Input**|**Output**|
| :-: | :-: | :-: |
|<h1>**refine\_object.mlgi**</h1>|{Element to be refined}|{Refining element}|
## **Overview**
Subroutine used to perform octree refinement of a mesh object along a specified surface or volume
### **Lines 1 - 2**
Clear any variables from previous use of subroutine
### **Line 4**
Create a mesh object attribute named *if\_inter*  that contains the intersection of the mesh object being refined and the mesh object being used to refine in this step. These are defined outside the subroutine
### **Line 5**
Create an element set named *erefine* based on where *if\_inter* is greater than 0 (i.e. where the meshes intersect)
### **Line 6**
The selected set, *erefine,* is refined.
### **File link:**



|**FIlename**|**Input**|**Output**|
| :-: | :-: | :-: |
|<h1>**9\_price\_surf\_north\_pond\_seafloor.lgi**</h1>|3\_middle\_sb.csv|9\_surf\_north\_pond\_seafloor.inp|
## **Overview**
Make a surface corresponding to x,y coordinates of North Pond sediment with z coordinates = 0
### **Lines 4 - 8**
Create triplane mesh object *sedbase*. Read in *3\_middle\_sb.csv* as the x,y,z coordinates of *sedbase*. Set z-coordinates = 0 and connect points.
### **Lines 11 - 15**
Reset all material and color properties associated with *sedbase*
### **Lines 19 - 21**
Dump mesh object *sedbase* to *9\_surf\_north\_pond\_seafloor.inp*
### **File link:**


|**FIlename**|**Input**|**Output**|
| :-: | :-: | :-: |
|<h1>**10\_price\_region\_set.lgi**</h1>|3\_middle\_sb.csv|10\_hex\_refine\_octree\_pre.inp|
||1\_surf\_domain.inp|10\_hex\_refine\_octree.inp|
||4\_surf\_flat\_zero.inp||
||7\_surf\_filt\_aq100.inp||
||7\_surf\_filt\_aq300.inp||
||7\_surf\_filt\_aq600.inp||
||7\_surf\_filt\_aq1k.inp||
||3\_surf\_mid\_sb.inp||
||9\_surf\_north\_pond\_seafloor.inp||
## **Overview**
Read in previously made surfaces and set regions corresponding to layered seafloor units
### **Lines 1 - 10**
Read in surfaces:

- *1\_surf\_domain.inp = domain*
- *4\_surf\_flat\_zero.inp = sf\_zero*
- *7\_surf\_filt\_aq100.inp = mo\_aq100*
- *7\_surf\_filt\_aq300.inp = mo\_aq300*
- *7\_surf\_filt\_aq600.inp = mo\_aq600*
- *7\_surf\_filt\_aq1k.inp = mo\_aq1k*
- *3\_surf\_mid\_sb.inp = mo\_sb*
- *9\_surf\_north\_pond\_seafloor.inp = np\_sea*
### **Lines 12 - 19**
Convert mesh objects read in above to sheets in order to intersect with elements and filter for geometry
### **Lines 22 - 24**
Select the refined simulation domain as the mesh object and reset all itet colors = 7
### **Lines 27 - 58**
This section details the intersecting of sheets above with the refined mesh object inorder to define individual zones that correspond to geologic units within the grid. The general workflow of this section takes example from Lines 33 - 35:

- Line 33 - Define the region of interest and name it *r1* which is less than or equal to the sheet zero and greater than the surface *s\_sb*. (s\_sb < r1 ≤ zero)
- Line 34 - Define an eltset, *e\_r1*, from the region *r1*.
- Line 35 - See the itetclr of *mo\_hex*  = 1

This process repeats for different conditions until line 58. The layers that correspond to the itetclr set in these lines are:

- 1 - Sediment
- 2 - Aquifer layer 0 - 100m
- 3 - Aquifer layer 100 - 300m
- 4 - Aquifer layer 300 - 600m
- 5 - Aquifer layer 600 - 1000m
- 6 - Conductive basement
- 7- Everything else (i.e., the area above itetclr 1 and 2)
### **Lines 62 - 70**
Specify conditionals to set itetclr = 7 for all elements above *zero* and *sf* sheets. This is done very similarly to lines 27 - 58 and quality of elements is checked.
### **Lines 72 - 75**
Dump mesh object *mo\_hex* to files *10\_hex\_refine\_octree\_pre.inp* and *10\_hex\_refine\_octree.inp*
### **File link:**


|**FIlename**|**Input**|**Output**|
| :-: | :-: | :-: |
|<h1>**11\_price\_remove\_top.lgi**</h1>|10\_hex\_refine\_octree.inp|11\_tmp\_hex\_mesh.inp|
|||11\_tmp\_tet\_interp.inp|
## **Overview**
Translate mesh with hexagonal elements to mesh with tetrahedral elements. Filter and remove domain area above North Pond and bathymetric surface. Translate regions set in *10\_price\_region\_set.lgi* to tetrahedral mesh.
### **Line 1**
Read in hexagonal mesh file *10\_hex\_refine\_octree.inp* to mesh object *mo\_octree*
### **Lines 2 - 4**
Convert hexagonal mesh *mo\_octree* to “clean” hexagonal mesh *mo\_hex*, and check quality.  From lagrit manual:

“Parent elements in octree\_mesh will be removed, so only the leaf elements remain. The result will be stored in new\_mesh. This is often used to ensure a single valid mesh for other commands or for use outside of LaGriT. The octree attributes itetpar, itetkid, and itetlev will be updated in the new\_mesh.”
### **Line 6**
Dump *mo\_hex* to file *11\_tmp\_hex\_mesh.inp*
### **Line 8 - 14**
Create new tetrahedral mesh object *mo\_tet* and copy points from *mo\_hex*. Set imt and itp attributes as constant for *mo\_tet* and connect points
### **Lines 18 - 19**
Interpolate the cell colors from *mo\_hex* to *mo\_tet*.
### **Lines 21 - 22**
Remove all of itetclr = 7 (i.e. region above North Pond and bathymetry) and remove any points without connections resulting from the removal.
### **Lines 26 - 27**
Replace the node material (imt) with the corresponding cell color (itetclr) as well as set node type (itp) based on itetclr.
### **Lines 29 - 34**
Check the quality of the mesh. Dump *mo\_tet* to file *11\_tmp\_tet\_interp.inp*.
### **File link:**

|**FIlename**|**Input**|**Output**|
| :-: | :-: | :-: |
|<h1>**12\_price\_top\_region\_set.lgi**</h1>|11\_tmp\_tet\_interp.inp|12\_tet\_reset\_final.inp|
|||12\_tet\_reset\_final.gmv|
## **Overview**
Set top region of North Pond to correct for removal of top of domain in *11\_price\_remove\_top.lgi*. Reorder and sort nodes within simulation grid
### **Lines 1 - 4**
Read in *11\_tmp\_tet\_interp.inp* as mesh object *mo* and check status
### **Line 6**
Dump outside zone numbers to files *tmp\_test\_outside\_vor.area* and *tmp\_test\_outside.zone*
### **Lines 8 - 9**
Define attribute node\_num from node numbers from mesh object *mo* and check status
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
Dump the mesh object *mo* to file *12\_tet\_reset\_final.inp*
### **File link:**


|**FIlename**|**Input**|**Output**|
| :-: | :-: | :-: |
|**13\_price\_surf\_aq300\_bubbles.lgi**|2\_surf\_mid\_grid.inp|13\_surf\_filt\_mid\_grid\_aq100.inp|
|||13\_surf\_filt\_mid\_grid\_aq300.inp|
|||13\_surf\_filt\_mid\_grid\_aq600.inp|
|||13\_surf\_filt\_mid\_grid\_aq1k.inp|
## **Overview**
Construct volumes corresponding to the aquifer units beneath North Pond and the adjacent region.
### **Line 2**
Read in *2\_surf\_mid\_grid.inp* as mesh object *mo*
### **Lines 5 - 11**
Create copies of mesh object *mo*. Check quality and status.
### **Lines 13 - 28**
Create volumes corresponding to aquifer layers beneath North Pond. Example syntax is:

- Line 13 - Subtract 100m from z-coordinate of mesh object *mo\_aq100*
- Line 14 -  Extrude *mo\_aq100* in the positive z-coordinate direction 100m and name *mo\_aq100long*

Repeat these steps for layers mo\_aq300long (100m - 300m below *mo*), mo\_aq600long (300m - 600m below *mo*), and mo\_aq1klong (600m - 1000m below *mo*). Check status and quality of volumes.
### **Lines 30 - 35**
Dump mesh objects to files:

- *mo\_aq100long* to *13\_surf\_filt\_mid\_grid\_aq100.inp*
- *mo\_aq300long* to *13\_surf\_filt\_mid\_grid\_aq300.inp*
- *mo\_aq600long* to *13\_surf\_filt\_mid\_grid\_aq600.inp*
- *mo\_aq1klong* to *13\_surf\_filt\_mid\_grid\_aq1k.inp*
### **File link:**





|**FIlename**|**Input**|**Output**|
| :-: | :-: | :-: |
|**14\_price\_aq300\_set.lgi**|12\_tet\_reset\_final.inp|14\_tet\_reset\_final.inp|
||13\_surf\_filt\_mid\_grid\_aq100.inp|14\_tet\_reset\_final.gmv|
||13\_surf\_filt\_mid\_grid\_aq300.inp|final\_mesh (files)|
||13\_surf\_filt\_mid\_grid\_aq600.inp||
||13\_surf\_filt\_mid\_grid\_aq1k.inp||
## **Overview**
Set material regions corresponding to aquifer regions beneath North Pond and adjacent region.
### **Lines 1**
Read in *12\_tet\_reset\_final.inp* mesh as mesh object *mo*
### **Lines 4 - 13**
Read in meshes and define mesh objects:

- *13\_surf\_filt\_mid\_grid\_aq100.inp* to  *mo\_aq100b*
- *13\_surf\_filt\_mid\_grid\_aq300.inp* to *mo\_aq300b*
- *13\_surf\_filt\_mid\_grid\_aq600.inp* to *mo\_aq600b*
- *13\_surf\_filt\_mid\_grid\_aq1k.inp* to *mo\_aq1kb*

Then convert to sheets:

- *mo\_aq100b* to *aq100*
- *mo\_aq300b* to *aq300*
- *mo\_aq600b* to *aq600*
- *mo\_aq1kb*  to *aq1k*
### **Line 16**
Select *mo* to filter on
### **Lines 19 - 33**
Filter points that are contained within the regions of the simulation domain given conditionals. Example syntax is:

Line 19 - Define region *r11* that is less than *aq100*

Line 20 - Define pset *e\_r11* that contains all points within region *r11*

Line 21 - Set imt = 8 for all points in *e\_r11*

Complete this for regions and pointsets for imt = 8, 9, 10.

The layers that correspond to the imt set in these lines are:

- 7 - Aquifer layer 0 - 100m in immediate North Pond region
- 8 - Aquifer layer 100 - 300m in immediate North Pond region
- 9 - Aquifer layer 300 - 600m in immediate North Pond region
- 10 - Aquifer layer 600 - 1000m in immediate North Pond region
### **Lines 37 - 40**
Reset all top nodes to imt = 2 in case they were reset in previous lines
### **Lines 43 - 44**
Dump *mo* to files *14\_tet\_reset\_final.inp*, *14\_tet\_reset\_final.gmv*, and FEHM mesh files
### **File link:**

# **Files:**
## **Function6**
Bathymetric function.
## **2\_mid\_grid.csv**
Clipped bathymetry file representing North Pond and surrounding area. Used for high resolution filtering
## **3\_middle\_sb.csv**
Clipped bathymetry file representing just the sediment = basement interface.
