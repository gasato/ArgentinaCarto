# Automatización de Cartas

## Diseño Cartográfico
* Que elementos componen el mapa
  * Diseño de los componentes del mapa
  * Definir puntos de anclaje y líneas guía
  * Ubicación, absoluta y relativade, los elementos en el papel
  * Tamaño
  * Tipografías

## Infraestructura de Datos

* Capas de información
* Grilla Indice
  * Identificador de cada hoja
  * Límites exactos de la hoja
  * Nombre
  * Otros datos como localidad, provincia
  * Información auxiliar puesta en forma explícita

* Parámetros Cartográficos
  * Sistema de Referencia
  * Escala
  * Estilo de grilla de coordenadas
  * Ajustes (si son necesarios)
    * Angulo de rotación
    * Extensión geográfica (xmin, ymin, xmax, ymax)
    * Tamaño del map frame (en mm)
    * DPI

* Diccionarios 

Equivalencia entre clases y simbología.

* Paleta Estándar de Colores
  * Estándar de colores (CMYK, Pantone Matching System)
  * Look Up Table 
 
* Mapa de Ubicación
* Información Complementara
  * Copyrigth
  * Autores
  * Información cartográfica complementaria
    * Año de edición
    * Fecha de generación del producto digital
    * Texto correspondiente a la descripción del SRC y Datum
* Leyenda
* Tablas
* Diagramas complementarios



## Enlaces útiles
https://www.nypl.org/blog/2011/01/06/elements-cartography

## Pseudo código para la confección automática de mapas

Definir tamaño de página

Identificar la hoja y el tema a desplegar

Definir SRC

Definir Escala

Establecer alineación con el norte. Calcular la rotación del canvas.

Obtener Tamaño del área en función de la escala para ajustar y definir el tamaño del marco

(((x_max(item_variables('Mapa 1')['map_extent']) - x_min(item_variables('Mapa 1')['map_extent'])) * 1000) / 250000) + 20

Centrar el marco en la hoja

x = @layout_pagewidth / 2

y = 100


Rotar el mapa de ser necesario

Ubicar el marco. 

Desplegar la composición
* Orden de despliegue de las capas de información

# Automatización QGIS
## Observaciones
* Todos los algoritmos se guardan en el archivo de proyecto, y en el archivo de plantilla de mapas.
* Layoutitem se refiere a cada uno de los items que componen el mapa

### Pasar Variables al Layout

QgsExpressionContextUtils.setLayoutVariable(QgsProject.instance().layoutManager().layouts()[0], "jajaja", "Hello World")

### Un cacho de código

'''
'''

from qgis.core import *

lyrGrid250 = 'h250grid'
h250ID     = 23663

#cambiar al sistema de referencia correspondiente
crs = QgsCoordinateReferenceSystem(22183)
QgsProject.instance().setCrs(crs)

#empezamos filtrando las capa por el ID
layer = QgsProject.instance().mapLayersByName(lyrGrid250)[0]

#analisis del dataprovider
dp = layer.dataProvider()

#layer.selectByExpression('\"ID\" = %s' % (h250ID))

print("Layer source %s" % layer.source())
print("Layer count %s" % layer.featureCount())
print("Layer select count %s" % layer.selectedFeatureCount())

iface.actionZoomToSelected().trigger()
#iface.actionZoomToLayer()

layer.removeSelection()

#QgsExpressionContextUtils.setLayoutVariable(QgsProject.instance().layoutManager().layouts()[0], "jajaja", "Hello World")

#cambiamos el srs por la faja que corresponde

#hacemos zoom a la capa grid_250

'''
'''

### Obtener datos desde la base de datos cartoParam

https://gis.stackexchange.com/questions/388063/get-feature-value-to-use-in-print-composer-label

### Incluir textos en el mapa desde archivos de texto

Existen varias opciones que deben ser exploradas para incluir dentro del cuerpo de mapa textos que descriptivos.
 Uno de ellos es convertir el archivo de texto a formato SVG u HTML. Cada uno de estos formatos tienen su dificultad y facilidad 
 para ser incluídos en el map frame.
 
 https://github.com/ksss/text2svg
 
 https://text2svg.syrusdark.website/latest/
 
 https://gitlab.com/zebulon-1er/text2svg
 
### Problemas

function does not work in print layout (at least in 3.16). 

attribute(get_feature_by_id ('POLIGONAL_FNHIS2008A_8babdf78_50dc_480f_b811_0da927b7c9a5',1),'area')
