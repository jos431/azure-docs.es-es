---
title: Interpretación de modelos para ejecuciones locales y remotas
titleSuffix: Azure Machine Learning
description: Aprenda a obtener explicaciones sobre cómo el modelo de aprendizaje automático determina la importancia de las características y realiza predicciones al usar el SDK de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mesameki
author: mesameki
ms.reviewer: trbye
ms.date: 10/25/2019
ms.openlocfilehash: ffb9e0547c44ee47a43de00e51933ce7d0584759
ms.sourcegitcommit: 28688c6ec606ddb7ae97f4d0ac0ec8e0cd622889
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2019
ms.locfileid: "74158730"
---
# <a name="model-interpretability-for-local-and-remote-runs"></a>Interpretación de modelos para ejecuciones locales y remotas

[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

En este artículo, aprenderá a usar el paquete de interpretación del SDK de Python para Azure Machine Learning para comprender por qué el modelo ha realizado sus predicciones. Aprenderá a:

* Interpretar modelos de aprendizaje automático entrenados tanto localmente como en recursos informáticos remotos
* Almacenar explicaciones locales y globales sobre el historial de ejecución de Azure
* Ver visualizaciones de interpretación en [Azure Machine Learning Studio](https://ml.azure.com)
* Implementar un explicador de puntuación con el modelo

Para más información, consulte [Interpretación del modelo de Azure Machine Learning Service](how-to-machine-learning-interpretability.md).

## <a name="local-interpretability"></a>Interpretación local

En el siguiente ejemplo se muestra cómo usar localmente el paquete de interpretación sin ponerse en contacto con los servicios de Azure.

1. Si es necesario, ejecute `pip install azureml-interpret` para obtener el paquete de interpretación.

1. Entrene un modelo de muestra en un cuaderno local de Jupyter.

    ```python
    # load breast cancer dataset, a well-known small dataset that comes with scikit-learn
    from sklearn.datasets import load_breast_cancer
    from sklearn import svm
    from sklearn.model_selection import train_test_split
    breast_cancer_data = load_breast_cancer()
    classes = breast_cancer_data.target_names.tolist()
    
    # split data into train and test
    from sklearn.model_selection import train_test_split
    x_train, x_test, y_train, y_test = train_test_split(breast_cancer_data.data,            
                                                        breast_cancer_data.target,  
                                                        test_size=0.2,
                                                        random_state=0)
    clf = svm.SVC(gamma=0.001, C=100., probability=True)
    model = clf.fit(x_train, y_train)
    ```

1. Llame al explicador localmente:
   * Para inicializar un objeto de explicación, pase el modelo y algunos datos de entrenamiento al constructor de la explicación.
   * Para que las explicaciones y visualizaciones sean más informativas, puede optar por pasar los nombres de las características y los nombres de las clases de salida si está realizando la clasificación.

   Los siguientes bloques de código muestran cómo crear localmente una instancia de un objeto explicador con `TabularExplainer`, `MimicExplainer` y `PFIExplainer`.
   * `TabularExplainer` llama a uno de los tres explicadores de SHA situados debajo (`TreeExplainer`, `DeepExplainer` o `KernelExplainer`).
   * `TabularExplainer` selecciona automáticamente el más adecuado para su caso de uso, pero puede llamar a cada uno de los tres explicadores subyacentes directamente.

    ```python
    from interpret.ext.blackbox import TabularExplainer

    # "features" and "classes" fields are optional
    explainer = TabularExplainer(model, 
                                 x_train, 
                                 features=breast_cancer_data.feature_names, 
                                 classes=classes)
    ```

    o

    ```python

    from interpret.ext.blackbox import MimicExplainer
    
    # you can use one of the following four interpretable models as a global surrogate to the black box model
    
    from interpret.ext.glassbox import LGBMExplainableModel
    from interpret.ext.glassbox import LinearExplainableModel
    from interpret.ext.glassbox import SGDExplainableModel
    from interpret.ext.glassbox import DecisionTreeExplainableModel

    # "features" and "classes" fields are optional
    # augment_data is optional and if true, oversamples the initialization examples to improve surrogate model accuracy to fit original model.  Useful for high-dimensional data where the number of rows is less than the number of columns.
    # max_num_of_augmentations is optional and defines max number of times we can increase the input data size.
    # LGBMExplainableModel can be replaced with LinearExplainableModel, SGDExplainableModel, or DecisionTreeExplainableModel
    explainer = MimicExplainer(model, 
                               x_train, 
                               LGBMExplainableModel, 
                               augment_data=True, 
                               max_num_of_augmentations=10, 
                               features=breast_cancer_data.feature_names, 
                               classes=classes)
    ```

    o

    ```python
    from interpret.ext.blackbox import PFIExplainer

    # "features" and "classes" fields are optional
    explainer = PFIExplainer(model,
                             features=breast_cancer_data.feature_names, 
                             classes=classes)
    ```

### <a name="overall-global-feature-importance-values"></a>Valores de importancia de la característica general (global)

Consulte el ejemplo siguiente para ayudarle a obtener los valores de importancia de la característica global.

```python

# you can use the training data or the test data here
global_explanation = explainer.explain_global(x_train)

# if you used the PFIExplainer in the previous step, use the next line of code instead
# global_explanation = explainer.explain_global(x_train, true_labels=y_test)

# sorted feature importance values and feature names
sorted_global_importance_values = global_explanation.get_ranked_global_values()
sorted_global_importance_names = global_explanation.get_ranked_global_names()
dict(zip(sorted_global_importance_names, sorted_global_importance_values))

# alternatively, you can print out a dictionary that holds the top K feature names and values
global_explanation.get_feature_importance_dict()
```

### <a name="instance-level-local-feature-importance-values"></a>Valores de importancia de la característica a nivel de instancia (local)

Obtenga los valores de la importancia de la característica local mediante la llamada a explicaciones para una instancia individual o un grupo de instancias.
> [!NOTE]
> `PFIExplainer` no admite explicaciones locales.

```python
# get explanation for the first data point in the test set
local_explanation = explainer.explain_local(x_test[0:5])

# sorted feature importance values and feature names
sorted_local_importance_names = local_explanation.get_ranked_local_names()
sorted_local_importance_values = local_explanation.get_ranked_local_values()
```

## <a name="interpretability-for-remote-runs"></a>Interpretación de las ejecuciones remotas

En el ejemplo siguiente se muestra cómo usar la clase `ExplanationClient` para habilitar la interpretación del modelo para ejecuciones remotas. Es similar conceptualmente al proceso local, excepto que el usuario:

* Usa `ExplanationClient` en la ejecución remota para cargar el contexto de interpretación.
* Descarga el contexto más adelante en un entorno local.

1. Usa `pip install azureml-contrib-interpret` para obtener el paquete necesario.

1. Cree un script de entrenamiento en un cuaderno local de Jupyter. Por ejemplo, `train_explain.py`.

    ```python
    from azureml.contrib.interpret.explanation.explanation_client import ExplanationClient
    from azureml.core.run import Run
    from interpret.ext.blackbox import TabularExplainer

    run = Run.get_context()
    client = ExplanationClient.from_run(run)

    # write code to get and split your data into train and test sets here
    # write code to train your model here 

    # explain predictions on your local machine
    # "features" and "classes" fields are optional
    explainer = TabularExplainer(model, 
                                 x_train, 
                                 features=feature_names, 
                                 classes=classes)

    # explain overall model predictions (global explanation)
    global_explanation = explainer.explain_global(x_test)
    
    # uploading global model explanation data for storage or visualization in webUX
    # the explanation can then be downloaded on any compute
    # multiple explanations can be uploaded
    client.upload_model_explanation(global_explanation, comment='global explanation: all features')
    # or you can only upload the explanation object with the top k feature info
    #client.upload_model_explanation(global_explanation, top_k=2, comment='global explanation: Only top 2 features')
    ```

1. Configure una instancia de Azure Machine Learning Compute como destino de proceso y envíe la ejecución de entrenamiento. Para más información, consulte [Configuración de destinos de proceso del entrenamiento del modelo](how-to-set-up-training-targets.md#amlcompute). También puede encontrar útil los [cuadernos de ejemplo](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model/azure-integration/remote-explanation).

1. Descargue la explicación en el cuaderno de Jupyter Notebook local.

    ```python
    from azureml.contrib.interpret.explanation.explanation_client import ExplanationClient
    
    client = ExplanationClient.from_run(run)
    
    # get model explanation data
    explanation = client.download_model_explanation()
    # or only get the top k (e.g., 4) most important features with their importance values
    explanation = client.download_model_explanation(top_k=4)
    
    global_importance_values = explanation.get_ranked_global_values()
    global_importance_names = explanation.get_ranked_global_names()
    print('global importance values: {}'.format(global_importance_values))
    print('global importance names: {}'.format(global_importance_names))
    ```

## <a name="raw-feature-transformations"></a>Transformaciones de características sin procesar

Puede optar por obtener explicaciones en términos de características sin procesar y sin transformar en lugar de características diseñadas. Para esta opción, se pasa la canalización de transformación de características al explicador en `train_explain.py`. De lo contrario, el explicador proporciona explicaciones en cuanto a las características diseñadas.

El formato de las transformaciones que se admiten es el mismo que se describe en [sklearn-pandas](https://github.com/scikit-learn-contrib/sklearn-pandas). En general, se admiten todas las transformaciones, siempre que operen en una sola columna y, por tanto, sean claramente uno a varios.

Obtenga una explicación para las características sin procesar mediante `sklearn.compose.ColumnTransformer` o una lista de tuplas de transformadores apropiadas. En el ejemplo siguiente se usa `sklearn.compose.ColumnTransformer`.

```python
from sklearn.compose import ColumnTransformer

numeric_transformer = Pipeline(steps=[
    ('imputer', SimpleImputer(strategy='median')),
    ('scaler', StandardScaler())])

categorical_transformer = Pipeline(steps=[
    ('imputer', SimpleImputer(strategy='constant', fill_value='missing')),
    ('onehot', OneHotEncoder(handle_unknown='ignore'))])

preprocessor = ColumnTransformer(
    transformers=[
        ('num', numeric_transformer, numeric_features),
        ('cat', categorical_transformer, categorical_features)])

# append classifier to preprocessing pipeline.
# now we have a full prediction pipeline.
clf = Pipeline(steps=[('preprocessor', preprocessor),
                      ('classifier', LogisticRegression(solver='lbfgs'))])


# clf.steps[-1][1] returns the trained classification model
# pass transformation as an input to create the explanation object
# "features" and "classes" fields are optional
tabular_explainer = TabularExplainer(clf.steps[-1][1],
                                     initialization_examples=x_train,
                                     features=dataset_feature_names,
                                     classes=dataset_classes,
                                     transformations=preprocessor)
```

Si desea ejecutar el ejemplo con la lista de tuplas de transformadores apropiadas, use el siguiente código:

```python
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.linear_model import LogisticRegression
from sklearn_pandas import DataFrameMapper

# assume that we have created two arrays, numerical and categorical, which holds the numerical and categorical feature names

numeric_transformations = [([f], Pipeline(steps=[('imputer', SimpleImputer(
    strategy='median')), ('scaler', StandardScaler())])) for f in numerical]

categorical_transformations = [([f], OneHotEncoder(
    handle_unknown='ignore', sparse=False)) for f in categorical]

transformations = numeric_transformations + categorical_transformations

# append model to preprocessing pipeline.
# now we have a full prediction pipeline.
clf = Pipeline(steps=[('preprocessor', DataFrameMapper(transformations)),
                      ('classifier', LogisticRegression(solver='lbfgs'))])

# clf.steps[-1][1] returns the trained classification model
# pass transformation as an input to create the explanation object
# "features" and "classes" fields are optional
tabular_explainer = TabularExplainer(clf.steps[-1][1],
                                     initialization_examples=x_train,
                                     features=dataset_feature_names,
                                     classes=dataset_classes,
                                     transformations=transformations)
```

## <a name="visualizations"></a>Visualizaciones

Después de descargar las explicaciones en su cuaderno local de Jupyter, puede usar el panel de visualización para reconocer e interpretar el modelo.

### <a name="global-visualizations"></a>Visualizaciones globales

Los siguientes gráficos proporcionan una vista global del modelo entrenado, junto con sus predicciones y explicaciones.

|Gráfico|DESCRIPCIÓN|
|----|-----------|
|Exploración de datos| Muestra una información general del conjunto de datos, junto con los valores de predicción.|
|Importancia global|Muestra globalmente las principales características importantes de K (K configurable). Ayuda a entender el comportamiento global del modelo subyacente.|
|Exploración de la explicación|Muestra hasta qué punto una característica afecta a un cambio en los valores de predicción de un modelo o la probabilidad de los valores de predicción. Muestra el impacto de la interacción de características.|
|Resumen de importancia|Usa los valores de importancia de la característica locales en todos los puntos de datos para mostrar la distribución del impacto de cada característica en el valor de predicción.|

[![Panel de visualización global](./media/machine-learning-interpretability-explainability/global-charts.png)](./media/machine-learning-interpretability-explainability/global-charts.png#lightbox)

### <a name="local-visualizations"></a>Visualizaciones locales

Para cargar el trazado local de importancia de la característica para cualquier punto de datos, seleccione el punto de datos individual en el trazado.

|Gráfico|DESCRIPCIÓN|
|----|-----------|
|Importancia local|Muestra las principales características importantes de K (K configurable) globalmente. Ayuda a mostrar el comportamiento local del modelo subyacente en un punto de datos concreto.|
|Exploración de perturbaciones|Permite los cambios en los valores de las características del punto de datos seleccionado y observar los cambios resultantes en el valor de predicción.|
|Expectativa condicional individual (ICE)| Permite que el valor de la característica cambie de un valor mínimo a uno máximo. Ayuda a mostrar cómo cambia la predicción del punto de datos cuando se modifica una característica.|

[![Importancia de características locales del panel de visualización](./media/machine-learning-interpretability-explainability/local-charts.png)](./media/machine-learning-interpretability-explainability/local-charts.png#lightbox)


[![Perturbation de características del panel de visualización](./media/machine-learning-interpretability-explainability/perturbation.gif)](./media/machine-learning-interpretability-explainability/perturbation.gif#lightbox)


[![Gráficos ICE del panel de visualización](./media/machine-learning-interpretability-explainability/ice-plot.png)](./media/machine-learning-interpretability-explainability/ice-plot.png#lightbox)

> [!NOTE]
> Antes de que se inicie el kernel de Jupyter, asegúrese de habilitar las extensiones de widget para el panel de visualización.

* Jupyter Notebook

    ```shell
    jupyter nbextension install --py --sys-prefix azureml.contrib.interpret.visualize
    jupyter nbextension enable --py --sys-prefix azureml.contrib.interpret.visualize
    ```

* JupyterLab

    ```shell
    jupyter labextension install @jupyter-widgets/jupyterlab-manager
    jupyter labextension install microsoft-mli-widget
    ```

Para cargar el panel de visualización, use el siguiente código:

```python
from azureml.contrib.interpret.visualize import ExplanationDashboard

ExplanationDashboard(global_explanation, model, x_test)
```

### <a name="visualization-in-azure-machine-learning-studio"></a>Visualización en Azure Machine Learning Studio

Si completa los pasos de la [interpretación remota](#interpretability-for-remote-runs), puede ver el panel de visualización de [Azure Machine Learning Studio](https://ml.azure.com). Este panel es una versión más sencilla del panel de visualización que se explicó anteriormente. Solo admite dos pestañas:

|Gráfico|DESCRIPCIÓN|
|----|-----------|
|Importancia global|Muestra globalmente las principales características importantes de K (K configurable). Ayuda a entender el comportamiento global del modelo subyacente.|
|Resumen de importancia|Usa los valores de importancia de la característica locales en todos los puntos de datos para mostrar la distribución del impacto de cada característica en el valor de predicción.|

Si se dispone de explicaciones globales y locales, los datos rellenan ambas pestañas. Si solo hay una explicación global disponible, se deshabilitará la pestaña del resumen de importancia.

Siga una de estas rutas de acceso para llegar al panel de visualización de Azure Machine Learning Studio:

* Panel **Experiments** (Experimentos) (versión preliminar)
  1. Al hacer clic en **Experiments** (Experimentos) en el panel izquierdo, verá una lista de experimentos que se han ejecutado en Azure Machine Learning Service.
  1. Seleccione un experimento determinado para ver todas las ejecuciones de ese experimento.
  1. Seleccione una ejecución y, después, la pestaña **Explanations** (Explicaciones) para ver el panel de visualización de explicaciones.

   [![Importancia de características locales del panel de visualización](./media/machine-learning-interpretability-explainability/amlstudio-experiments.png)](./media/machine-learning-interpretability-explainability/amlstudio-experiments.png#lightbox)

* Panel **Models** (Modelos)
  1. Si registró el modelo original siguiendo los pasos descritos en [Implementación de modelos con Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where), puede seleccionar **Models** (Modelos) en el panel izquierdo para verlo.
  1. Desde aquí, haga clic en la pestaña **Explanations** (Explicaciones) para ver el panel de visualización de explicaciones.

## <a name="interpretability-at-inference-time"></a>Interpretación en tiempo de inferencia

Puede implementar el explicador junto con el modelo original y usarlo a la hora de la inferencia para proporcionar la información de la explicación local. También ofrecemos explicadores de puntuación más ligeros para mejorar el rendimiento de la interpretación a la hora de la inferencia. El proceso de implementación de una explicación de puntuación ligera es similar al de implementación de un modelo e incluye los siguientes pasos:

1. Cree un objeto de explicación. Por ejemplo, puede usar `TabularExplainer`:

   ```python
    from interpret.ext.blackbox import TabularExplainer


   explainer = TabularExplainer(model, 
                                initialization_examples=x_train, 
                                features=dataset_feature_names, 
                                classes=dataset_classes, 
                                transformations=transformations)
   ```

1. Cree un explicador de puntuación mediante el objeto de explicación:

   ```python
   from azureml.contrib.interpret.scoring.scoring_explainer import KernelScoringExplainer, save

   # create a lightweight explainer at scoring time
   scoring_explainer = KernelScoringExplainer(explainer)

   # pickle scoring explainer
   # pickle scoring explainer locally
   OUTPUT_DIR = 'my_directory'
   save(scoring_explainer, directory=OUTPUT_DIR, exist_ok=True)
   ```

1. Configure y registre una imagen que utilice al modelo de explicación de puntuación.

   ```python
   # register explainer model using the path from ScoringExplainer.save - could be done on remote compute
   # scoring_explainer.pkl is the filename on disk, while my_scoring_explainer.pkl will be the filename in cloud storage
   run.upload_file('my_scoring_explainer.pkl', os.path.join(OUTPUT_DIR, 'scoring_explainer.pkl'))
   
   scoring_explainer_model = run.register_model(model_name='my_scoring_explainer', 
                                                model_path='my_scoring_explainer.pkl')
   print(scoring_explainer_model.name, scoring_explainer_model.id, scoring_explainer_model.version, sep = '\t')
   ```

1. Como un paso opcional, puede recuperar el explicador de puntuación en la nube y probar las explicaciones.

   ```python
   from azureml.contrib.interpret.scoring.scoring_explainer import load

   # retrieve the scoring explainer model from cloud"
   scoring_explainer_model = Model(ws, 'my_scoring_explainer')
   scoring_explainer_model_path = scoring_explainer_model.download(target_dir=os.getcwd(), exist_ok=True)

   # load scoring explainer from disk
   scoring_explainer = load(scoring_explainer_model_path)

   # test scoring explainer locally
   preds = scoring_explainer.explain(x_test)
   print(preds)
   ```

1. Implemente la imagen en un destino de proceso, para lo que debe seguir estos pasos:

   1. Si lo necesita, registre el modelo de predicción original; para ello, siga los pasos de [Implementación de modelos con Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where).

   1. Cree un archivo de puntuación.

         ```python
         %%writefile score.py
         import json
         import numpy as np
         import pandas as pd
         import os
         import pickle
         from sklearn.externals import joblib
         from sklearn.linear_model import LogisticRegression
         from azureml.core.model import Model
          
         def init():
         
            global original_model
            global scoring_model
             
            # retrieve the path to the model file using the model name
            # assume original model is named original_prediction_model
            original_model_path = Model.get_model_path('original_prediction_model')
            scoring_explainer_path = Model.get_model_path('my_scoring_explainer')

            original_model = joblib.load(original_model_path)
            scoring_explainer = joblib.load(scoring_explainer_path)

         def run(raw_data):
            # get predictions and explanations for each data point
            data = pd.read_json(raw_data)
            # make prediction
            predictions = original_model.predict(data)
            # retrieve model explanations
            local_importance_values = scoring_explainer.explain(data)
            # you can return any data type as long as it is JSON-serializable
            return {'predictions': predictions.tolist(), 'local_importance_values': local_importance_values}
         ```
   1. Defina la configuración de la implementación.

         Esta configuración depende de los requisitos del modelo. En el ejemplo siguiente se define una configuración que usa un núcleo de CPU y 1 GB de memoria.

         ```python
         from azureml.core.webservice import AciWebservice

          aciconfig = AciWebservice.deploy_configuration(cpu_cores=1,
                                                    memory_gb=1,
                                                    tags={"data": "NAME_OF_THE_DATASET",
                                                          "method" : "local_explanation"},
                                                    description='Get local explanations for NAME_OF_THE_PROBLEM')
         ```

   1. Cree un archivo con las dependencias del entorno.

         ```python
         from azureml.core.conda_dependencies import CondaDependencies

         # WARNING: to install this, g++ needs to be available on the Docker image and is not by default (look at the next cell)

         azureml_pip_packages = ['azureml-defaults', 'azureml-contrib-interpret', 'azureml-core', 'azureml-telemetry', 'azureml-interpret']
 

         # specify CondaDependencies obj
         myenv = CondaDependencies.create(conda_packages=['scikit-learn', 'pandas'],
                                          pip_packages=['sklearn-pandas'] + azureml_pip_packages,
                                          pin_sdk_version=False)


         with open("myenv.yml","w") as f:
            f.write(myenv.serialize_to_string())

         with open("myenv.yml","r") as f:
            print(f.read())
         ```

   1. Cree un dockerfile personalizado con g++ instalado.

         ```python
         %%writefile dockerfile
         RUN apt-get update && apt-get install -y g++
         ```

   1. Implemente la imagen creada.
   
         Este proceso tarda unos cinco minutos.

         ```python
         from azureml.core.webservice import Webservice
         from azureml.core.image import ContainerImage

         # use the custom scoring, docker, and conda files we created above
         image_config = ContainerImage.image_configuration(execution_script="score.py",
                                                         docker_file="dockerfile",
                                                         runtime="python",
                                                         conda_file="myenv.yml")

         # use configs and models generated above
         service = Webservice.deploy_from_model(workspace=ws,
                                             name='model-scoring-service',
                                             deployment_config=aciconfig,
                                             models=[scoring_explainer_model, original_model],
                                             image_config=image_config)

         service.wait_for_deployment(show_output=True)
         ```

1. Pruebe la implementación.

    ```python
    import requests

    # create data to test service with
    examples = x_list[:4]
    input_data = examples.to_json()

    headers = {'Content-Type':'application/json'}

    # send request to service
    resp = requests.post(service.scoring_uri, input_data, headers=headers)

    print("POST to url", service.scoring_uri)
    # can covert back to Python objects from json string if desired
    print("prediction:", resp.text)
    ```

1. Limpiar.

   Para eliminar un servicio web implementado, use `service.delete()`.

## <a name="next-steps"></a>Pasos siguientes

[Más información sobre la interpretación de modelos](how-to-machine-learning-interpretability.md)