---
title: "Release Update: RRIM Toolbox for ArcGIS Pro with Massive-Raster Tiling & Multi-Core Batching"
---

### High‑Performance Terrain Visualization Engineered for Massive LiDAR & Statewide Datasets

<img class="pure-img" src="/images/UpperMississippiRiver.jpg" width="" height="" style="margin-bottom:10px; border:1px solid #000000;" alt="Darren Thornbrugh, Ph.D.">
*Image 1. Red Relief Image Map of the Mississippi River and floodplain in Minnesota near Jacobson MN, southeast of Grand Rapids, MN.*

A major production update to the **[RRIM_Toolbox](https://github.com/Darren-Thornbrugh/RRIM_Toolbox)** for ArcGIS Pro is now live. Engineered specifically for massive LiDAR-derived DEMs, statewide elevation collections, and big-data GIS production pipelines, this release introduces an end-to-end **2048 × 2048 windowed tiling engine** and optimized multi-core batch processing.

Memory allocation crashes (`numpy.core._exceptions._ArrayMemoryError`) on rasters exceeding 60,000 × 70,000 cells (16+ GB) are eliminated, allowing seamless terrain processing on standard workstation hardware.

---

## 🚀 What’s New in This Release

### 1. Unified 2048 × 2048 Windowed Tiling Architecture
- **Memory-Bounded Streaming:** Single-raster mode streams processing in discrete 2048 × 2048 blocks with halo buffers equal to kernel and search radii.
- **Seam-Free Output:** Edge-overlap math and post-index boundary smoothing eliminate tile seams, truncation, and raster collar artifacts across multi-gigabyte rasters while keeping RAM usage strictly constrained.

### 2. High-Speed SIMD Vectorized Openness Engine
- **Streamlined Workflow:** Topographic Openness now computes positive, negative, and the combined Openness Index in a single, high-throughput pass.
- **Hardware Acceleration:** In-place array operations cut computation times down to ~2.3 minutes on 100-megapixel (10k × 10k) tiles without RAM bloat.

### 3. Balanced Multi-Core Batch Processing
- Folder batch mode introduces protected queue batching that prevents external USB and network drive bottlenecks:
  - **Openness:** Processes batches of 8 using up to 8 concurrent workers.
  - **Slope:** Batches of 4 locked to 2 concurrent workers to prevent drive queue saturation.
  - **RRIM & RRIM Classic:** Batches of 8 locked to 2 concurrent workers to handle dual-raster reads and 3-band writes safely.
- Real-time telemetry reports per-tile durations, remaining queue counts, moving averages, and overall elapsed times.

### 4. True 3-Band RGB Output with Instant Pro Symbology
- Both Custom (Cividis) and Classic (Grayscale) RRIM tools generate standard 3-band 8-bit unsigned (`GDT_Byte`) GeoTIFFs with automated `<PAMDataset>` (`DataType=Processed`) `.aux.xml` sidecars.
- **No manual symbology tweaks:** ArcGIS Pro immediately recognizes the output as pre-rendered RGB with `Stretch: None`, avoiding unwanted 2% contrast stretches or color-cast distortions.
- Transparent footprint masking ensures non-terrain boundary pixels display seamlessly over basemaps.

---

## 🛠️ The Core Tool Suite

1. **Topographic Openness Index** — Single-pass zenith/nadir radial scanning with boundary-safe smoothing.
2. **Slope from DEM** — Optimized 3×3 Horn slope calculator with seamless tile edge preservation.
3. **Red Relief Image Map (Custom)** — Perceptually uniform Cividis openness blended with a chromatic red slope overlay ($\alpha = 0.7$).
4. **Red Relief Image Map (Classic)** — High-contrast, traditionally styled grayscale openness blended with a red slope overlay, oriented for accurate, non-inverted relief perception.

---

## 📦 Dependency‑Free Architecture

Runs directly out of the box within ArcGIS Pro’s default Python environment. No extra environments, custom wheels, or external libraries required:

- ArcGIS Pro 3.x (`arcpy`)  
- NumPy (bundled)  
- GDAL / OGR (bundled)  

---

## 🔗 GitHub Repository  
Download the toolbox, explore the source code, and read the updated documentation:

**[RRIM Toolbox on GitHub](https://github.com/Darren-Thornbrugh/RRIM_Toolbox)**

---

## 📘 Documentation & Technical Specifications  
Full parameter matrices, architectural details, and production tiling tips:

**[RRIM Toolbox - README](https://github.com/Darren-Thornbrugh/RRIM_Toolbox/blob/main/README.md)**

---

## 🧭 System Requirements

- **Host Application:** ArcGIS Pro 3.0 or later  
- **Python Environment:** Default `arcgispro-py3` (Python 3.9+)  
- **Operating System:** Windows 10 / 11 / Server 2019+  
- **Storage Compatibility:** Local NVMe SSDs, external USB HDDs, and network storage shares (UNC paths)  

---

## Author  
**Darren J. Thornbrugh**  
USDA Forest Service — Spatial Analysis, Remote Sensing & Ecological Modeling