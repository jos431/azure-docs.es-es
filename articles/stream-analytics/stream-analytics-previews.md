---
title: Características en vista previa (GB) de Azure Stream Analytics
description: En este artículo se enumeran las características de Azure Stream Analytics que están actualmente en versión preliminar.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 10/30/2019
ms.openlocfilehash: df3e8c1cd91c676c64d15c46c5acdc3d5bcfaa8e
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2019
ms.locfileid: "73161376"
---
# <a name="azure-stream-analytics-preview-features"></a>Características en vista previa (GB) de Azure Stream Analytics

En este artículo se resumen todas las características actualmente en versión preliminar de Azure Stream Analytics. No se recomienda el uso de características en vista previa en un entorno de producción.

## <a name="public-previews"></a>Versiones preliminares públicas

Las características siguientes se encuentran en la versión preliminar pública. Puede aprovechar las ventajas de estas características hoy mismo, pero no las use en su entorno de producción.

### <a name="online-scaling"></a>Escalado en línea

Con el escalado en línea, no es necesario detener el trabajo si necesita cambiar la asignación de la SU. Puede aumentar o disminuir la capacidad de la SU de un trabajo en ejecución sin tener que detenerlo. Esto se basa en la promesa del cliente de las canalizaciones críticas de ejecución prolongada que Stream Analytics ofrece hoy en día. Para más información, consulte [Configuración de las unidades de streaming de Azure Stream Analytics](stream-analytics-streaming-unit-consumption.md#configure-stream-analytics-streaming-units-sus).

### <a name="c-custom-de-serializers"></a>Deserializador personalizado de C#
Los desarrolladores pueden aprovechar la eficacia de Azure Stream Analytics para procesar los datos en Protobuf, XML o cualquier formato personalizado. Puede implementar [deserializadores personalizados](custom-deserializer-examples.md) en C# para deserializar los eventos recibidos por Azure Stream Analytics.

### <a name="extensibility-with-c-custom-code"></a>Extensibilidad con código personalizado de C#

Los desarrolladores que crean módulos de Stream Analytics en la nube o en IoT Edge pueden escribir o volver a usar funciones personalizadas de C# e invocarlas directamente en la consulta a través de las [funciones definidas por el usuario](stream-analytics-edge-csharp-udf-methods.md).

### <a name="managed-identity-authentication-with-power-bi"></a>Autenticación de identidad administrada con Power BI

Azure Stream Analytics ofrece compatibilidad total con la autenticación basada en identidades administrada con Power BI para una experiencia de paneles dinámica.

### <a name="anomaly-detection"></a>Detección de anomalías

Los modelos de aprendizaje automático de Azure Stream Analytics son compatibles para a detección de *picos* y *caídas*, además de detección de tendencias bidireccionales, positivas lentas y negativas lentas. Para más información, visite [Detección de anomalías en Azure Stream Analytics](stream-analytics-machine-learning-anomaly-detection.md).

### <a name="debug-query-steps-in-visual-studio"></a>Depuración de pasos de consulta en Visual Studio

Puede obtener fácilmente una vista previa del conjunto de filas intermedio en un diagrama de datos al realizar pruebas locales en las herramientas de Azure Stream Analytics para Visual Studio. 

### <a name="local-testing-with-live-data-in-visual-studio-code"></a>Consultas de prueba con datos activos en Visual Studio Code

Puede probar las consultas con datos activos en el equipo local antes de enviar el trabajo a Azure. Cada iteración de prueba tarda menos de dos a tres segundos en promedio, lo que produce un proceso de desarrollo muy eficaz.

### <a name="visual-studio-code-for-azure-stream-analytics"></a>Visual Studio Code para Azure Stream Analytics

Los trabajos de Azure Stream Analytics se pueden crear en Visual Studio Code. Consulte el [tutorial de introducción de VS Code](https://docs.microsoft.com/azure/stream-analytics/quick-create-vs-code).


### <a name="anomaly-detection"></a>Detección de anomalías

Azure Stream Analytics introduce nuevos modelos de aprendizaje automático con compatibilidad para a detección de *picos* y *caídas*, además de detección de tendencias bidireccionales, positivas lentas y negativas lentas. Para más información, visite [Detección de anomalías en Azure Stream Analytics](stream-analytics-machine-learning-anomaly-detection.md).


### <a name="integration-with-azure-machine-learning"></a>Integración con Azure Machine Learning

Puede escalar los trabajos de Stream Analytics con las funciones de Machine Learning (ML). Para obtener más información acerca de cómo puede usar las funciones de Machine Learning en un trabajo de Stream Analytics, visite [Escalado del trabajo de Análisis de transmisiones con funciones de Azure Machine Learning](stream-analytics-scale-with-machine-learning-functions.md). Eche un vistazo a un escenario realista con [Análisis de opiniones mediante Azure Stream Analytics y Azure Machine Learning](stream-analytics-machine-learning-integration-tutorial.md).


### <a name="live-data-testing-in-visual-studio"></a>Prueba de datos activos en Visual Studio

Las herramientas de Visual Studio para Azure Stream Analytics mejoran la función de pruebas locales que le permite realizar pruebas en las consultas en comparación con los flujos de datos de eventos activos desde fuentes en la nube como un centro de eventos o un centro de IoT. Aprenda cómo realizar una [Prueba local de datos activos mediante herramientas de Azure Stream Analytics para Visual Studio](stream-analytics-live-data-local-testing.md).


### <a name="net-user-defined-functions-on-iot-edge"></a>Funciones definidas por el usuario de .NET en IoT Edge

Con las funciones definidas por el usuario de . NET Standard, puede ejecutar código de .NET Standard como parte de la canalización de transmisiones de datos. Puede crear clases de C# simples o importar bibliotecas y proyectos completos. Las funciones de creación y depuración completas se admiten en Visual Studio. Para obtener más información, visite [Desarrollar funciones definidas por el usuario de .NET Standard para trabajos perimetrales de Azure Stream Analytics](stream-analytics-edge-csharp-udf-methods.md).

## <a name="other-previews"></a>Otras versiones preliminares

En la versión preliminar también están disponibles las características siguientes a petición.

### <a name="real-time-high-performance-scoring-with-custom-ml-models-managed-by-azure-machine-learning"></a>Puntuación de alto rendimiento en tiempo real con modelos de ML personalizados y administrados por Azure Machine Learning

Azure Stream Analytics admite puntuaciones en tiempo real y de alto rendimiento aprovechando modelos de Machine Learning entrenados previamente personalizados administrados por Azure Machine Learning y hospedados en Azure Kubernetes Service (AKS) o Azure Container Instances (ACI), mediante un flujo de trabajo que no requiere que escriba código. [Registrarse](https://aka.ms/asapreview1) para la versión preliminar

### <a name="support-for-azure-stack"></a>Compatibilidad con Azure Stack
Esta característica habilitada en el entorno de ejecución de Azure IoT Edge aprovecha las características personalizadas de Azure Stack, como la compatibilidad nativa con las entradas locales y las salidas que se ejecutan en Azure Stack (por ejemplo, Event Hubs, IoT Hub, Blob Storage). Esta nueva integración le permite crear arquitecturas híbridas que pueden analizar los datos cerca de donde se generan, disminuyen la latencia y maximizan las conclusiones.
Esta característica le permite crear arquitecturas híbridas que pueden analizar los datos cerca de donde se generan, disminuyen la latencia y maximizan las conclusiones. Debe [registrarse](https://aka.ms/asapreview1) para esta versión preliminar.
