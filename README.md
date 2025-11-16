# Raster Tile Mosaicing Project

## Overview
This project automates the creation of seamless, georeferenced mosaics from multiple adjacent satellite raster tiles (GeoTIFFs). The workflow intelligently handles tiles with varying resolution and coordinate systems by validating metadata, resampling to a common grid, and merging them into a single, cohesive output without visible seams or distortions.

**Perfect for:** Satellite imagery analysis, orthomosaic generation, multi-source raster consolidation, and large-scale geospatial data processing.

## Key Features
-  **Metadata Validation:** Automatically detects and reports CRS and resolution mismatches across tiles
-  **Intelligent Resampling:** Uses cubic interpolation to ensure smooth, high-quality output when resolution differs
-  **Seamless Merging:** Combines adjacent tiles into a single coherent raster
-  **Compression:** Automatically compresses output with LZW encoding for efficient storage
-  **Visualization:** Displays the final mosaic with proper geospatial metadata
-  **Robust Error Handling:** Validates all inputs and provides detailed error messages

## Requirements

### Python Version
- Python 3.8 or higher

### Dependencies
| Library | Purpose |
|---------|---------|
| **Rasterio** | Read, write, merge, and reproject geospatial raster data |
| **Pandas** | File management and metadata organization |
| **NumPy** | Underlying array operations and data manipulation |
| **Matplotlib** | Visualization of the final mosaic output |
| **rioxarray** | Advanced xarray-rasterio integration for raster analysis |

### Installation
Install all required packages with:
```bash
pip install numpy pandas matplotlib rasterio rioxarray
```

Or use the provided notebook, which includes installation commands in the first cell.

## Project Structure
```
raster_tile_mosaicing_folder/
├── raster_tile_mosaicing.ipynb    # Main Jupyter Notebook with complete workflow
├── README.md                       # This file
└── data/                          # Input folder (create this manually)
    ├── tile_001.tif
    ├── tile_002.tif
    └── ...
```

## Workflow Steps

### 1. **Setup & Data Preparation**
   - Install dependencies (handled automatically in notebook)
   - Place all input `.tif` files in a `data` folder in the project directory

### 2. **File Discovery & Validation** (Cells 1-3)
   - Scans the `data` folder for all GeoTIFF files
   - Creates a DataFrame listing all discovered tiles
   - Filters out hidden files and non-TIFF formats

### 3. **Metadata Inspection** (Cell 4)
   - Opens each tile and extracts CRS (Coordinate Reference System) and resolution
   - Validates consistency across all tiles
   - Reports any mismatches with warnings
   - Identifies reference CRS and resolution for resampling

### 4. **Intelligent Resampling** (Cell 5)
   - Resamples all tiles to a common grid using **cubic interpolation** for smooth output
   - Normalizes coordinate systems
   - Handles resolution mismatches automatically
   - Sets nodata pixels to 0 (black)

### 5. **Mosaic Creation** (Cell 6)
   - Merges all resampled tiles into a single raster
   - Uses "first" method for overlapping areas (prioritizes first tile in sequence)
   - Updates metadata with new dimensions and geotransform

### 6. **Output Generation** (Cell 7)
   - Exports the final mosaic as `cloudless_mosaic.tif`
   - Applies LZW compression for efficient file storage
   - Maintains all geospatial metadata (CRS, geotransform, etc.)
   - Reports output file size

### 7. **Verification & Visualization** (Cell 8)
   - Opens the final mosaic and verifies all metadata
   - Displays the complete mosaic with proper georeferencing
   - Confirms visual consistency and absence of seams

## Quick Start Guide

1. **Prepare Your Data**
   ```
   Create a folder named 'data' in the project directory
   Place all .tif files in this folder
   ```

2. **Run the Notebook**
   - Open `raster_tile_mosaicing.ipynb` in Jupyter Notebook or JupyterLab
   - Run cells sequentially from top to bottom (or use "Run All" for full execution)
   - Monitor console output for validation results and warnings

3. **Access the Output**
   - The file `cloudless_mosaic.tif` will be generated in the project root directory
   - Use any GIS software (ArcGIS, QGIS) or Python libraries (Rasterio, Rioxarray) to further process

## Output Specifications
The generated `cloudless_mosaic.tif` includes:
- **Driver:** GeoTIFF (GTiff)
- **Compression:** LZW (lossless)
- **CRS:** Inherited from input tiles
- **Resolution:** Unified across all bands
- **Metadata:** Complete geospatial georeferencing preserved

## Troubleshooting

| Issue | Solution |
|-------|----------|
| "No such file" error | Verify the `data` folder exists and contains `.tif` files with correct extension |
| Module not found error | Run `pip install` commands in first notebook cells or use requirements installation above |
| CRS/Resolution warnings | These are expected if tiles come from different sources; resampling handles this automatically |
| Large output file size | File size depends on tile resolution and area covered; LZW compression is already applied |
| Memory errors on large datasets | Process tiles in batches or increase available system memory |

## Performance Notes
- Processing time depends on tile count, resolution, and system resources
- Cubic resampling produces higher-quality output but takes slightly longer than nearest-neighbor
- LZW compression reduces file size by 40-60% without data loss

## Example Workflow Output
```
Inspecting Tile Metadata...
--------------------------------------------------
File: tile_001.tif | CRS: EPSG:4326 | Res: (-0.0001, 0.0001)
File: tile_002.tif | CRS: EPSG:4326 | Res: (-0.0001, 0.0001)
--------------------------------------------------
Validation Complete. All files will be mosaiced.
Reference CRS: EPSG:4326
Reference Resolution: (-0.0001, 0.0001)

Mosaic created. Shape: (3, 15000, 12000)
Output metadata updated successfully.

Successfully saved: cloudless_mosaic.tif
File size: 45.23 MB

--- Final Output Metadata Verification ---
CRS: EPSG:4326
Resolution: (-0.0001, 0.0001)
Bands: 3
Width: 12000, Height: 15000
------------------------------------------
```

## Advanced Options
To customize the workflow, edit these parameters in the notebook:
- **Resampling Method:** Change `Resampling.cubic` to `Resampling.nearest`, `Resampling.bilinear`, etc.
- **No-Data Value:** Modify `nodata=0` to match your data convention
- **Compression:** Change `"compress": "lzw"` to `"deflate"`, `"packbits"`, or `None`
- **Output Name:** Edit `OUTPUT_FILENAME` to customize the output file path

## References
- [Rasterio Documentation](https://rasterio.readthedocs.io/)
- [GDAL/OGR Geospatial Data Format Specifications](https://gdal.org/)
- [GeoTIFF Specification](http://geotiff.maptools.org/)

## License & Attribution
This project uses open-source geospatial libraries. Ensure compliance with respective licenses when redistributing.
