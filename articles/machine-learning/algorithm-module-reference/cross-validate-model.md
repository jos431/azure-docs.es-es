---
title: 'Modelo de validación cruzada: Referencia del módulo'
titleSuffix: Azure Machine Learning service
description: Aprenda a usar el módulo Cross-Validate Model (Modelo de validación cruzada) del servicio Azure Machine Learning para realizar una validación cruzada de las estimaciones de parámetros de los modelos de clasificación o regresión mediante la partición de los datos.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/10/2019
ms.openlocfilehash: d83a9b5df7acc9d626613e53369f483367e55a54
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73717241"
---
# <a name="cross-validate-model"></a>Modelo de validación cruzada

En este artículo se describe cómo usar el módulo Cross-Validate Model (Modelo de validación cruzada) del diseñador de Azure Machine Learning (versión preliminar). La *validación cruzada* es una técnica que se usa a menudo en el aprendizaje automático para evaluar tanto la variabilidad de un conjunto de datos como la confiabilidad de todos los modelos entrenados con esos datos.  

El módulo Cross-Validate Model (Modelo de validación cruzada) toma como entrada un conjunto de datos con etiquetas, junto con un modelo de clasificación o regresión no entrenado. Divide el conjunto de datos en varios subconjuntos (*plegamientos*), crea un modelo en cada plegamiento y, a continuación, devuelve un conjunto de estadísticas de precisión para cada plegamiento. Para interpretar la calidad del conjunto de datos, se comparan las estadísticas de precisión de todos los pliegues. Después, sabrá si el modelo es susceptible a sufrir variaciones en los datos.  

El modelo de validación cruzada también devuelve las probabilidades y los resultados previstos para el conjunto de datos, para que pueda evaluar la confiabilidad de las predicciones.  

### <a name="how-cross-validation-works"></a>Funcionamiento de la validación cruzada

1. La validación cruzada divide aleatoriamente los datos de entrenamiento en pliegues. 

   El algoritmo se establece de forma predeterminada en 10 plegamientos si no ha particionado previamente el conjunto de datos. Para dividir el conjunto de datos en un número diferente de plegamientos, puede usar el módulo de [partición y ejemplo](partition-and-sample.md) e indicar cuántos plegamientos quiere usar.  

2.  El módulo reserva los datos del pliegue 1 para su uso en la validación. (A veces, este método se denomina *pliegue de datos de exclusión*). El módulo utiliza los pliegues restantes para entrenar un modelo. 

    Por ejemplo, si crea cinco pliegues, el módulo genera cinco modelos durante la validación cruzada. El módulo entrena cada modelo con cuatro quintos de los datos y prueba cada modelo en el quinto restante.  

3.  Durante las pruebas del modelo para cada pliegue, el módulo evalúa varias estadísticas de precisión. Las estadísticas que usa el modelo dependen del tipo de modelo que se está evaluando. Se usan estadísticas diferentes para evaluar los modelos de clasificación y los modelos de regresión.  

4.  Cuando el proceso de compilación y evaluación se completa para todos los pliegues, el modelo de validación cruzada genera un conjunto de métricas de rendimiento y resultados puntuados de todos los datos. Revise estas métricas para ver si un pliegue tiene precisión alta o baja. 

### <a name="advantages-of-cross-validation"></a>Ventajas de la validación cruzada

Una forma diferente y común de evaluar un modelo es dividir los datos en un conjunto de entrenamiento y prueba mediante [Split Data](split-data.md) (Dividir datos) y, a continuación, validar el modelo en los datos de entrenamiento. Sin embargo, la validación cruzada ofrece algunas ventajas:  

-   La validación cruzada utiliza más datos de prueba.

    La validación cruzada mide el rendimiento del modelo con los parámetros especificados en un espacio de datos mayor. Es decir, la validación cruzada utiliza todo el conjunto de datos de entrenamiento tanto para el entrenamiento como para la evaluación, en lugar de solo una parte. Por el contrario, si valida un modelo usando los datos generados a partir de una división aleatoria, se suele evaluar el modelo solo con un 30 % o menos de los datos disponibles.  

    Sin embargo, dado que la validación cruzada entrena y valida el modelo varias veces con un conjunto de datos mayor, es mucho más intensivo desde el punto de vista del proceso y tarda mucho más tiempo que la validación con una división aleatoria.  

-   La validación cruzada evalúa el conjunto de datos y el modelo.

    La validación cruzada no mide simplemente la precisión de un modelo, sino que también ofrece alguna idea sobre lo representativo que es el conjunto de datos y el grado de vulnerabilidad del modelo a las variaciones en los datos.  

## <a name="how-to-use-cross-validate-model"></a>Cómo usar el modelo de validación cruzada

La validación cruzada puede tardar mucho tiempo en ejecutarse si el conjunto de caracteres es grande.  Por lo tanto, puede usar el modelo de validación cruzada en la fase inicial de compilación y prueba del modelo. En esa fase, puede evaluar la calidad de los parámetros del modelo (suponiendo que el tiempo de proceso sea tolerable). A continuación, puede entrenar y evaluar el modelo con los parámetros establecidos con los módulos [Train model](train-model.md) (Entrenar modelo) y [Evaluate Model](evaluate-model.md) (Evaluar modelo).

En este escenario, el modelo se entrena y se prueba con el módulo Cross-Validate Model (Modelo de validación cruzada).

