# Goal: Intersect the land cover and the potential erosion data
The challenge is double. On the one hand the potential erosion raster data set has an attribute table that makes processing of the data tricky. On the other hand, the landcover dataset has 10 m resolution making its processing in the virtual machine complicated.

## Getting the classes of interest from the potential erosion data
We are interested in the classes: 6,7,8 and 9. 
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
