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

Características promedio de quienes siguen siendo clientes y de quienes terminaron dándose de baja:

| Cluster | gender |	Near_Location |	Partner |	Promo_friends	Phone |	Contract_period	Group_visits |	Age |	Avg_additional_charges_total |	Month_to_end_contract	| Lifetime	| Avg_class_frequency_total |	Avg_class_frequency_current_month |
|---------|--------|----------------|---------|---------------------|------------------------------|------|------------------------------|------------------------|-----------|---------------------------|-------------------------------------------|
| 0 |	0.510037 |	0.873086 |	0.534195 |	0.353522 |	0.903709 |	5.747193 |	0.464103 |	29.976523 |	158.445715 |	5.283089 |	4.711807 |	2.024876 |	2.027882 |
| 1 |	0.510839 |	0.768143 |	0.355325 |	0.183789 |	0.902922 |	1.728558 |	0.268615 |	26.989632 |	115.082899 |	1.662582 |	0.990575 |	1.474995 |	1.044546 |

Si se observan los valores medios de esta tabla, se pueden obtener algunas nociones de los factores que podrían incidir en la cancelación, por ejemplo en columnas como `Lifetime` la diferencia es muy grande e indica que __quienes cancelan, llevan, en promedio, menos tiempo de vida en el gimnasio que quienes siguen siendo clientes__. También hay columnas que parecen no tener influencia alguna, como `gender` y `Phone` que se mantienen iguales en ambas filas.

Se analiza la distribución de las columnas y se encuentran muchas características con distribuciones disparejas, por lo que __al momento de entrenar los modelos, se deberá estandarizar los datos__.

Una parte importante del análisis exploratorio es la matriz de correlaciones.
  
<image src="https://github.com/BastianLQ/Gym-prediccion-churn-y-agrupacion-clustering/blob/main/Images/output_25_0.png" alt="banner">

Hay dos correlaciones muy altas que son: 
- "Cantidad de visitas semanales promedio históricamente" y "Cantidad de visitas promedio semanales en el mes presente", esta correlación es lógica y es muy esperable.
- "Meses faltantes para el término del contrato" y "Periodo de contrato", tambien puede ser lógica, ya que la gente que contrata periodos más largos, generalmente, tiene mucho más tiempo por delante que la gente que contrata periodos cortos.

Se observan también correlaciones negativas interesantes en la columna `Churn` en varias columnas, como es el caso de `Lifetime` y `Age` y `Avg_class_frecuency_current_month`, lo que indicaría que mientras más bajas estas columnas, habría más `Churn`. Con estas observaciones concluye el análisis exploratorio y pasamos a la construcción de los modelos.

### Parte 2: Predecir la renuncia

Para entrenar los modelos, se dividen los datos en `X` e `y` (Los valores de característica y la variable objetivo, respectivamente), estos datos se han vuelto a dividir en una proporción de 80/20 para entrenamiento y prueba, respectivamente. Finalmente se estandarizan los datos numéricos de entrenamiento (los booleanos como 0 y 1 o la columna `Contract_period` no cuentan como numéricos).

Los modelos a entrenar y sus respectivos resultados, son los siguientes:
- Regresión Logística:
  - Exactitud: 92%.
  - Precisión: 82%.
  - Recall: 84% 
- RandomForestClassifier:
  - Exactitud: 92%.
  - Precisión: 84%.
  - Recall: 81%
 
__La regresión logística con datos estandarizados ha demostrado ser la más efectiva para predecir cancelaciones__ ya que tiene más alta la métrica `Recall` y nuestro objetivo de negocio hace preferible un falso positivo (Dar beneficios a alguien que no va a renunciar) a un falso negativo (No tomar acción sobre una renuncia inminente).

### Agrupación por rasgos clave

Para generar las agrupaciones usando clustering, primero, debemos determinar el número de grupos que se van a generar, para ello se construyó un dendrograma.

<image src="https://github.com/BastianLQ/Gym-prediccion-churn-y-agrupacion-clustering/blob/main/Images/output_39_0.png" alt="dendrograma">

Observando las ramas y los colores del gráfico, se determinó que se crearán __5 grupos__. Una vez creados los grupos se agrupan para visualizar una tabla con el promedio de las características.

| cluster | gender	| Near_Location |	Partner |	Promo_friends |	Phone |	Contract_period |	Group_visits |	Age	| Avg_additional_charges_total |	Month_to_end_contract |	Lifetime |	Avg_class_frequency_total |	Avg_class_frequency_current_month |	Churn |
|---------|---------|---------------|---------|---------------|-------|-----------------|--------------|------|------------------------------|------------------------|----------|----------------------------|---------------------------------------|-------|
| 0 |	0.502970 |	0.959406 |	0.783168 |	0.574257 |	1.000000 |	10.889109 |	0.542574 |	29.982178 |	160.761016 |	9.954455 |	4.736634 |	1.982055 |	1.974789	| 0.027723 |
| 1 |	0.522078 |	0.862338 |	0.470130 |	0.306494 |	0.000000 |	4.787013 |	0.425974 |	29.301299 |	143.957664 |	4.475325 |	3.924675 |	1.847220 |	1.716369	| 0.267532 |
| 2 |	0.495050 |	0.000000 |	0.463366 |	0.079208 |	1.000000 |	2.352475 |	0.215842 |	28.477228 |	135.457501 |	2.198020 |	2.809901 |	1.660461 |	1.477324	| 0.443564 |
| 3 |	0.485737 |	1.000000 |	0.350238 |	0.240095 |	1.000000 |	1.948494 |	0.341521 |	28.167987 |	131.622204 |	1.856577 |	2.440571 |	1.247634 |	1.012983 |	0.514263 |
| 4 |	0.559666 |	0.976134 |	0.356802 |	0.230310 |	0.998807 |	2.669451 |	0.473747 |	30.125298 |	161.657905 |	2.459427 |	4.898568 |	2.852002 |	2.850161 |	0.068019 |

Observando los clusteres en base a la cancelación, podemos separarlos en dos grupos:
- __0 y 4__: tienen una tasa de cancelación bajísima, por lo que, __para disminuir la tasa de cancelación, debemos hacer que el resto de los usuarios comparta sus características clave__, que serían:
  - Tener más de cuatro meses en el gym.
  - Pagar, en promedio, 160 dólares en cargos adicionales del gym (evidentemente no se trata de generar cobros porque si, sino incentivar al cliente a comprar productos del gimnasio).
  - Hacen actividades grupales.
  - Contratar el plan de 12 meses.
  - Trabajar en una compañía asociada. 
- __1, 2 y 3__: tienen una tasa de cancelación alta: tienen características que hay que trabajar, en la conclusión se detallarán.

Ahora se mostrarán algunas distribuciones de características importantes, para lo cual se utiliza una función con el fin de automatizar el proceso.

## Ejecuta el proyecto [aquí](https://portfoliodabastianlopez.on.drv.tw/Portafolio/P13.html)
Para ver el diccionario de datos, el desarrollo completo en código, todos los gráficos y las conclusiones, haga click en el enlace de arriba.
