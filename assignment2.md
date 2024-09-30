# Exclusion of solar panel areas in Segovia
1. Exposrting the selected feature of Segovia as a geopackage
2. Clipping solar exclusion with Segovia province (input layer is info, output layer was zone)

```processing.run("native:clip", {'INPUT':'C:/Users/localuser/Documents/GIS data/enre_cyl_excl_foto.gpkg|layername=enre_cyl_excl_foto','OVERLAY':'C:\\Users\\localuser\\Documents\\GIS data\\segovia_province.gpkg|layername=prov_cyl_recintos','OUTPUT':'ogr:dbname=\'C:/Users/localuser/Documents/GIS data/solarpanelsseogiva.gpkg\' table="solarpanelssegovia" (geom)'})```

3. Dissolve the solar exclusion in Segovia
```processing.run("native:dissolve", {'INPUT':'C:/Users/localuser/Documents/GIS data/solarpanelsseogiva.gpkg|layername=solarpanelssegovia','FIELD':[],'SEPARATE_DISJOINT':False,'OUTPUT':'TEMPORARY_OUTPUT'})```

==> manualy made permanent with the name: `solar_exclusion_segovia_dissolve.gpkg`
