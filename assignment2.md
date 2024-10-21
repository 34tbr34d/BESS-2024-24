# Exclusion of solar panel areas in Segovia
1. Exporting the selected feature of Segovia as a geopackage
2. Clipping solar exclusion with Segovia province (input layer is info, output layer was zone)

```processing.run("native:clip", {'INPUT':'C:/Users/localuser/Documents/GIS data/enre_cyl_excl_foto.gpkg|layername=enre_cyl_excl_foto','OVERLAY':'C:\\Users\\localuser\\Documents\\GIS data\\segovia_province.gpkg|layername=prov_cyl_recintos','OUTPUT':'ogr:dbname=\'C:/Users/localuser/Documents/GIS data/solarpanelsseogiva.gpkg\' table="solarpanelssegovia" (geom)'})```

3. Dissolve the solar exclusion in Segovia
```processing.run("native:dissolve", {'INPUT':'C:/Users/localuser/Documents/GIS data/solarpanelsseogiva.gpkg|layername=solarpanelssegovia','FIELD':[],'SEPARATE_DISJOINT':False,'OUTPUT':'TEMPORARY_OUTPUT'})```

==> manualy made permanent with the name: `solar_exclusion_segovia_dissolve.gpkg`


Layers opened:
*Layer 1:*
- Study Area: Castilla y León
- Looking for: _Microtus arvalis_ distribution of occurances in Castilla y León provinces
- Processe(s): Clipping, Create points layer from table

```processing.run("native:createpointslayerfromtable", {'INPUT':'C:/Users/localuser/Documents/GIS data/Microtus_arvalis_5_1.csv','XFIELD':'decimalLongitude','YFIELD':'decimalLatitude','ZFIELD':'','MFIELD':'','TARGET_CRS':QgsCoordinateReferenceSystem('EPSG:4326'),'OUTPUT':'TEMPORARY_OUTPUT'})```

*Layer 2*
- S
