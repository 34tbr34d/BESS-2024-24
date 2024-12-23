# **First Spatial Analysis** - Ideal Vole Locations
**Objective:** Identify areas where voles are likely to thrive due to proximity to water bodies and vegetation types in Segovia, Spain.

## Layers

### Hydrology
  #### Hidrografía de Castilla y León: masas de agua (download SHP) - https://datos.gob.es/es/catalogo/a07002862-hidrografia-de-castilla-y-leon-masas-de-agua
##### - This hydrology layer is polygonal data (vector) on water bodies within Castilla y Leon (rivers, lakes, reservoirs and ponds)
##### - Scale Resolution of 1:25,000
##### - Data published by the Junta de Castilla y Leon, specifically the autonomous administration working under environmental managemnet
##### - Licsensed under the IGCYL-NC license
##### - Dataset created and last updated in 2012
#### **Relevance** : This hydrology layer can be used when identifying areas where voles will thrive because water availability plays a role in shaping the vegetation distribution, and voles rely on this (and especially greener plants found closer to water bodies) for food and cover. Areas near water sources tend to support dense vegetation, in turn creating ideal habitats for voles. 


### Vegetation type
  ####  Castille and León crops and natural land map in 2022 (download .tif) - https://mcsncyl.itacyl.es/en/descarga 
##### - Data is raster layer of different vegetation types in Castilla y Leon, attribute table states specifically the crop types presented
##### - Scope of Castilla y Leon, data published by Junta de Castilla y Leon and Instituto Tecnologico Agario
##### - Scale Resolution of 1:50,000
##### - Created in 2022
#### **Relevance**: This vegetation layer is relevant because through filtering, it is possible to only map the vegetation that the Common vole prefers, therefore locating where the species could be drawn to, enabling the mapping of ideal habitat areas. 


## Steps Taken
1. Clip Raster by Mask Layer Vegetation map to Segovia Provincia (so that vegetation and hydrology is more localized and visible)

``` processing.run("gdal:cliprasterbymasklayer", {'INPUT':'C:/Users/localuser/Documents/GIS data/2022_Clasificacion_MCSNCyL/2022-10-07-MCSNCyL2022.tif','MASK':'C:/Users/localuser/Documents/GIS data/sg_province.gpkg|layername=prov_cyl_recintos','SOURCE_CRS':None,'TARGET_CRS':None,'TARGET_EXTENT':None,'NODATA':None,'ALPHA_BAND':False,'CROP_TO_CUTLINE':True,'KEEP_RESOLUTION':False,'SET_RESOLUTION':False,'X_RESOLUTION':None,'Y_RESOLUTION':None,'MULTITHREADING':False,'OPTIONS':'','DATA_TYPE':0,'EXTRA':'','OUTPUT':'C:/Users/localuser/Documents/GIS data/Vegetation_sg_province.tif'})```

**DIDNT WORK BECAUSE SYMBOLOGY WAS LOST, INPUTS WERE NO LONGER CORRELATED WITH VEGETATION VALUES** 

2. Trying to Clip Raster by Extent to perserve symbology

``` processing.run("gdal:cliprasterbyextent", {'INPUT':'C:/Users/localuser/Documents/GIS data/2022_Clasificacion_MCSNCyL/2022-10-07-MCSNCyL2022.tif','PROJWIN':'343063.391700000,486801.225500000,4496909.935100000,4608067.193200000 [EPSG:25830]','OVERCRS':False,'NODATA':None,'OPTIONS':'','DATA_TYPE':0,'EXTRA':'','OUTPUT':'C:/Users/localuser/Documents/GIS data/attempt_not_delete_symbology_veg_sg.tif'})```

**DIDNT WORK SYMBOLOGY WAS STILL LOST, ATTEMPTING TO NOT CLIP AND LEARN HOW TO BUFFER RASTER LAYER**

3. Trying to convert Raster to Vector (to be able to buffer hydrology)

   3.1. This conversion did not work and I encountered problems when loading my files, I later realized my hydrology layer was already a vectorr

4. Buffer Hydrology Vector Layer

   4.1. Create a 200m buffer zone around all hydrology areas in CyL

