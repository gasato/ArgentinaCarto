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

```
from qgis.core import *

def get250kQuadrant(h250ID):
    '''
    Obtiene cuadrante. Se realiza de esta manera a fines
    de facilitar la lectura del código
    '''
    if len(str(h250ID)) != 5:
        raise ValueError ("Error código de hoja 250k, inválido %s" % (h250ID))
               
    return str(h250ID)[-1]
    
def mapAngle(quadrant):
    '''
    Identifica el ángulo de rotación para mapas 1:250k
    en función del número de cuadrante
    '''
    #chequeamos errores
    if type(quadrant) is str:
        quadrant = int(quadrant)
       
    if quadrant > 4 or quadrant < 1:
       print ("Error código de cuadrante inválido $s" % (quadrant))
       mAngleVal = None    
        
    #diccionario con equivalencias de cuadrantes vs ángulos
    mangleDict= {1:0.48214,2:-0.48214,3:0.48214, 4:-0.48214}
    
    mAngleVal = mangleDict[quadrant]
    
    print ("Angulo de rotación hoja 250.000 cuadrante %s  %s" % (quadrant, mAngleVal))
            
    return mAngleVal
    
h250ID = 37694

lyrGrid250  = 'h250grid' 
lyrGrid250M = 'h250gridMask'
lyrGeo250   = 'v_h250geounidades'

#Obtenemos layers 
layerGeo = QgsProject.instance().mapLayersByName(lyrGeo250)[0]
layerM   = QgsProject.instance().mapLayersByName(lyrGrid250M)[0]
layer    = QgsProject.instance().mapLayersByName(lyrGrid250)[0]

#cambiar al sistema de referencia correspondiente 
crs = QgsCoordinateReferenceSystem(layerGeo.sourceCrs()) 
QgsProject.instance().setCrs(crs)
print(".......Seteando CRS a: %s" % crs.description())

#alinear hoja 250k al norte
mAngle = mapAngle(get250kQuadrant(h250ID))
iface.mapCanvas().setRotation(mAngle)
print(".......Rotando Map Canvas %s" % (mAngle))

#analisis del dataprovider 
dp = layer.dataProvider()

print(".......Enmascarando área externa")
layerM.setSubsetString("ID !=%s" % h250ID)
print(".......Seleccionando y activando el area de la hoja")
layer.setSubsetString("ID=%s" % h250ID)
iface.setActiveLayer(layer) #activamos el layer


print("Layer source %s\n" % layer.source()) 
print("Layer count %s\n" % layer.featureCount()) 
print("Layer select count %s\n" % layer.selectedFeatureCount())

#iface.actionZoomToSelected().trigger() #tiene que estar enfocado

iface.actionZoomToLayer().trigger()

#obtener los valores de items
#iterFeature = layer.getSelectedFeatures()
iterFeature = layer.getFeatures()
itemFeature = next(iterFeature)

itemLst = ('xmin', 'xmax', 'ymin', 'ymax','width250kmm','heigth250kmm')

for i in itemLst:
    itemValue = itemFeature[i]
    print(i, itemValue)
    QgsExpressionContextUtils.setLayoutVariable(QgsProject.instance().layoutManager().layouts()[0], i, itemValue)
    
QgsExpressionContextUtils.setLayoutVariable(QgsProject.instance().layoutManager().layouts()[0], 'escala', 250000)
    


layer.removeSelection() #remover luego de la lectura de las variables



```

### Formateo de Grilla Posgar

```
concat(replace(left(@grid_number,2),
                array('1','2','3','4','5','6','7','8','9','0'),
                array('¹','²','³','⁴','⁵','⁶','⁷','⁸','⁹','⁰')
              ),
       substr(@grid_number,2,2) 
       )
```

### Obtener datos desde la base de datos cartoParam

https://gis.stackexchange.com/questions/388063/get-feature-value-to-use-in-print-composer-label

### Incluir textos en el mapa desde archivos de texto

Existen varias opciones que deben ser exploradas para incluir dentro del cuerpo de mapa textos que descriptivos.
 Uno de ellos es convertir el archivo de texto a formato SVG u HTML. Cada uno de estos formatos tienen su dificultad y facilidad 
 para ser incluídos en el map frame.
 
 https://github.com/ksss/text2svg
 
 https://text2svg.syrusdark.website/latest/
 
 https://gitlab.com/zebulon-1er/text2svg
 
 Si se opta por formato html, se puede hacer un script python incluyendo CSS.
 
### Problemas

function does not work in print layout (at least in 3.16). 

attribute(get_feature_by_id ('POLIGONAL_FNHIS2008A_8babdf78_50dc_480f_b811_0da927b7c9a5',1),'area')

## Otros

### Magnetic Declination Calculator
https://pypi.org/project/magnetic-field-calculator/
