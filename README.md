### Road Low Point Analysis
__________________________________________
Author: Noah Hallisey

Date: 5/15/2026

This README file contains code to conduct the CHAMP road Low Point Analysis (LPA). Road low points represent the critical, highly vulnerable, and first to flood locations from heavy rain or storm surge. The code below used to identify the lowest road elevation between intersections which can be run against hydrodyanmic storm models, such as ADCIRC, to identify roads likely to flood prior to a storm's landfall. 

Note: low points identified are not guaranteed to flood.

## Data Required:
# Rhode Island Geographic Information System (RIGIS) E-911 Road Centerlines Datatset
# U.S. Geological Survey (USGS) 3D Elevation Program (DEP) 1-meter bare-earth Digitial Elevation Model

## Packages Required:
* Arcpy

## ARCGIS Pro Tools Used:
* Multitpart to Single part ()
* Merge Divided Roads ()
* Feature Vertices to Points ()
* Split Line at Point ()
* Generate Points Along Lines ()
* Extract Value To Points ()
* Statistics ()

## Steps:

# import arcpy
import arcpy
from arcpy import env
form arcpy.sa import *

# Set Environment and Local Variables
arcpy.env.workspace = ""

#Read in roads dataset and DEM

# Local Variables
Roads = ""
DEM = ""

### Step 1: Think road dataset by selecting and extracting only major roads (local, state, highway, etc.)
arcpy.management.SelectLayerByAttribute(Roads, "NEW_SELECTION", where_clause="ST_Class in (3, 4, 20, 30, 40, 50, 52, 59, 91, 92)")

#Export selected features to a new feature class 
arcpy.management.CopyFeatures(Roads, 'roads_rd')

### Step 2: Convert roads dataset from multipart to single part - this is required to merge divided roads (in the next step)
arcpy.management.MultipartToSinglepart('roads_rd', 'roads_sp')

### Step 3: Merge divided roads 
arcpy.cartography.MergeDividedRoads('roads_sp', merge_field = "ST_CLASS", merge_distance = "100 Feet", out_features= "roads_merge")

### Step 4: Split lines at Intersections Using the Feature to Line tool
arcpy.management.FeatureVerticesToPoints(in_features='roads_merge', out_feature_class='roads_vertices', point_location="BOTH_ENDS")

### Step 5: Split lines at vertices
arcpy.management.SplitLineAtPoint(in_features="roads_merge", point_features="roads_vertices", out_feature_class='roads_split')

### Step 6: Thin road network again to reove roads shorter than 500 feet in length
roads_reduce = arcpy.management.SelectLayerByAttribute(in_layer_or_view='roads_split', selection_type = "NEW_SELECTION", where_clause = '"Shape_Length" >= 500')

###Export selected features to a new feature class (Copy Features)
arcpy.management.CopyFeatures(roads_reduce, out_feature_class= "roads_G500")

### Step 8: Generate a field to input a unique ID for each road segment
arcpy.management.AddField(in_table="roads_G500", field_name="ROAD_ID", field_type="FLOAT")

### Python expression for generating sequential ist of numbers used as the unique ID
expression = "autoIncrement()"
code_block = """
rec = 0
start = 1
interval = 1

def autoIncrement():
    global rec
    if rec == 0:
        rec = start
    else:
        rec += interval
    return rec
"""

### Step 9: Input sequential number list from step above in the empty field 
arcpy.management.CalculateField(in_table="roads_split", field="ROAD_ID", expression=expression, expression_type="PYTHON3", code_block=code_block)

### Step 10: Select the lowest value (Select By Attribute)

### Step 11: Extract the low point to a new feature class (Copy Features)
