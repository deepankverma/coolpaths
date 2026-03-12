# CoolPaths

Street-scale PET mapping and microclimate-aware pedestrian routing from open data.

This repository contains a Google Colab notebook (`Coolpaths.ipynb`) for generating high-resolution PET (Physiologically Equivalent Temperature) maps and using them to compute cooler pedestrian routes from OpenStreetMap-based networks.

---

## Overview

The notebook creates a workflow that:

1. downloads buildings and paths from OpenStreetMap,
2. estimates building heights where missing,
3. exports canopy, NDVI, water mask, DEM, and DSM data from Google Earth Engine,
4. generates shadows, surface masks, DSM, and sky view factor,
5. retrieves meteorological inputs from the NASA POWER API,
6. computes irradiance, MRT, and PET rasters,
7. uses PET maps for routing analysis to compare shortest versus coolest routes. 

---

<p align="center">
  <a href="https://colab.research.google.com/github/deepankverma/coolpaths/blob/main/notebooks/Coolpaths.ipynb">
    <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab">
  </a>
</p>

<p align="center">
  <a href="https://coolpaths.netlify.app/"><b>Coolpaths Website</b></a>
</p>

## Methodology
PET quantifies the combined effect of solar and thermal radiation on humans and is key to assessing outdoor comfort. The pipeline automatically extracts building footprints, infers building heights, and computes shadows and sky-view factors. It uses Google Earth Engine to obtain tree canopy layers and other open-access satellite data products to compute vegetation, impervious surfaces, and water bodies, which help generate surface albedo maps. Solar irradiance, Mean Radiant Temperature (MRT), and PET maps are then created using meteorological inputs retrieved from the NASA Power API. The generated maps are then fed into an OSM-based routing engine to identify shorter, cooler paths.

<p align="center">
  <img src="images/Methodology.jpg" alt="Methodology image" width="700">
</p>

## PET generation process


<p align="center">
  <img src="images/PET_generation.jpg" alt="Methodology image" width="700">
</p>

## Coolpaths routing


<p align="center">
  <img src="images/Routing.jpg" alt="Methodology image" width="700">
</p>




## How to Use

This notebook is designed primarily for **Google Colab** because it uses:

- Google Drive mounting
- Earth Engine authentication in Colab
- Drive-based export and file copying

---

## Requirements

Each user must have:

- a **Google account**
- access to **Google Earth Engine**
- a **Google Cloud Project** linked to Earth Engine
- access to **Google Drive**
- Google Colab

The notebook installs the following Python packages:

- `pvlib`
- `osmnx`
- `rasterio`
- `geopandas`
- `pybdshadow`
- `earthengine-api`
- `numpy`
- `topocalc`
- `geemap`
- `ipyleaflet`
- `timezonefinder`
- `pythermalcomfort` 

---

## Google Earth Engine and Google Drive setup

To run this notebook with your own account, you must configure your own Earth Engine and Drive access.

### 1. Create or use a Google account

You need a Google account that you will use for:

- Earth Engine authentication
- Google Drive access
- Colab login

It is best to use the **same account** for all three.

---

### 2. Get access to Google Earth Engine

You must have Earth Engine enabled for your Google account.

If your account does not yet have Earth Engine access, request it through the Earth Engine signup process.

---

### 3. Create a Google Cloud Project

Earth Engine in Python requires a Cloud Project during initialization.

Create a Google Cloud Project and note its **Project ID**.

Example:

```python
my-coolpaths-project-123
```

### 4. Enable Earth Engine for that Cloud Project

Make sure your Cloud Project is configured for Earth Engine use.

You will need this project ID in the notebook when initializing Earth Engine.

### 5. Update the notebook with your own Earth Engine project

In the notebook, there is a line like this:

```python
ee.Authenticate()
ee.Initialize(project="ee-deepankverma1")
```
Replace it with your own Cloud Project ID:

```python
ee.Authenticate()
ee.Initialize(project="YOUR_CLOUD_PROJECT_ID")
```
## 6. Google Drive configuration

The notebook exports Earth Engine rasters to Google Drive and then copies them into the local working directory.

The current notebook uses:

```python
folder = "test_simcity"
prefix = "Berlin"
```
You should change these to your own values, for example:

```python
folder = "coolpaths_exports"
prefix = "MyStudyArea"
```

Where the folder is the name of the folder in your Google Drive where Earth Engine exports will be saved and the prefix is used in the filenames and output folders.