``` processing.run("native:buffer", {'INPUT':'C:/Users/localuser/Documents/GIS data/Cyl_masas_de_agua/hy.hidro_cyl_masas.shp','DISTANCE':200,'SEGMENTS':8,'END_CAP_STYLE':0,'JOIN_STYLE':0,'MITER_LIMIT':1,'DISSOLVE':False,'SEPARATE_DISJOINT':False,'OUTPUT':'C:/Users/localuser/Documents/GIS data/hydrology_CyL_200mBuffer.gpkg'})```
   
5. Filter Vegetation that voles are attracted to

   5.1. This vegetation mapped and filtered is: Wheat, Barely, Maize, Rye, Oats, Other cereals, Green peas, Vetches, Alfalfa, Other legumes
   
6. Attempting to create an overlap area of hydrology buffer and vegetation types - 

   6.1 Need to make hydrology and vegetation layer the same type of data - making hydrology vector into raster

``` processing.run("gdal:rasterize", {'INPUT':'C:/Users/localuser/Documents/GIS data/hydrology_CyL_200mBuffer.gpkg','FIELD':'','BURN':0,'USE_Z':False,'UNITS':0,'WIDTH':0,'HEIGHT':0,'EXTENT':None,'NODATA':0,'OPTIONS':'','DATA_TYPE':5,'INIT':None,'INVERT':False,'EXTRA':'','OUTPUT':'C:/Users/localuser/Documents/GIS data/attempt_hydrology_raster.tif'})```

**DID NOT WORK, NOT SURE WHY, NOW TRYING AGAIN BUT ADDING PIXEL SIZE OF 10,-10**

``` processing.run("gdal:rasterize", {'INPUT':'C:/Users/localuser/Documents/GIS data/hydrology_CyL_200mBuffer.gpkg','FIELD':'','BURN':0,'USE_Z':False,'UNITS':0,'WIDTH':10,'HEIGHT':0,'EXTENT':None,'NODATA':0,'OPTIONS':'','DATA_TYPE':5,'INIT':None,'INVERT':False,'EXTRA':'','OUTPUT':'C:/Users/localuser/Documents/GIS data/attempt_hydro_raster.tif'})```

**DID NOT WORK, ATTEMPTING CONVERTING RASTER TO VECTOR**

  6.2. Making Vegetation Raster into Vector to be able to execute an overlap area

``` processing.run("gdal:polygonize", {'INPUT':'C:/Users/localuser/Documents/GIS data/2022_Clasificacion_MCSNCyL/2022-10-07-MCSNCyL2022.tif','BAND':1,'FIELD':'DN','EIGHT_CONNECTEDNESS':False,'EXTRA':'','OUTPUT':'C:/Users/localuser/Documents/GIS data/attempt_CyL_veg_vector.gpkg'})```

**DID NOT WORK AND STATED "Process was unexpectedly terminated"**

7. Created cartography map

![Ideal Habitat for Common Vole based on Hydrology and Vegetation](Ideal_Habitat_for_Common_Vole_in_CyL.png)

This map is focused on the areas where the Common vole has an ideal habitat to thrive in based on hydrology and vegetation within Castilla y Leon. Within this map, hydrology buffers were created to highlight a surrounding of 200 meters around water masses where the common vole would succeed. The vegetation represents the main crops the Common vole is drawn to, highlighting where the common vole would be likely found. 

Ideally, I was attempting to create an overlapping layer where both vegetation and hydrology buffers stood. I also wanted this map to be focused only in Segovia so that the data would be clearly visualized. The viewer would idealy understand that these areas are likely infested with voles, and should be prioritized when implementing solutions. 


# **Second Spatial Analysis** - High Risk Areas in Terms of Economic and Envrionmental Losses
**Objective:** Identify areas in Segovia, Spain that have a high risk in terms of vole populations, focusing towards agricultural zones that are vulnerable to economic loss and environemntal loss (both due to crop damage), and regions with high erosion potential where soil degredation can amplify environmental and economic impacts. 

## Layers

### **Erosion Potential** 
  #### Descargas del Inventario Nacional de Erosion de Suelos (INES) en la Comunidad Autónoma de Castilla y León, Downloaded File Name: "Erosión potencial por niveles" (.tiff file) - https://www.miteco.gob.es/es/biodiversidad/temas/inventarios-nacionales/inventario-nacional-erosion-suelos/descarga_ines_castilla_leon.html 