1. Agregue el módulo Cross-Validate Model (Modelo de validación cruzada) a la canalización. Puede encontrarlo en el diseñador de Azure Machine Learning, en la categoría **Model Scoring & Evaluation** (Puntuación y evaluación del modelo). 

2. Conecte la salida de cualquier modelo de clasificación o regresión. 

    Por ejemplo, si usa el módulo **Two Class Bayes Point Machine** (Máquina del punto de Bayes de dos clases) para la clasificación, configure el modelo con los parámetros que desee. A continuación, arrastre un conector desde el puerto **Untrained model** (Modelo sin entrenar) del clasificador hasta el puerto coincidente del modelo de validación cruzada. 

    > [!TIP] 
    > No es necesario entrenar el modelo, porque el modelo de validación cruzada lo entrena automáticamente como parte de la evaluación.  
3.  En el puerto **Dataset** (Conjunto de datos) del modelo de validación cruzada, conecte cualquier conjunto de datos de entrenamiento etiquetado.  

4.  En el panel **Properties** (Propiedades) seleccione **Launch column selector** (Iniciar selector de columna). Elija la columna única que contiene la etiqueta de clase o el valor de predicción. 

5. Establezca un valor para el parámetro **Random seed** si quiere repetir los resultados de la validación cruzada en ejecuciones posteriores de los mismos datos.  

6. Ejecución de la canalización

7. Consulte la sección [Resultados](#results) para obtener una descripción de los informes.

    Para obtener una copia del modelo para su reutilización posterior, haga clic con el botón derecho en la salida del módulo que contiene el algoritmo (por ejemplo, **Two Class Bayes Point Machine** [Máquina del punto de Bayes de dos clases]) y seleccione **Save as Trained Model** (Guardar como modelo entrenado).

## <a name="results"></a>Results

Una vez completadas todas las iteraciones, el modelo de validación cruzada crea puntuaciones para todo el conjunto de datos. También crea métricas de rendimiento que sirven para evaluar la calidad del modelo.

### <a name="scored-results"></a>Resultados puntuados

La primera salida del módulo proporciona los datos de origen de cada fila, junto con algunos valores de predicción y las probabilidades relacionadas. 

Para ver estos resultados, en la canalización, haga clic con el botón derecho en el módulo Cross Validate Model (Modelo de validación cruzada), seleccione **Scored results** (Resultados puntuados) y haga clic en **Visualize** (Visualizar).

| Nuevo nombre de columna      | DESCRIPCIÓN                              |
| -------------------- | ---------------------------------------- |
| Etiquetas puntuadas        | Esta columna se agrega al final del conjunto de datos. Contiene el valor de predicción para cada fila. |
| Probabilidades puntuadas | Esta columna se agrega al final del conjunto de datos. Indica la probabilidad estimada del valor de **Scored Labels** (Etiquetas puntuadas). |
| Número de plegamientos          | Indica el índice de base 0 del pliegue al que se asignó cada fila de datos durante la validación cruzada. |

 ### <a name="evaluation-results"></a>Evaluation results

El segundo informe se agrupa por plegamientos. Recuerde que, durante la ejecución, el modelo de validación cruzada divide aleatoriamente los datos de entrenamiento en *n* pliegues (de forma predeterminada, 10). En cada iteración sobre el conjunto de datos, el modelo de validación cruzada usa un pliegue como conjunto de datos de validación y los *n-1* pliegues restantes para entrenar un modelo. Cada uno de los *n* modelos se prueba en comparación con los datos de todos los demás plegamientos.

En este informe, los plegamientos se enumeran por valor de índice en orden ascendente.  Para ordenar por cualquier otra columna, puede guardar los resultados como un conjunto de datos.

Para ver estos resultados, en la canalización, haga clic con el botón derecho en el módulo Cross Validate Model (Modelo de validación cruzada). Seleccione **Evaluation results by fold** (Resultados de la evaluación por pliegue) y **Visualize** (Visualizar).


|Nombre de la columna| DESCRIPCIÓN|
|----|----|
|Número de plegamiento| Identificador de cada plegamiento. Si ha creado cinco pliegues, debería haber cinco subconjuntos de datos, numerados de 0 a 4.
|Número de ejemplos del plegamiento|Número de filas asignadas a cada plegamiento. Deben ser aproximadamente iguales. |


El módulo también incluye las siguientes métricas para cada pliegue, en función del tipo de modelo que se esté evaluando: 

+ **Modelos de clasificación**: precisión, recuperación, puntuación F, AUC y precisión  

+ **Modelos de regresión**: error absoluto medio, error cuadrático medio raíz, error absoluto relativo, error cuadrado relativo y coeficiente de determinación


## <a name="technical-notes"></a>Notas técnicas  

+ Se recomienda normalizar los conjuntos de datos antes de usarlos para la validación cruzada. 

+ El modelo de validación cruzada es mucho más intensivo desde el punto de vista del proceso y tarda más tiempo en completarse que si el modelo se validara con un conjunto de datos dividido aleatoriamente. La razón es que el modelo de validación cruzada entrena y valida el modelo varias veces.

+ No es necesario dividir el conjunto de datos en conjuntos de entrenamiento y de prueba cuando se usa la validación cruzada para medir la precisión del modelo. 


## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning Service. 

