# Project 6: Spatial Interpolation and Visualization of LiDAR Data

[Github Repo](https://github.com/AgnessChisale/spatial_interpolation_visualisation_lidar)

## Overview
This project uses LiDAR data collected over the UBC Malcolm Knapp Research Forest (MKRF) in Maple Ridge, British Columbia, to generate and evaluate Digital Elevation Models (DEMs) using multiple spatial interpolation approaches. Ground return points were extracted, filtered, and processed using PDAL, and three interpolation methods — Natural Neighbor, Inverse Distance Weighting (IDW), and Spline — were compared against a binned reference DEM using zonal statistics across elevation and slope classes.

---

## Objectives
- Preprocess a LiDAR point cloud using PDAL pipelines
- Visualize point cloud data in 2D and 3D in QGIS and ArcGIS Pro
- Generate a reference DEM using binning from a full-density point cloud
- Apply and compare three spatial interpolation methods on a thinned point cloud
- Evaluate interpolation accuracy across elevation and slope zones using zonal statistics

---

## Data Sources & Tools

**Data**
| Layer | Description | Source |
|-------|-------------|--------|
| `UBC_MKRF_LiDAR_2016` (16 tiles) | LiDAR point cloud tiles over MKRF, collected 2016 | [MGEM Data Store](https://206-12-122-94.cloud.computecanada.ca/UBC_MKRF_LiDAR_2016/) |
| `tile_index.geojson` | Spatial tile index for the LiDAR collection | MGEM Data Store |

**Tools**
| Tool | Purpose |
|------|---------|
| PDAL | Point cloud filtering, cropping, merging, and thinning |
| QGIS | Tile selection, point cloud visualization, and 3D mapping |
| ArcGIS Pro | DEM generation, interpolation, zonal statistics, and 3D scene visualization |

---

## Methods
Sixteen LiDAR tiles covering the AOI were identified using a spatial tile index and downloaded from the MGEM Data Store. A PDAL pipeline was used to filter ground returns, crop to the AOI, and merge tiles into a single LAS file. A thinned point cloud was also created by sampling one point per 5 m radius sphere. In ArcGIS Pro, a reference DEM was generated using binning at 1 m resolution. Three interpolation methods (Natural Neighbor, IDW, Spline) were applied to the thinned point cloud and difference rasters were computed against the reference DEM. Zonal statistics were calculated across reclassified elevation and slope zones to quantitatively evaluate each method.

📄 *For a detailed breakdown of the methodology, [click here](methodology.md)*

---

## Outputs

- `map_panel_dems_and_differences.pdf` — Panel map showing all three interpolated DEMs (shaded relief) and their difference rasters compared to the binned reference DEM
- Zonal statistics table showing mean and standard deviation of difference rasters across elevation and slope classes

---

## Key Findings
- The three interpolation methods produced visibly different surfaces, particularly in areas of steep terrain
- Difference rasters revealed that interpolation error was generally higher in high-slope zones than in low-slope zones
- The binning approach using full-density LiDAR served as the most reliable reference surface

---

## Skills Learned
- Reading and interpreting LiDAR metadata and tile indexes
- Writing and executing PDAL pipelines for point cloud processing
- Filtering, cropping, merging, and thinning LiDAR point clouds
- Visualizing point clouds in 2D and 3D in QGIS
- Creating LAS Datasets and generating DEMs in ArcGIS Pro
- Applying Natural Neighbor, IDW, and Spline interpolation methods
- Computing difference rasters using the Raster Calculator
- Reclassifying rasters and running zonal statistics
- Visualizing interpolated surfaces in a 3D ArcGIS Pro Local Scene
