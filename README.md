# Road Low Point Analysis
__________________________________________
Author: Noah Hallisey

Date: 1/7/2026

This repository contains instructions and Python code for conducting a road low point analysis using arcpy and ArcGIS Pro tools. The code using an adapted version of Peter Stempel's R script for identifying road low points. 

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


## Steps:

# Set Environmental and Local Variables
arcpy.env.workspace = ""
arcpy.env.overwriteOutput = True

# Local Variables
Roads = ""
DEM = ""

### Step 1: Generate Point At Vertices (Feature Vertices to Point)

### Step 2: Split line at vertices (Split Line At Points)

### Step 3: Dissolve road lines by road name (Dissolve)

### Step 4: Generate points along road lines (Generate Point Along LInes)

### Step 5: Extract elevation from DEM at points (Extract Value To Points)

### Step 6: Reduce points dataset to points at elevation between 5 and 35 feet above mean sea level (Select by Attribute)

### Step 7: Copy the selected points to a new feature layer (Copy Features)

### Step 8: Create a statistics table containing the minimum value for each road using road name as the unique ID

### Step 9: Join the statistics table to the point feature class

### Step 10: Select the lowest value (Select By Attribute)

### Step 11: Extract the low point to a new feature class (Copy Features)
