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


# Installation instructions
Installation instructions to install Python and required Python packages can be found here: 
https://www.earthdatascience.org/courses/intro-to-earth-data-science/python-code-fundamentals/use-python-packages/

Other supporting packages for import are listed at the top of the workflow and can be exectuted prior to the main code. 

# Data 
The following data sources have been used in this analysis and can be followed to reproduce similar workflows. 

### Spatial Data
- Sentinel-2 HLS (Harmonized Landsat Sentinel-2) HLS Landsat Operational Land Imager Surface Reflectance and TOA Brightness Daily Global 30m v2.0: 
    - Data Request: https://search.earthdata.nasa.gov/search/granules?p=C2021957657-LPCLOUD&pg[0][v]=f&pg[0][gsk]=-start_date&q=sentinel-2%202a&tl=1650336873!3!!
    - About HLS: https://earthdata.nasa.gov/esds/harmonized-landsat-sentinel-2
    
### Shapefiles
- Sentinel-2 Military Grid Reference System (MGRS) Shapefiles: https://github.com/justinelliotmeyers/Sentinel-2-Shapefile-Index.git
- LA Streams and Rivers: https://data.lacounty.gov/GIS-Data/Streams-and-Rivers/6bsh-b6vg
- LA County Stormwater Spreading Grounds: https://data.lacounty.gov/Shape-Files/LA-County-Spreading-Grounds/36uw-n6yh
- GAGES (Geospatial Attributes of Gages for Evaluating Streamflow) II Gage Boundaries:  https://water.usgs.gov/GIS/metadata/usgswrd/XML/gagesII_Sept2011.xml#stdorder


### Supporting Data 
- Los Angeles Sanitation Department Green Infrastucture Project List: 
    - Link: https://data.lacity.org/w/pdbw-x3k8/ir6t-6fx6?cur=J_MxZ9Rog8-
    - API: https://data.lacity.org/api/views/pdbw-x3k8/rows.json?accessType=DOWNLOAD
- USGS National Water Dashboard Gage Data: https://dashboard.waterdata.usgs.gov/app/nwd/?region=lower48&aoi=default



# Workflow

## Pseudocode steps
The primary workflow is divided into four main steps: Analayis Prep, Core Analysis, YoY Time Series Analysis, and Urban Scale Impacts 

### Analysis Prep
1. Download packages and Spatial Data, Shapefiles, and Supporting Data 
2. Define pathways to datagroups above 
3. Create functions necessary for streamlined workflow 
 - Normalized difference moisture index (NDMI)
 - Normalized difference built-up index (NDBI)
 - Opening cloud masked bands 
 
### Core Analysis for Site and Project Areas
1. Select site area (LA Metro area; MGRS = T11SLT) and date 
2. Source pathway to proper Sentinel-2 data .tif files for location & date
3. Convert .tifs to rasters and test plot rasters to validate data is correct
4. Run NDMI function to determine moisture index over urban area
5. Overlay shapefiles for select GI and/or stormwater collection sites
    -Check that shapefiles and rasters are same CRS; convert if not 
6. Calculate mean NDMI of project areas and compare to non-project areas
7. Assign daily precipication values for chosen date
8. Determine applicable streamflow gages for poject area and assign daily peak flow value with GAGES II 

### YoY Time Series Analysis  
1. Perform Core Analysis steps for same site area, project area, seasonal range year-over-year (YoY) 
- Loop through 2015 - 2022 Sentinel-2 downloaded datasets to extract similar date/time ranges YoY
- Filter YoY ranges to time samples within 3 days of "significant" precipitation
- Calculate mean NDMI values for project sites and non-project sites 
- Assign daily max discharge values from associated streamflow gages for each project site
- Plot trends
    - Plot YoY preciptation values and mean NDVI of project areas to measure how GI and/or greenspace capture stormwater
    - Plot correlation between daily preciptation and daily peak streamflow values

### Urban Scale Impacts  
1. Run NDBI function for site area range to measure change in urban hardscape values across site area 
    - Use inverse of change in hardscape to determine change in "greenscape" to quantify GI and LID scalability 
    - Cross-reference inversely correlated areas with GI project shapefiles to validate methodology 
2. Compare change in NBDI from 2015 - 2022 with plots in YoY Time Series Analysis
    - Determine how change in NBDI correlates with precip and peak streamflow correlation in YoY analysis 

## Challenges/Questions to Answer 

- Challenge #1: Stream gage data is severely lacking in dense urban environments 
    -For example, the few active ongoing gage measurements are on the outer bounds of the LA metro site area of study.
    -Are these streamflows well enough correlated with the project areas in question? 
   
- Challenge #2: How well does using the inverse of NDBI measure changing greenspace within an urban environment?
    - Los Angeles instituted a low impact development (LID) ordinance in 2012 and targeted GI projects in it's 2015 and 2019 "Sustainability pLAn" efforts. These efforts were primarily focused on mitigating stormwater runoff and improving quality. Can we see the macro-scale impacts of this here? 

[![DOI](https://zenodo.org/badge/481719777.svg)](https://zenodo.org/badge/latestdoi/481719777)





