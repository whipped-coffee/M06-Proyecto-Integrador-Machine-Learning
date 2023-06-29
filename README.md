# **Proyecto Machine Learning**

Este proyecto fue realizado con el objetivo de poner en práctica las habilidades analíticas y simular el desarrollo de un modelo de predicción basado en Machine Learning. Realizamos este proyecto en un lapso de 4 días con un equipo de 4 integrantes: German Voss, Leonel Brizuela, Melina Arroyo y Gabriel Nuñez. 
Los datos fueron proporcionados por el equipo de Henry para trabajar sobre un escenario ficticio y no representan necesariamente a la realidad.  

# **Introducción**

Actualmente, el cáncer es uno de los principales problemas de salud y el principal causa de muerte a nivel mundial, de acuerdo con la Organización Mundial de la Salud (OMS), siendo el cáncer de próstata el cuarto más común.

Hoy en día, se utilizan pruebas de laboratorio, pruebas con imágenes y biopsias para la detección de esta enfermedad. Si hablamos del cáncer de próstata, los métodos más usados son: prueba de PSA, ultrasonidos y biopsias. Este último método, a veces genera complicaciones mayores que pueden poner en riesgo la vida del paciente, después de su procedimiento.

Por eso, este proyecto plantea el desarrollo de un sistema Machine Learning que nos permitirá emitir el diagnóstico del paciente, evaluar la efectividad de estrategias de intervención y anticipar comportamientos en escenarios relacionados con la atención médica de un hospital. Dicho sistema, nos va a servir como auxiliar para identificar las características más importantes que tienen en común los pacientes sometidos a una biopsia prostática, y que posteriormente, terminan en hospitalización.
 
# **Planteamiento de la problemática**

Este proyecto fue diseñado específicamente para uno de nuestros clientes, quien desea realizar un estudio de atención médica en salud para su hospital.

Nuestro cliente quiere conocer cuales son características principales que tienen los pacientes a los que se les ha realizado una biopsia prostática, y que luego, tienen complicaciones mayores, de manera que terminan hospitalizados. 

Para esto, se definieron los siguientes términos:
1. Como caso, se fijaron los pacientes sometidos a una biopsia prostática y que en un periodo máximo de 30 días posteriores al procedimiento presentaron fiebre, infección urinaria o sepsis; requiriendo manejo médico ambulatorio u hospitalizado para la resolución de la complicación.
2. Como control, fueron asignados los pacientes que fueron sometido a una biopsia prostática y que no presentaron complicaciones infecciosas en el período de 30 días posteriores al procedimiento.

La informacion fue proporcionada por el departamento de recopilación de datos `Antecedentes del paciente`, `Morbilidad asociada al paciente` y `Antecedentes relacionados con la toma de la biopsia` y `Complicaciones infecciosas`. En la siguiente tabla (_`Fig. 1`_), se encuentra un diccionario de datos asociados:

