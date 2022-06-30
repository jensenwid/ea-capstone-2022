[![DOI](https://zenodo.org/badge/481719777.svg)](https://zenodo.org/badge/latestdoi/481719777)

# urban-stormwater-capture
Final project repository for Earth Data Analytics 2022 CU Boulder course


# Project
Goal: Measure the role and impact of urban green infrastructure (GI) projects and low impact development (LID) on stormwater capture and discharge at both the site/project level and urban scale in the Los Angeles region.  

This repository will provide the analysis tools and datasets needed to measure impact of GI projects and other urban greenspace features. The major site area focuses on Los Angeles County, where recapture of stormwater is critically important to replenishing local groundwater resources. However, this same code and workflow could be used for other urban environments where project area shapefiles can be provided.  

WHAT IS A GREEN INFRASTRUCTURE PROJECT?  
Green infrastructure projects are geo-engineered features that help redirect and recapture stormwater runoff from urban hardscapes
Can be site-scale projects such as water retention ponds and bioswales, or urban-scale projects such as greenways and infiltration catchment areas to help:
- More effectively recapture groundwater resources
- Help flatten spikes in peak flow volumes after heavy precipitation
- Reduce pollutant discharge into local water systems


# Installation Instructions
Installation instructions to install Python and required Python packages can be found here:
https://www.earthdatascience.org/courses/intro-to-earth-data-science/python-code-fundamentals/use-python-packages/

## Running Jupyter Notebook
- Install Python, packages, and set up earth-analytics-python Conda environment
- Open shell and activate environment (conda activate earth-analytics-python)
- Navigate to directory using "$ cd "
- Open Jupyter Notebook with "$ jupyter notebook"
- Run code by selecting "Kernal", then "Restart and Run All"
  - Note: Code will only run to end of notebook with data sources downloaded; see below


Other supporting packages for import are listed at the top of the workflow and can be executed prior to the main code.

# Repository
- Name: urban-stormwater-capture
- Link: https://github.com/jensenwid/urban-stormwater-capture.git
- Files:
  - README
  - data: Folder containing supporting data files, including all files listed in Data section below EXCEPT for Spatial Data (Sentinel-2 HLS data) 
  - graphics: Folder containing images for example stormwater spreading grounds
  - LICENSE: Permissive license
  - ea-capstone-workbook-2022.ipynb: Working Jupyter Notebook for analysis
  - functions.ipynb: Exported functions for code used in main nb
  - environment.droplet.yml: env


# Data
The following data sources have been used in this analysis and can be followed to reproduce similar workflows.

### Spatial Data
- Sentinel-2 HLS (Harmonized Landsat Sentinel-2) HLS Landsat Operational Land Imager Surface Reflectance and TOA Brightness Daily Global 30m v2.0:
    - Data Request: https://search.earthdata.nasa.gov/search/granules?p=C2021957657-LPCLOUD&pg[0][v]=f&pg[0][gsk]=-start_date&q=sentinel-2%202a&tl=1650336873!3!!
    - Download Instructions:
      - Access data link above for NASA Earthdata search
      - Search for Sentinel-2A data and select collections matching name above
      - Subset location via polygon tool to Los Angeles area; select "11SLT" MGRS TILES ONLY
      - Subset date with date start and end date filters; this analysis uses data from Jan 2017 - May 2022
      - Download list of files, and click download script to download all requested files
     - About HLS: https://earthdata.nasa.gov/esds/harmonized-landsat-sentinel-2

### Shapefiles
- Sentinel-2 Military Grid Reference System (MGRS) Shapefiles: https://github.com/justinelliotmeyers/Sentinel-2-Shapefile-Index.git
- LA County Stormwater Spreading Grounds: https://data.lacounty.gov/Shape-Files/LA-County-Spreading-Grounds/36uw-n6yh

### Precipitation Data
- GPM IMERG Late Precipitation L3 1 day 0.1 degree x 0.1 degree V06 (GPM_3IMERGDL)
  - Data Request
    -  https://disc.gsfc.nasa.gov/datasets/GPM_3IMERGDL_06/summary?keywords=GPM%20imerg
  - Download Instructions:
    - Access data link above for NASA GES DISC
    - Subset location via polygon tool to Los Angeles area in Download Method;
    - Subset date with date start and end date filters; this analysis uses data from Jan 2017 - May 2022
    - Follow instructions to download list of files provided from subsetted list


### Supporting Data
- Los Angeles Sanitation Department Green Infrastructure Project List:
    - Link: https://data.lacity.org/w/pdbw-x3k8/ir6t-6fx6?cur=J_MxZ9Rog8-
    - API: https://data.lacity.org/api/views/pdbw-x3k8/rows.json?accessType=DOWNLOAD



# Workflow

## Pseudocode steps
The primary workflow is divided into four main steps: Analysis Prep, Core Analysis, YoY Time Series Analysis, and Urban Scale Impacts

### Analysis Prep
1. Download packages and Spatial Data, Shapefiles, Precipitation Data, and Supporting Data
2. Define pathways to datagroups above
3. Create functions necessary for streamlined workflow
 - Normalized difference moisture index (NDMI)
 - Opening cloud masked bands

### Perform Core Analysis for Single Frame of Site and Project Area
1. Select site area (LA Metro area; MGRS = T11SLT)  
2. Source pathway to proper Sentinel-2 data .tif files for location & date
3. Convert .tifs to rasters
4. Run cloud mask and NDMI function to determine moisture index over project and site areas
5. Overlay shapefiles for select GI and/or stormwater collection sites
    -Check that shapefiles and rasters are same CRS; convert if not
7. Calculate summary values (such as mean NDMI and percentiles)
8. Transform into dataframes and plot NDMI arrays

### YoY Time Series Analysis  
1. Perform Core Analysis steps for same site area, project area, seasonal range year-over-year (YoY)
- Loop through 2015 - 2022 Sentinel-2 downloaded datasets to extract similar date/time ranges YoY
  - Calculate mean NDMI and summary values for project sites and non-project sites
- Loop through 2015 - 2022 IMERG precipitation, filter coordinates to near project and site area, and transport summary precipitation values to dataframe
- Plot YoY Precipitation values and mean NDMI of project areas to measure how GI and/or greenspace capture stormwater
- Plot correlation between daily precipitation and daily peak streamflow values


## Challenges/Questions to Answer

- Challenge #1: Spatial resolution of 30m may still be too small to accurately measure smaller green infrastructure projects

- Challenge #2: How well does NDMI correspond with soil moisture? Will NDMI accurately reflect stormwater capture by showing growth in vegetation?