##### - PDF with layer information found at - https://www.miteco.gob.es/content/dam/miteco/es/biodiversidad/servicios/banco-datos-naturaleza/1-ines/1memorias/libro_ines_40_segovia_tcm30-153960.pdf 
##### - Scale Resolution of 1:30,000
##### - Data of Castilla y Leon
##### - Data posted by the Instituto Geológico y Minero de España (IGME), under the National Soil Erosion Inventory (INES)
##### - The methodology used to generate the erosion potential was based on scientific research and historical data
##### - Dataset last updated in 2014
##### - Data is Raster file, values represent different erosion potential levels: 0-1 = null or very low, 2-3 = low or moderate, 4-5 = medium, 6-7 = high, 8-9-10 = very high
#### **Relevance**: This Erosion Potential layer is relevant and important because land erosion influences several environemntal factors such as plant growth, soil fertility and land productivity. Erosion weakens topsoil, which is where voles often feed and burrow. This behavior in voles would make areas with high erosion potential even more fragile and vulnerable. In turn, economic and environmental losses would be expected from this behavior. 


### **Land Cover**
  #### Esri Sentinel-2 Land Cover Explorer (.tif file, Raster data) - https://livingatlas.arcgis.com/landcoverexplorer/#mapCenter=-5.12978%2C40.99605%2C5.9632000000000005&mode=step&timeExtent=2017%2C2021&renderingRule=0&year=2022
##### - Dataset information foudn at https://www.arcgis.com/home/item.html?id=cfcb7609de5f478eb7666240902d4d3d 
##### - Dataset is available under a Creative Commons by Attribution license
##### - The data is the result of a collaboration between Esri and Impact Observatory
##### - Cell size is 10 meters
##### - Mapped Land Use of 2023, corresponding values are: 1= Water, 2= Trees, 4= Flooded Vegetation, 5= Crops, 7= Built Area, 8=Bare ground, 9= Snow/Ice, 10= No land information due to cloud cover, 11= Rangeland 
##### - Source Data Coordinate System: Universal Transverse Mercator (UTM) WGS84
##### - Service Coordinate System: Web Mercator Auxiliary Sphere WGS84 (EPSG:3857)
#### **Relevance**: Using this land cover data, filtering processes can be done to isolate crop land. This is relevant because where there are agricultural fields and crops, voles are more likely to have impactful damage. In these areas, the vole damage would cause economic losses and have environmental consequences. 

## Steps TakenWorking on Erosion Potential Layer

1. Working on Erosion Potential Layer
   
   1.1. Input Erosion Potential of CyL into QGIS
   
   1.2. Layer -> Properties -> Classify data
   
   1.3. Filter data: Deleted values 1,2,3,4 and 5 (only kept erosion potential values of high to very high (6,7,8,9,10))
   
2. Working on Land Cover Layer
   
   2.1. **Clip** Land Cover layer to Castilla y Leon
   
``` processing.run("gdal:cliprasterbymasklayer", {'INPUT':'C:/Users/localuser/Documents/GIS data/land_cover_data/29T_20230101-20240101.tif','MASK':'C:/Users/localuser/Documents/GIS data/prov_cyl_recintos.gpkg|layername=prov_cyl_recintos','SOURCE_CRS':None,'TARGET_CRS':None,'TARGET_EXTENT':None,'NODATA':None,'ALPHA_BAND':False,'CROP_TO_CUTLINE':True,'KEEP_RESOLUTION':False,'SET_RESOLUTION':False,'X_RESOLUTION':None,'Y_RESOLUTION':None,'MULTITHREADING':False,'OPTIONS':'','DATA_TYPE':0,'EXTRA':'','OUTPUT':'C:/Users/localuser/Documents/GIS data/29T_landcover_CyL.tif'}) ```

``` processing.run("gdal:cliprasterbymasklayer", {'INPUT':'C:/Users/localuser/Documents/GIS data/land_cover_data/30T_20230101-20240101.tif','MASK':'C:/Users/localuser/Documents/GIS data/prov_cyl_recintos.gpkg|layername=prov_cyl_recintos','SOURCE_CRS':None,'TARGET_CRS':None,'TARGET_EXTENT':None,'NODATA':None,'ALPHA_BAND':False,'CROP_TO_CUTLINE':True,'KEEP_RESOLUTION':False,'SET_RESOLUTION':False,'X_RESOLUTION':None,'Y_RESOLUTION':None,'MULTITHREADING':False,'OPTIONS':'','DATA_TYPE':0,'EXTRA':'','OUTPUT':'C:/Users/localuser/Documents/GIS data/30T_landcover_CyL.tif'}) ```


   2.2. Classify and Filter data: Remove values 1,2,4,7,8,9 and 10 (only keep value 5 of crop land)
  
