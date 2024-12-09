# Goal: Intersect the land cover and the potential erosion data
The challenge is double. On the one hand the potential erosion raster data set has an attribute table that makes processing of the data tricky. On the other hand, the landcover dataset has 10 m resolution making its processing in the virtual machine complicated.

## Getting the classes of interest from the potential erosion data
We are interested in the classes: 6, 7, 8 and 9. 
1. We load the attribute table of the raster in QGIS (that is the `.vat.dbf` file).
2. We copy the rows and paste them in a [google sheet](https://docs.google.com/spreadsheets/d/1JcVDjJSCd32rAbGbC0PCmVoMvgXTe34n1va9lTcQYiQ/edit?gid=0#gid=0)
3. We sort the data so we have the rows ordered by the column: `EroPot_pb`
4. With the help from [ChatGPT](https://chatgpt.com/share/67575650-22cc-8003-82c1-b4790f1c3d48) we build the expression for the raster calculator: We save the raster as: `EroPotNiveles_41_reclass.tiff`
```
("EroPotNiveles_41@1" = 66 OR "EroPotNiveles_41@1" = 78 OR "EroPotNiveles_41@1" = 90 OR "EroPotNiveles_41@1" = 178 OR 
"EroPotNiveles_41@1" = 186 OR "EroPotNiveles_41@1" = 194 OR "EroPotNiveles_41@1" = 210 OR "EroPotNiveles_41@1" = 231 OR 
"EroPotNiveles_41@1" = 259 OR "EroPotNiveles_41@1" = 64 OR "EroPotNiveles_41@1" = 77 OR "EroPotNiveles_41@1" = 91 OR 
"EroPotNiveles_41@1" = 175 OR "EroPotNiveles_41@1" = 189 OR "EroPotNiveles_41@1" = 195 OR "EroPotNiveles_41@1" = 211 OR 
"EroPotNiveles_41@1" = 232 OR "EroPotNiveles_41@1" = 261 OR "EroPotNiveles_41@1" = 88 OR "EroPotNiveles_41@1" = 92 OR 
"EroPotNiveles_41@1" = 98 OR "EroPotNiveles_41@1" = 185 OR "EroPotNiveles_41@1" = 187 OR "EroPotNiveles_41@1" = 198 OR 
"EroPotNiveles_41@1" = 215 OR "EroPotNiveles_41@1" = 225 OR "EroPotNiveles_41@1" = 257 OR "EroPotNiveles_41@1" = 80 OR 
"EroPotNiveles_41@1" = 86 OR "EroPotNiveles_41@1" = 96 OR "EroPotNiveles_41@1" = 181 OR "EroPotNiveles_41@1" = 188 OR 
"EroPotNiveles_41@1" = 190 OR "EroPotNiveles_41@1" = 214 OR "EroPotNiveles_41@1" = 241 OR "EroPotNiveles_41@1" = 256) * 1
```
5. We convert the resulting raster to type `int16` to have a less large raster
```
processing.run("gdal:translate", {'INPUT':'E_PotencialxNiveles41/EroPotNiveles_41_reclass.tiff','TARGET_CRS':None,'NODATA':None,'COPY_SUBDATASETS':False,'OPTIONS':'','EXTRA':'','DATA_TYPE':2,'OUTPUT':'E_PotencialxNiveles41/EroPotNiveles_41_reclass_int.tif'})
```

## Getting the masked landcover data
1. Limit of Castilla y León: `comu_cyl_recintos.gpkg` downladed from [IDECyL](https://idecyl.jcyl.es/geonetwork/srv/spa/catalog.search#/metadata/SPAGOBCYLCITDTSAULAR), CCBY4.
2. Masking landcover data with limits of CyL and changing resolution to 25m
```
processing.run("gdal:cliprasterbymasklayer", {'INPUT':'29T_20230101-20240101.tif','MASK':'comu_cyl_recintos.gpkg|layername=comu_cyl_recintos','SOURCE_CRS':None,'TARGET_CRS':QgsCoordinateReferenceSystem('EPSG:32629'),'TARGET_EXTENT':None,'NODATA':None,'ALPHA_BAND':False,'CROP_TO_CUTLINE':True,'KEEP_RESOLUTION':False,'SET_RESOLUTION':False,'X_RESOLUTION':25,'Y_RESOLUTION':25,'MULTITHREADING':False,'OPTIONS':'','DATA_TYPE':0,'EXTRA':'','OUTPUT':'29T_mask.tif'})
```
```
processing.run("gdal:cliprasterbymasklayer", {'INPUT':'30T_20230101-20240101.tif','MASK':'comu_cyl_recintos.gpkg|layername=comu_cyl_recintos','SOURCE_CRS':None,'TARGET_CRS':QgsCoordinateReferenceSystem('EPSG:32629'),'TARGET_EXTENT':None,'NODATA':None,'ALPHA_BAND':False,'CROP_TO_CUTLINE':True,'KEEP_RESOLUTION':False,'SET_RESOLUTION':False,'X_RESOLUTION':25,'Y_RESOLUTION':25,'MULTITHREADING':False,'OPTIONS':'','DATA_TYPE':0,'EXTRA':'','OUTPUT':'30T_mask.tif'})
```
3. Warp the rasters so the resolution is 500 meters
```
processing.run("gdal:warpreproject", {'INPUT':'/30T_mask.tif','SOURCE_CRS':None,'TARGET_CRS':QgsCoordinateReferenceSystem('EPSG:25830'),'RESAMPLING':0,'NODATA':None,'TARGET_RESOLUTION':500,'OPTIONS':'','DATA_TYPE':0,'TARGET_EXTENT':None,'TARGET_EXTENT_CRS':None,'MULTITHREADING':False,'EXTRA':'','OUTPUT':'/30T_mask_warp.tif'})
```
```
processing.run("gdal:warpreproject", {'INPUT':'29T_mask.tif','SOURCE_CRS':None,'TARGET_CRS':QgsCoordinateReferenceSystem('EPSG:25830'),'RESAMPLING':0,'NODATA':None,'TARGET_RESOLUTION':500,'OPTIONS':'','DATA_TYPE':0,'TARGET_EXTENT':None,'TARGET_EXTENT_CRS':None,'MULTITHREADING':False,'EXTRA':'','OUTPUT':'29T_mask_warp.tif'})
```
4. Merging the rasters
```
processing.run("gdal:merge", {'INPUT':['29T_mask_warp.tif','30T_mask_warp.tif'],'PCT':False,'SEPARATE':False,'NODATA_INPUT':None,'NODATA_OUTPUT':None,'OPTIONS':'','EXTRA':'','DATA_TYPE':2,'OUTPUT':'lc_merged.tif'})
```
