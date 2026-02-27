# Methodology: Project 6 — Spatial Interpolation and Visualization of LiDAR Data

This document contains the detailed step-by-step methodology followed in Project 6.

---

## Task 1: Preprocess LiDAR Data in PDAL
- Navigated to the MGEM Data Store and inspected the UBC MKRF 2016 LiDAR collection metadata
- Loaded the `tile_index.geojson` directly into QGIS via remote URL
- Defined the AOI as a bounding box GeoJSON polygon and saved it as `mkrf_aoi.geojson`
- Used Select by Location in QGIS to identify the 16 tiles intersecting the AOI
- Downloaded all 16 LiDAR tiles (.copc.laz) to the local project folder
- Wrote a PDAL JSON pipeline to filter ground returns (Classification 2), crop to the AOI bounds, merge all tiles, and write the output as `mkrf_aoi_lidar.las`
- Ran the pipeline from the OSGeo4W shell using `pdal pipeline process-lidar.json`
- Created a thinned point cloud (`thinned.las`) by sampling one point per 5 m radius sphere using `pdal translate`

---

## Task 2: Visualize LiDAR Data in QGIS
- Loaded `mkrf_aoi_lidar.las` into QGIS and applied attribute-based symbology using Intensity, ReturnNumber, and ScanAngleRank
- Created a 3D map view in QGIS with Eye Dome Lighting enabled
- Navigated the 3D point cloud using SHIFT, CTRL, ALT, and scroll controls to observe terrain variation

---

## Task 3: Prepare LiDAR in ArcGIS Pro
- Created LAS Datasets for both the full and thinned point clouds using the Create LAS Dataset tool, with statistics computed and PRJ files generated
- Compared point count, average point spacing, and elevation range between the full and thinned datasets
- Generated a 1 m resolution reference DEM (`MKRF_DEM`) from the full point cloud using the LAS Dataset to Raster tool with Binning interpolation type and Minimum cell assignment
- Converted the thinned LAS Dataset to a multipoint feature class using LAS to Multipoint, then split to singlepart features
- Added Z values as attributes using the Add Z Information tool, producing a 2D point feature class with elevation values for interpolation

---

## Task 4: Apply and Evaluate Spatial Interpolation Algorithms
- Generated three 1 m resolution DEMs from the thinned point cloud using Natural Neighbor, IDW, and Spline interpolation methods
- Calculated three difference rasters by subtracting each interpolated surface from `MKRF_DEM` using the Raster Calculator (`spline_diff`, `nn_diff`, `idw_diff`)
- Reclassified `MKRF_DEM` into three elevation zones (low, medium, high) using the Reclassify tool
- Derived a slope raster from `MKRF_DEM` using the Slope tool and reclassified it into three slope zones
- Ran Zonal Statistics as Table for all six combinations of difference raster and zone raster, producing six summary tables
- Produced a panel map layout with all interpolated DEMs (shaded relief symbology) and difference rasters, including summary statistics, north arrow, and labels
- Exported the map layout as a PDF

---

## Task 5: Visualize 3D Data in ArcGIS Pro
- Created a new Local Scene in ArcGIS Pro and added the LiDAR LAS file
- Added interpolated DEM rasters under the Ground surface layer and difference rasters as 2D layers
- Applied diverging colour stretch symbology to difference rasters with a zero midpoint
- Toggled DEM and difference raster pairs to compare interpolated surfaces against the raw point cloud in 3D

---

*Back to [README](README_project6.md)*
