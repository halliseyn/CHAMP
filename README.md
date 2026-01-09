# Road Low Point Analysis
__________________________________________
Author: Noah Hallisey 
Date: 1/7/2026

This repository contains Python code for conducting a road low point analysis using arcpy and ArcGIS Pro tools.
The code using an adapted version of Peter Stempel's R script for identifying road low points. 

## Data Required:
* Road dataset (vector)
* Digitial Elevation Model (raster)

## Packages Required:
* Arcpy

## Tools Used:
* Feature Vertices To Points
* Spilt Line At Points
* Dissolve
* Generate Points Along Lines
* Extract Value To Points
* Statistics
* Join Field

### Step 1: Generate Point At Vertices


### Step 2: Split line at vertices

### Step 3: Dissolve road lines by road name

### Step 4: Generate points along road lines

### Step 5: Extract elevation from DEM at points

### Step 6: Reduce points dataset to points at elevation between 5 and 35 feet above mean sea level 

### Step 7: Copy the selected points to a new feature layer

### Step 8: Create a statistics table containing the minimum value for each road using road name as the unique ID

### Step 9: Join the statistics table to the point feature class

### Step 10: Select the lowest value 

### Step 11: Extract the low point to a new feature class 
