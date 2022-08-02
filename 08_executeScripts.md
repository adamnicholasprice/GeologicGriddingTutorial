---
layout: post
title:  "Execute Scripts"
nav_order: 9
---
# Execute Scripts
<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---
## Generalized Mesh
### Syntax
```
lagrit< 1_create_surface.lgi && 
    lagrit < 2_refine_domain.lgi && 
    lagrit < 3_set_region_truncate.lgi
```

## North Pond

### Syntax
```
lagrit < 1_surf_domain.lgi &&
    lagrit < 2_surf_mid_grid.lgi &&
    lagrit < 3_surf_mid_sb.lgi &&
    lagrit < 4_surf_flat_zero.lgi &&
    lagrit < 5_surf_aq100.lgi &&
    lagrit < 6_surf_aq300_1k.lgi &&
    lagrit < 7_surf_filt.lgi &&
    lagrit < 8_driver_high_resolution_clipped.lgi &&
    lagrit < 9_surf_north_pond_seafloor.lgi &&
    lagrit < 10_surf_aq_volumes.lgi &&
    lagrit < 11_region_set.lgi &&
    lagrit < 12_remove_top.lgi &&
    lagrit < 13_top_region_set.lgi
```
[Pseudo code: North Pond](http://adamnicholasprice.github.io/GeologicGriddingTutorial/07_pseudoNorthPond.html){: .btn .btn-purple }[Home](http://adamnicholasprice.github.io/GeologicGriddingTutorial/index.html){: .btn .btn-purple } 