3. Align Rasters using **Raster Projections- Warp (Reproject)**

Erosion Potential is ESPG: 25830 while Land Cover (30T and 29T) are ESPG: 32630

**Reprojecting Land Cover to be 25830**

```processing.run("gdal:warpreproject", {'INPUT':'C:/Users/localuser/Documents/GIS data/30T_landcover_CyL.tif','SOURCE_CRS':QgsCoordinateReferenceSystem('EPSG:25830'),'TARGET_CRS':None,'RESAMPLING':0,'NODATA':None,'TARGET_RESOLUTION':None,'OPTIONS':'','DATA_TYPE':0,'TARGET_EXTENT':None,'TARGET_EXTENT_CRS':None,'MULTITHREADING':False,'EXTRA':'','OUTPUT':'C:/Users/localuser/Documents/GIS data/30T_landcover_ESPG25830.tif'})```

```processing.run("gdal:warpreproject", {'INPUT':'C:/Users/localuser/Documents/GIS data/29T_landcover_CyL.tif','SOURCE_CRS':QgsCoordinateReferenceSystem('EPSG:25830'),'TARGET_CRS':None,'RESAMPLING':0,'NODATA':None,'TARGET_RESOLUTION':None,'OPTIONS':'','DATA_TYPE':0,'TARGET_EXTENT':None,'TARGET_EXTENT_CRS':None,'MULTITHREADING':False,'EXTRA':'','OUTPUT':'C:/Users/localuser/Documents/GIS data/29T_landcover_ESPG_25830.tif'})```

ERROR OCCURRING - 29T Land Cover data is displaced on map, doesnt align with the erosion potential coordinates and is no longer next to 30T

**MOVING FORWARD WITH ONLY 30T WHICH IS MAJORITY OF CYL, NEED ASSISTENCE TO FIND WHAT WENT WRONG AND THEN COMPLETE SAME UPCOMING STEPS WITH 29T**

4. Refilter Data - same as step 2.2, only keep value 5

5. Raster => Raster Calculator 

Goal is to intersect these layers, creating points on the map where the raster layers overlap 

  5.1. Formula was erosion raster * land cover raster 
          (1=high erosion, 0=no erosion), (1=crop land, 0=noncropland)

  5.2 Was hoping raster would be: 1= areas with both high erosion and crop land, 0 = other areas 

**DID NOT WORK - CREATED A FILE WITH NUMEROUS VALUES RANGING FROM 64 TO 2871? AREAS ARE SHADED AND NOT SPECIFICALLY OVERLAPPED?**

**ASSUMED ISSUE WAS BECAUSE DATA WAS NOT CLASSIFIED INTO BINARY?**

6. Reclassify data using Table - Erosion Potential Raster

Using classes:
1) 1 < x ≤ 5 → 0
2) 6 < x ≤ 10 → 1

``` processing.run("native:reclassifybytable", {'INPUT_RASTER':'C:/Users/localuser/Documents/GIS data/overlap_30T_EroPot_2.tif','RASTER_BAND':1,'TABLE':['1','5','0','6','10','1'],'NO_DATA':-9999,'RANGE_BOUNDARIES':0,'NODATA_FOR_MISSING':False,'DATA_TYPE':5,'OUTPUT':'C:/Users/localuser/Documents/GIS data/reclassified_EroPot.tif'}) ```

**DID NOT WORK?? SAME ISSUE, THIS RECLASSIFICATION CREATED THE SAME LAYER THE PREVIOUS STEP CREATED, ALONG WITH THE SAME BAND VALUES RANGING FROM 64 TO 2871?**

Ideally, if this mapping worked out, I would create a map showing high risk areas in Castilla y Leon. This areas would be taking into account erosion potential and the agricultural land use. The intented message the viewer would recieve would be that these areas should be prioritized when looking at the Common vole infestation because of their negative economic and enviornmental impacts. There would be a key showing the erosion potential values and colors. There would be no key necessary for land use because the layer of land use was already narrowed to only crop land.



