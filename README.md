# Pedalear Sin Fatigarse
Recursos para la estimación de energía de pedaleo, según el artículo Orellana, D., Pesántez, I. M., Tenemaza, P. P., & Sisalima, A. T. (2019). Pedalear sin fatigarse: análisis de infraestructura ciclística urbana basado en la energía del pedaleo. Documents d'anàlisi geogràfica, 65(2), 273-292. https://doi.org/10.5565/rev/dag.514

Las estimaciones se basan en una red vial extraida de OpenStreetMap y procesadas en ArcGIS 10.3 con los siguientes requerimientos: 
- OSM Editor Extension (https://github.com/Esri/arcgis-osm-editor)
- Network Analyst
- 3D Analyst (en caso que se requiera calcular las pendientes de la red vial)

En este repositorio se incluyen los siguientes recursos:

- Bikesheds.tbx: Caja de Herramientas para ArcGIS 9.3 con los pasos para la importación y preprocesamiento del archivo de OSM. Requiere la extensión Network Analysis

- Cycle Energy 3d.xml: Archivo de configuración para ArcGIS' Network Analysis. Estima impedancia en la red vial (segmentos de calle e intersecciones) en términos de energía necesaria para el pedaleo. Los factores que se toman en cuenta incluyen: el tipo de vía, la infraestructura ciclística, la superficie de rodadura, la pendiente, las intersecciones (señalización, semáforos, tipo de vías que llegan a la intersección, etc).

## Procesamiento

1.	Obtain the data

Either:

●	Use the Download OSM Data tool to downolad an specific area and create the network dataset in a file geodatabase.
●	If there is already a .osm file, it is possible to use the “load osm” tool to create the network dataset in a file geodatabase.
This will create a feature dataset containing three layers of OSM data: points, lines and polygons.


3. Split lines

Lines must be splitted to intersections and small segments (100m) to obtain accurate slope values.

- Use the Feature to Line tool to split the lines at the intersections.
  Further refinement for long lines can be done via manual editing or scripting.

4. Obtain elevation data and compute slopes

- Use the interpolate line from 3D analysis to create a 3D line feature feature class based on a digital elevation model.
- Compute two fields for slope (slope_ft and slope_tf) in each direction.
- Rename the original line feature class to *_bakup  and rename the 3D feature class to the same name as the original feature class. This will allow the script to use the 3d lines to create the network.

5. Create the network.

Use the “create OSM network” tool from OSM Editor toolbox and the “cycling energy XML configuration file” to create the network. The file will compute energy and power as network attributes based in the equation by wilson. Mandatory fields are:

- highway (highway type to determine non-traversable segments)
- Surface (to compute CR)
- Slope_ft and slope_tf

A toolbox model  is provided to perform steps 2 and 3.
