# Space AG Data Analyst Challenge

SpaceAG es una empresa que da herramientas para ayudar al agricultor a tomar mejores decisiones. Dentro de las herramientas usadas están los datos adquiridos de las imágenes de satélite, entre ellos el NDVI.

## NDVI

El NDVI es un índice de vegetación que mide el vigor de las plantas, es decir, una medición indirecta de la salud de la planta. Los valores de este índice van de -1 a 1.  A mayor valor de NDVI en un determinado pixel de la imagen entonces se espera una mayor biomasa del cultivo, es decir un cultivo más sano. A menores valores de NDVI se espera una menor biomasa o un área de vegetación o cultivo enferma. Asimismo, los valores de vegetación están entre 0.2 a 1 de NDVI. Así también, el suelo generalmente tienen valores negativos de NDVI hasta valores de 0.2 en este índice. Como información adicional, la presencia de nubes sobre el área evaluar afecta el promedio del NDVI.

Algunos agricultores de paltos piensan que puede existir una relación entre el NDVI con el rendimiento (Kg/Ha) del cultivo.

## El desafío

En este reto vamos a explorar el ndvi de un campo. Estos son los objetivos a lograr:

1.  Extraer las bandas de satélite de Sentinel 2 usando Sentinel Hub para la lista de fechas disponibles que te brindamos en [SentinelHubImage-available_dates.xlsx](SentinelHubImage-available_dates.xlsx).
Puedes encontrar más información de como lograr esto [aquí](https://sentinelhub-py.readthedocs.io/en/latest/examples/ogc_request.html).
2.  Extraer los valores de NDVI promedio para cada lote para cada una de las fechas. __Cada lote es un feature del [geojson adjunto](farm_map.json) en el repositorio.__
3.  Guardar las imágenes de NDVI de cada lote y cada fecha.
4.  Hacer un análisis de los datos obtenidos para obtener insights de cómo el NDVI se comporta en el tiempo para los diferentes lotes.

__Tienes 72h para completar este desafío. Te sugerimos usar Colab o Jupyter Notebook para este desafío.__

### Dudas o preguntas ?

Si tienes alguna duda o pregunta escribe a adolfo@spaceag.co

__Mucha suerte :)__

## Respuestas

1. Obtención de NVDI

La obtención del NVDI se da en el Notebook data_wrangling. Este tiene dos outputs principalmente:
- df_data.csv: Tabla con el detalle de cada lote y sus NDVIs. Por cada fecha del excel SentinelHubImage-available_dates.xlsx se genera una columna con el detalle del NVDI y otra con el promedio.
- images: Imágenes en .png del NDVI por cada lote y por cada fecha. Estas se encuentran en la ruta "outputs\images" y tienen como nombre el formato "fecha_id de lote"

2. Análisis de resultados

El análisis de los resultados se da en el Notebook data_analysis. Outputs:
- correlation_analysis.csv: gráfico de correlación entre variables estadísticas del NDVI y la producción de los lotes
- df_corr.png: correlación entre variables estadísticas del NDVI y la producción de los lotes
- time_series_analysis.png: gráfico de series de tiempo de NDVI por lote coloreado por nivel de productividad del lote
- time_series_grouped_analysis.png: gráfico que muestran las series de tiempo de NDVI agrupado en lotes con NDVI promedio mayor a 0.4 y menor igual a 0.4
- plot_month_of_year.png: gráfico del nivel de NDVI agrupado según nivel y mostrado por mes del año
- decomposition.png: gráfico con el resultados de decomposición de las series de tiempo de NDVI mensual agrupadas por lote 
- decomposition_residual_distribution.png: gráfico que muestra la distribución del residual de la decomposición
- moving_average: gráfico que muestra el promedio móvil (cada 6 periodos) del NDVI por serie de tiempo agrupada
- df_monthly: dataset mensual con resultados de decomposición y promedio móvil

# Propuestas de mejora:

Data wrangling

- La obtención de NDVI puede ser más rápida si se utiliza un rango de tiempo en el parámetro "time" del request con las fechas mínimas y máximas del excel SentinelHubImage-available_dates.xlsx.
Luego habría que quedarse solo con las imágenes que se hayan dado en las fechas especificadas en el mismo archivo.

- El cálculo del promedio de NVDI no toma en cuenta valores menores a 0.2 debido a que estos valores no pertenecen a un campo de cultivo. Sin embargo, hay campos en los que solo unos poco píxeles pasan este 
límite. Una variable que se podría tomar en cuenta es cuántos de los píxeles pasan el 0.2 en un determinado lote/fecha para poder tener un mejor entendimiento del NDVI promedio. Incluso ignorar el promedio
de una observación si muy pocos píxeles pasan el 0.2. Otra variables podría ser la desviación entándar de los valores de los pixeles.

- Para entender mejor por qué ciertos campos de cultivo tienen valores menores a 0.2 para todos sus pixeles sería importante conocer en qué fechas estuvieron activos, así se podría reconocer si los valores
bajos son debido a que el campo no contaba con vegetación o si tiene que ver con la presencia de nubes.

Data analysis

- Para agrupar los lotes y analizar las series de tiempo se podrían clusterizar los lotes y no solo dividirlos según el nivel promedio de NDVI para encontrar más series de tiempo con distintos patrones.
En este ejercicio podría no ser un cambio muy significativo, pero si se agregan lotes de otras regiones, por ejemplo, puede ser muy útil.

- Se puede desarrollar un algoritmo de forecast para hallar los valores de NDVI para las series de tiempo agrupadas en los meses faltante.
