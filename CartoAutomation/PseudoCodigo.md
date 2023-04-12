# Automatización de Cartas

## Infraestructura de Datos

* Capas de información
* Grilla Indice
  * Límites exactos de la hoja
  * Identificador
  * Nombre
  * Otros datos como localidad, provincia
  * 
* Paleta Estándar de Colores
  * Look Up Table, diccionarios  
* Mapa de Ubicación
* Información Complementara
  * Copyrigth
  * Autores
  * Información cartográfica complementaria
    * Año de edición
    * Texto correspondiente a la descripción del SRC y Datum
* Leyenda
* Tablas
* Diagramas complementarios
* Estándares de Despliegue de la Información
  * Sistema de Referencia
  * Escala
  * Ajustes (si son necesarios)

* 
* Orden de despliegue de las capas de información

## Pseudo código para la confección automática de mapas

Definir tamaño de página

Definir SRC

Definir Escala

Obtener Tamaño del área en función de la escala para ajustar y definir el tamaño del marco

(((x_max(item_variables('Mapa 1')['map_extent']) - x_min(item_variables('Mapa 1')['map_extent'])) * 1000) / 250000) + 20

Centrar el marco en la hoja

x = @layout_pagewidth / 2

y = 100


Rotar el mapa de ser necesario

Ubicar el marco y desplegar la composición

# Automatización QGIS
## Observaciones
* Todos los algoritmos se guardan en el archivo de proyecto, y en el archivo de plantilla de mapas.
* Layoutitem se refiere a cada uno de los items que componen el mapa
### Obtener datos desde la base de datos cartoParam

https://gis.stackexchange.com/questions/388063/get-feature-value-to-use-in-print-composer-label

### Problemas

function does not work in print layout (at least in 3.16). 

attribute(get_feature_by_id ('POLIGONAL_FNHIS2008A_8babdf78_50dc_480f_b811_0da927b7c9a5',1),'area')
