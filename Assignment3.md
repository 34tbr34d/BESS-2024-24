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
#### jnj


## Steps Taken
1. 

# **Second Spatial Analysis** - High Risk Areas in Terms of Economic and Envrionmental Losses
**Objective:** Identify areas in Segovia, Spain that have a high risk in terms of vole populations, focusing towards agricultural zones that are vulnerable to economic loss and environemntal loss (both due to crop damage), and regions with high erosion potential where soil degredation can amplify environmental and economic impacts. 

## Layers

### **Erosion Potential** 
  #### Descargas del Inventario Nacional de Erosion de Suelos (INES) en la Comunidad Autónoma de Castilla y León, Downloaded File Name: "Erosión potencial por niveles" (.tiff file) - https://www.miteco.gob.es/es/biodiversidad/temas/inventarios-nacionales/inventario-nacional-erosion-suelos/descarga_ines_castilla_leon.html 
##### - PDF with layer information found at - https://www.miteco.gob.es/content/dam/miteco/es/biodiversidad/servicios/banco-datos-naturaleza/1-ines/1memorias/libro_ines_40_segovia_tcm30-153960.pdf 
##### - Scale Resolution of 1:30,000
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

## Steps Taken
1) Working on Erosion Potential Layer
1.1) 
