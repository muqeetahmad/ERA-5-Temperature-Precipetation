# ERA-5 Temperature and Precipitation

ERA5-Land temperature and precipitation processing and visualization, plus Google Earth Engine NDVI extraction, developed for high-mountain basin climate analysis (Hunza/Karakoram and Solukhumbu/Dudh Koshi, Nepal).

## ⚠️ Status of this repo

This repo is a working/exploratory collection, not a single clean pipeline. It mixes:
- **Real ERA5-Land processing** (`code_era.ipynb`) — opens an actual downloaded ERA5-Land NetCDF file and computes genuine anomalies for the Solukhumbu Basin
- **Simulated/synthetic temperature series** (`Temperature_era.ipynb`, and part of `code_era.ipynb`) — built from an assumed base temperature, seasonal cycle, and literature-based warming rate rather than from real downloaded ERA5 values, but plotted with "ERA5-Land" in the title. These are illustrative/placeholder plots, not derived climate data.
- One unrelated cell running **XRF geochemistry PCA** on rock samples, which appears to be a leftover from a different analysis

If you plan to share or cite this repo, it's worth separating the real ERA5 analysis from the synthetic placeholder plots (and removing or relocating the XRF cell) so viewers don't mistake simulated output for actual reanalysis data.

## Contents

| Notebook | What's actually in it |
|---|---|
| `code_era.ipynb` | GEE NDVI extraction function; real ERA5-Land NetCDF clipping, anomaly, and time-series analysis for the Solukhumbu Basin, Nepal; synthetic temperature-anomaly plot for "Rikha Samba Glacier" |
| `Temperature_era.ipynb` | Synthetic ERA5-Land-style temperature anomaly plot (2002–2025) for "Rikha Samba Glacier"; unrelated XRF geochemistry PCA cell |

## Real ERA5-Land analysis (`code_era.ipynb`)

- Loads `era5_land_data_2000_2023.nc`, sets spatial dims and CRS (`EPSG:4326`)
- Clips to the **Solukhumbu Basin** (Nepal) shapefile
- Computes **temperature anomaly** (mean 2011–2023 minus mean 2000–2010, converted Kelvin → °C) and **precipitation anomaly** (annual totals, converted m → mm) as spatial maps
- Produces basin-averaged **monthly time series** of temperature and precipitation (2000–2023)
- Prints summary statistics: mean annual temperature, total precipitation, and anomaly ranges

## NDVI extraction (`code_era.ipynb`)

A helper function (`calculate_ndvi_gee`) that reads a study-area shapefile, pulls the least-cloudy Landsat 8 scene for a given year via Earth Engine, computes NDVI (`SR_B5`, `SR_B4`), and exports the result to Google Drive as a GeoTIFF.

## Synthetic temperature plots

Both notebooks include a block that generates a monthly temperature series from:
- A fixed base temperature (−12 °C, representative of a high-elevation glacierized site)
- A sinusoidal seasonal cycle
- An assumed linear warming trend (~0.45–0.5 °C/decade, cited as consistent with IPCC/regional Himalayan warming literature)
- Randomly generated interannual variability

...then computes monthly anomalies relative to climatology, a 12-month rolling mean, and a linear trend line (°C/decade). This is useful as a template/demo for anomaly-plot styling, but the underlying values are simulated, not measured.

## Data requirements

- `era5_land_data_2000_2023.nc` — ERA5-Land monthly/hourly NetCDF (temperature `t2m`, precipitation `tp`)
- Basin boundary shapefile (Solukhumbu/Dudh Koshi Basin)
- Optional glacier outline shapefile, for overlay on anomaly maps
- A study-area shapefile and Earth Engine project, for the NDVI extraction function

## Dependencies

`earthengine-api` (`ee`) · `geemap` · `xarray` · `rioxarray` · `netCDF4` · `geopandas` · `numpy` · `pandas` · `matplotlib` · `cartopy` · `seaborn` · `regionmask` · `contextily` · `pyproj` · `rasterio`

---

*Reanalysis climate-data component of a broader high-mountain Asia glacier–hydrology research workflow.*
