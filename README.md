# Mejora en la retención de usuarios: Model Fitness
__Predicción de churn, agrupación de clientes mediante clustering y recomendaciones de marketing para gym__

<image src="https://github.com/BastianLQ/Gym-prediccion-churn-y-agrupacion-clustering/blob/main/Images/banner.png" alt="banner">

_Para ver desarrollo en código hacer click [aquí](https://portfoliodabastianlopez.on.drv.tw/Portafolio/P13.html)_

## Descripción del Proyecto
La cadena de gimnasios Model Fitness está desarrollando una estrategia de interacción con clientes basada en datos analíticos.

Uno de los problemas más comunes que enfrentan los gimnasios y otros servicios es la pérdida de clientes. ¿Cómo se descubre si un/a cliente ya no está en el gimnasio? Se puede calcular la pérdida en función de las personas que se deshacen de sus cuentas o no renuevan sus contratos. Sin embargo, a veces no es obvio que un/a cliente se haya ido: puede que se vaya de puntillas.

Los indicadores de pérdida varían de un campo a otro. Si un usuario o una usuaria compra en una tienda en línea con poca frecuencia, pero con regularidad, no se puede decir que ha huido. Pero si durante dos semanas no ha abierto un canal que se actualiza a diario, es motivo de preocupación: es posible que el seguidor o seguidor/a se haya aburrido y haya hecho abandono.

En el caso de un gimnasio, tiene sentido decir que un/a cliente se ha ido si no viene durante un mes. Por supuesto, es posible que estén en Cancún y retomen sus visitas cuando regresen, pero ese no es un caso típico. Por lo general, si un/a cliente se une, viene varias veces y luego desaparece, es poco probable que regrese.

Con el fin de combatir la cancelación, Model Fitness ha digitalizado varios de sus perfiles de clientes. La tarea consistió en analizarlos y elaborar una estrategia de retención de clientes.
  
## Herramientas Utilizadas
- __Lenguaje de Programación:__ Python.
- __Entorno de Desarrollo:__ Jupyter Notebook.
- __Bibliotecas:__ Pandas, Matplotlib, Seaborn, Scikit-learn, Scipy.

## Proceso del Proyecto
Tomando en cuenta los requerimientos, el proyecto de dividirá en cinco pasos:

__Importar librerías y datos.__

- Importar correctamente todas las librerías necesarias para completar el proyecto.
- Importar correctamente el conjunto de datos proporcionado por la cadena de gimnasios.

__Llevar a cabo el análisis exploratorio de datos (EDA).__

- Revisar en dataset buscando valores ausentes y duplicados, y estudiar los valores promedio y la desviación estándar.
- Observar los valores medios de las características en dos grupos: para las personas que se fueron (cancelación) y para las que se quedaron.
- Trazar histogramas de barras y distribuciones de características para aquellas personas que se fueron (cancelación) y para las que se quedaron.
- Crear una matriz de correlación y visualizarla.

__Construir un modelo para predecir la cancelación de usuarios.__

Se creará un modelo de clasificación binaria para clientes donde la característica objetivo es la marcha del usuario o la usuaria el mes siguiente, para esto se seguirán los siguientes pasos:

- Dividir los datos en conjuntos de entrenamiento y validación.
- Entrenar el modelo en el set de entrenamiento con dos métodos:
    - Regresión logística;
    - Bosque aleatorio.
- Evaluar la exactitud, precisión y recall para ambos modelos utilizando los datos de validación. Las métricas se usarán para determinar qué modelo funciona mejor.


__Crear clústeres de usuarios/as.__

Se dejará de lado la columna con datos sobre la cancelación para identificar los clústeres de objetos (usuarios/as) para esto, se realizarán las siguientes acciones:

- Estandarizar los datos.
- Crear una matriz de distancias basada en la matriz de características estandarizada y trazar un dendrograma.
- Entrenar el modelo de clustering con el algortimo K-means y predecir los clústeres de clientes. 
- Agrupar por cluster y analizar los valores medios de las características y qué grupos son más propensos a irse.
- Trazar distribuciones de características significativas para los clústeres. 

## Descubrimientos importantes
### Parte 1: Análisis exploratorio de datos

- Características promedio de quienes siguen siendo clientes y de quienes terminaron dándose de baja:

| Cluster | gender |	Near_Location	Partner |	Promo_friends	Phone |	Contract_period	Group_visits |	Age	Avg_additional_charges_total |	Month_to_end_contract	| Lifetime	| Avg_class_frequency_total |	Avg_class_frequency_current_month |
|---------|--------|------------------------|---------------------|------------------------------|-----------------------------------|------------------------|---------------|---------------------------|-----------------------------------|
| 0 |	0.510037 |	0.873086 |	0.534195 |	0.353522 |	0.903709 |	5.747193 |	0.464103 |	29.976523 |	158.445715 |	5.283089 |	4.711807 |	2.024876 |	2.027882 |
| 1 |	0.510839 |	0.768143 |	0.355325 |	0.183789 |	0.902922 |	1.728558 |	0.268615 |	26.989632 |	115.082899 |	1.662582 |	0.990575 |	1.474995 |	1.044546 |

Si se observan los valores medios de esta tabla, se pueden obtener algunas nociones de los factores que podrían incidir en la cancelación, por ejemplo en columnas como `Lifetime` la diferencia es muy grande e indica que __quienes cancelan, llevan, en promedio, menos tiempo de vida en el gimnasio que quienes siguen siendo clientes__. También hay columnas que parecen no tener influencia alguna, como `gender` y `Phone` que se mantienen iguales en ambas filas.

- Se analiza la distribución de las columnas y se encuentran muchas características con distribuciones disparejas, por lo que __al momento de entrenar los modelos, se deberá estandarizar los datos__.

- Matriz de correlaciones
  
<image src="https://github.com/BastianLQ/Gym-prediccion-churn-y-agrupacion-clustering/blob/main/Images/output_25_0.png" alt="banner">

Hay dos correlaciones muy altas que son: 
- "Cantidad de visitas semanales promedio históricamente" y "Cantidad de visitas promedio semanales en el mes presente", esta correlación es lógica y es muy esperable.
- "Meses faltantes para el término del contrato" y "Periodo de contrato", tambien puede ser lógica, ya que la gente que contrata periodos más largos, generalmente, tiene mucho más tiempo por delante que la gente que contrata periodos cortos.

Se observan también correlaciones negativas interesantes en la columna `Churn` en varias columnas, como es el caso de `Lifetime` y `Age` y `Avg_class_frecuency_current_month`, lo que indicaría que mientras más bajas estas columnas, habría más `Churn`. Con estas observaciones concluye el análisis exploratorio y pasamos a la construcción de los modelos.



## Ejecuta el proyecto [aquí](https://portfoliodabastianlopez.on.drv.tw/Portafolio/P13.html)
Para ver el diccionario de datos, el desarrollo completo en código, todos los gráficos y las conclusiones, haga click en el enlace de arriba.
