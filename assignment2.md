# Exclusion of solar panel areas in Segovia
1. Exporting the selected feature of Segovia as a geopackage
2. Clipping solar exclusion with Segovia province (input layer is info, output layer was zone)

```processing.run("native:clip", {'INPUT':'C:/Users/localuser/Documents/GIS data/enre_cyl_excl_foto.gpkg|layername=enre_cyl_excl_foto','OVERLAY':'C:\\Users\\localuser\\Documents\\GIS data\\segovia_province.gpkg|layername=prov_cyl_recintos','OUTPUT':'ogr:dbname=\'C:/Users/localuser/Documents/GIS data/solarpanelsseogiva.gpkg\' table="solarpanelssegovia" (geom)'})```

3. Dissolve the solar exclusion in Segovia
```processing.run("native:dissolve", {'INPUT':'C:/Users/localuser/Documents/GIS data/solarpanelsseogiva.gpkg|layername=solarpanelssegovia','FIELD':[],'SEPARATE_DISJOINT':False,'OUTPUT':'TEMPORARY_OUTPUT'})```

==> manualy made permanent with the name: `solar_exclusion_segovia_dissolve.gpkg`

Layers opened:

*Layer 1*
- Study Area: Castilla y León
- Looking for: Cropland area percentage in Castilla y León in year 2000
- Process(s): Clipping of
- Layer data installed through: http://www.earthstat.org/cropland-pasture-area-2000/ 

```processing.run("gdal:cliprasterbymasklayer", {'INPUT':'C:/Users/localuser/Documents/GIS data/CroplandPastureArea2000_Geotiff/CroplandPastureArea2000_Geotiff/Cropland2000_5m.tif','MASK':'C:/Users/localuser/Documents/GIS data/prov_cyl_recintos.gpkg|layername=prov_cyl_recintos','SOURCE_CRS':None,'TARGET_CRS':None,'TARGET_EXTENT':None,'NODATA':None,'ALPHA_BAND':False,'CROP_TO_CUTLINE':True,'KEEP_RESOLUTION':False,'SET_RESOLUTION':False,'X_RESOLUTION':None,'Y_RESOLUTION':None,'MULTITHREADING':False,'OPTIONS':'','DATA_TYPE':0,'EXTRA':'','OUTPUT':'TEMPORARY_OUTPUT'})```


*Layer 2:*
- Study Area: Castilla y León
- Looking for: _Microtus arvalis_ distribution of occurances in Castilla y León provinces
- Processe(s): Clipping provinces, Create points layer from table

```processing.run("native:createpointslayerfromtable", {'INPUT':'C:/Users/localuser/Documents/GIS data/Microtus_arvalis_5_1.csv','XFIELD':'decimalLongitude','YFIELD':'decimalLatitude','ZFIELD':'','MFIELD':'','TARGET_CRS':QgsCoordinateReferenceSystem('EPSG:4326'),'OUTPUT':'TEMPORARY_OUTPUT'})```

*Layer 3* 
- 
