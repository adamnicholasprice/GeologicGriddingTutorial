---
layout: post
title:  "Constructing and refining domain"
nav_order: 4
has_children: true
---
# Constructing and refining domain

[Placeholder]

## Generalized Mesh

| **Filename** | **Input** | **Output** |
| --- | --- | --- |
| _2_refine_domain.lgi_ | _1_surface_aq200.inp_ | _2_initial_domain.inp_ |
|                       | _1_surface_aq800.inp_ | _2_hexRefine_octree.inp_ |
|                       | _1_surface.inp_ | _2_surface_octree.inp_ |
|                       |                       | _2_surfaceAQ200_octree.inp_ |
|                       |                       | _2_surfaceAQ800_octree.inp_ |

### Overview

#### Figures
<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/GeneralScene/";
    var file = "1_surface.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>
## North Pond

### Overview

#### Figures
<script>
    var app = "https://kitware.github.io/paraview-glance/app";
    var datadir = "https://raw.githubusercontent.com/adamnicholasprice/GeologicGriddingTutorial/main/GeneralScene/";
    var file = "1_surface.vtkjs";

    document.write("<iframe src='" + app + "?name=" + file + "&url=" +datadir + file + "' id='iframe' width='800' height='500'></iframe>");
</script>

[Constructing refining surfaces](http://adamnicholasprice.github.io/GeologicGriddingTutorial/02_surfaces.html){: .btn .btn-purple } [Define regions and assign materials](http://adamnicholasprice.github.io/GeologicGriddingTutorial/04_defineRegions.html){: .btn .btn-purple }