![image](https://user-images.githubusercontent.com/118769777/220240501-8c21461d-2de5-495b-954e-10fb9bf38014.png)
_`Fig. 1. Diccionario de datos asociados.`_

El departamento de recopilación de datos advierte que hay algunos problemas con la calidad de la información suministrada, por lo que el primer reto del equipo será realizar un análisis exploratorio de los datos con el fin de transformar y preparar las datos adecuadamente.

 # **Metodología**

# Análisis exploratorio de datos

Realizamos un exhaustivo análisis exploratorio de datos (EDA) en el que llevamos a cabo las siguientes acciones:

1. Se comprendieron los datos en cuanto a su estructura, contenido y calidad.
2. Se eliminaron las columnas irrelevantes para nuestro Modelo.
3. Corregimos y eliminamos valores atípicos y valores nulos. 
4. Transformamos nuestras variables categóricas a variables numéricas, para su próxima utilización en Machine Learning.
5. Hicimos una matriz de correlación, y utilizamos para el posterior solo las 3 mejores variables correlacionadas a nuestra varible objetivo.
6. Se realizó una técnica de sobremuestreo como preprocesamiento de datos, debido a la desproporción de datos positivos y negativos.

_Nota:_ Para más detalles, consultar el archivo *_EDA.ipynb_* donde se documenta el paso a paso del análisis y transformación de los datos.

# Modelos Machine Learning

Se desarrollaron 3 diferentes modelos de Machine learning, con y sin sobremuestreo para su comparación, los cuales fueron KNeighborsClassifier, DecisionTreeClassifier y Scalable Linear Support Vector Machine, donde hicimos lo siguiente:

1. Definimos nuestras variables predictoras y la variable objetivo, así como nuestros datos de entrenamiento y testeo.
2. Utilizamos la técnica de _`Grid Search Cross-Validation`_ para elegir los mejores hiperparámetros, y así, evaluamos el rendimiento de éstos mediante validación cruzada.
3. Se entrenó cada modelo con los hiperpárametros óptimos que se obtuvieron anteriormente.
4. Graficamos y visualizamos la _`Matriz de Confusión`_ para facilitar el analisis del modelo y sus métricas.
5. Evaluamos las tasas de verdaderos positivos y falsos positivos para cada clase por medio de _`Curvas de ROC.`_
6. Realizamos un _`Gráfico de ajuste`_, variando el parámetro mas determinante en cada modelo y de este modo observar su comportamiento y precisión.


# **Interpretación de resultado y conclusiones**

# Análisis exploratorio de datos

A través de la matriz de correlación de los datos sobremuestreados realizada en el EDA (´_Fig. 2_´), en función a la problemática planteada, encontramos que las características más influyentes en la hospitalización del paciente son las siguientes:
- FIEBRE
- ITU 
- SEPSIS, la cuál fue definida previamente por el equipo

![image](_src/assets/corr.png)
_`Fig. 2. Matriz de correlación de los datos sobremuestreados.`_
  
  
# Modelos

En las pruebas que se realizaron con los distintos modelos, se determinó que aquellos que tuvieron la menor eficacia con respecto a su funcionamiento fueron: KNeighborsClassifier y Scalable Linear Support Vector Machine. De esta manera, se estableció como el modelo indicado el DecisionTreeClassifier. A continuación, se observan los resultados del mismo.
  
## GridSearch

Los valores óptimos de los hiperparámetros **`'max_depth' y 'criterion'`** que obtuvimos en base a la técnica de  **_Grid Search Cross-Validation_**  se muestran en _`Fig. 3`_, los cuales fueron utilizados para el entrenamiento del modelo DecisionTreeClassifier.

![image](_src/assets/gridsearch.png)

_`Fig. 3. Mejores hiperparámetros`_

## **Relación exactitud/profundidad**
En la _`Fig. 4`_, podemos ver el gráfico que muestra la relación de exactitud y profundidad del árbol de decisión, en el cual variamos el parámetro de profundidad. Y efectivamente, podemos ver que el mejor desempeño del DecisionTreeClassifier se presenta cuando nuestro parámetro de profundidad tiene un valor de 2, coincidiendo con nuestros resultados en  **_Grid Search Cross-Validation_**.

![image](_src/assets/tree_depth_graphic.png)

_`Fig. 4. Exactitud con respecto a la profundidad del árbol de decisión en el modelo`_

## **Matriz de confusión**
Para finalizar, visualizamos la matriz de confusión de nuestro modelo de testeo (_`Fig. 5.`_), donde se muestran los valores de sensibilidad, especificidad y precisión con un factor de exactitud de 0.94. 

![image](_src/assets/test_matrix_confusion.png)

_`Fig. 5. Matriz de confusión con el set de testeo en KNeighborsClassifier`_

### **_Se decidio que el modelo DecisionTreeClassifier es la mejor alternativa para que satisfacer las necesidades propuestas por nuestro cliente, el cual podrá predecir con una efectividad del 94%._**
