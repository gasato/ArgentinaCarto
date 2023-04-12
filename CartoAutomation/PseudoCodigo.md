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

Rotar el mapa de ser necesario

Ubicar el marco y desplegar la composición

