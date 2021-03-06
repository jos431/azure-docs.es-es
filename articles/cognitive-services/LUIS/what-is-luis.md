---
title: ¿Qué es Language Understanding (LUIS)?
titleSuffix: Azure Cognitive Services
description: Language Understanding (LUIS) es un servicio de API basado en la nube que aplica inteligencia de aprendizaje automático personalizado a una conversación o un texto de lenguaje natural de un usuario para predecir el significado global y extraer información pertinente y detallada.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: overview
ms.date: 11/22/2019
ms.author: diberry
ms.openlocfilehash: 99f312521727658788e96a57b619a7c0e3d4751b
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2019
ms.locfileid: "74456555"
---
# <a name="what-is-language-understanding-luis"></a>¿Qué es Language Understanding (LUIS)?

Language Understanding (LUIS) es un servicio de API basado en la nube que aplica inteligencia de aprendizaje automático personalizado a un texto de lenguaje natural para predecir el significado global y extraer información pertinente y detallada. 

Por ejemplo, cuando una aplicación cliente envía el texto “`find me a wireless keyboard for $30`” LUIS responde con el siguiente objeto JSON. 

```JSON
{
    "query": "find me a wireless keyboard for $30",
    "prediction": {
        "topIntent": "Finditem",
        "intents": {
            "Finditem": {
                "score": 0.934672
            }
        },
        "entities": {
            "item": [
                "wireless keyboard"
            ],
            "money": [
        {
            "number": 30,
            "units": "Dollar"
        }
           ]
        }
        
    }
}
```
En el ejemplo anterior, la _**intención**_ o el significado general de la frase es que el usuario está intentando encontrar un elemento. Los fragmentos de información detallados que LUIS extrae se denominan _**entidades**_ . En este caso, las entidades son el nombre del elemento que está buscando el usuario y la cantidad de dinero que desea gastar.

Las aplicaciones cliente usan el JSON devuelto de LUIS, la _intención_ (categoría) y las _entidades_ (información detallada extraída), para impulsar acciones en la aplicación cliente. Una aplicación cliente para LUIS suele ser una aplicación conversacional que se comunica con un usuario en lenguaje natural para completar una tarea. Entre estas aplicaciones cliente se encuentran, por ejemplo, aplicaciones de redes sociales, bots de chat y aplicaciones de escritorio habilitadas para voz. 

![Imagen conceptual de tres aplicaciones cliente que funcionan con Language Understanding (LUIS) de Cognitive Services](./media/luis-overview/luis-entry-point.png "Imagen conceptual de tres aplicaciones cliente que funcionan con Language Understanding (LUIS) de Cognitive Services")

## <a name="example-use-luis-in-a-chat-bot"></a>Ejemplo de uso de LUIS en un bot de chat

<a name="Accessing-LUIS"></a>

Una aplicación cliente envía expresiones (texto) a la [API][endpoint-apis] del punto de conexión de procesamiento de lenguaje natural de LUIS publicado y recibe los resultados como respuestas en formato JSON. Una aplicación cliente común para LUIS es un bot de chat.


![Imagen conceptual del funcionamiento de LUIS con el bot de chat para predecir el texto del usuario con la comprensión del lenguaje natural (NLP)](./media/luis-overview/LUIS-chat-bot-request-response.svg "Imagen conceptual del funcionamiento de LUIS con el bot de chat para predecir el texto del usuario con la comprensión del lenguaje natural (NLP)")

|Paso|Acción|
|:--|:--|
|1|La aplicación cliente envía la _expresión_ del usuario (el texto en sus propias palabras), "I want to call my HR rep" ("Quiero llamar a mi representante de recursos humanos") al punto de conexión de LUIS como una solicitud HTTP.|
|2|LUIS aplica modelos de lenguaje de aprendizaje automático en el texto de entrada no estructurado del usuario y devuelve una respuesta con formato JSON, con una intención alta, `HRContact`. La respuesta en formato JSON del punto de conexión contiene como mínimo la expresión de consulta y la intención con mayor puntuación. También puede extraer datos, como la entidad _Tipo de contacto_.|
|3|La aplicación cliente usa la respuesta JSON para tomar decisiones acerca de cómo responder a las solicitudes del usuario. Estas decisiones pueden incluir un árbol de decisión en el bot y llamadas a otros servicios. |

La aplicación de LUIS proporciona inteligencia para que la aplicación cliente pueda tomar decisiones inteligentes. LUIS no proporciona estas opciones. 

<a name="Key-LUIS-concepts"></a>
<a name="what-is-a-luis-model"></a>

## <a name="natural-language-processing"></a>Procesamiento de lenguaje natural

La aplicación de LUIS contiene modelos de lenguaje natural específico de un dominio que funcionan conjuntamente. Puede iniciar la aplicación de LUIS con uno o varios modelos precompilados, crear el suyo propio o combinar modelos precompilados con su propia información personalizada.

* **Modelo precompilado**: LUIS tiene gran cantidad de dominios precompilados que incluyen modelos de intención y entidad que funcionan conjuntamente para completar escenarios de uso frecuentes. Estos dominios incluyen expresiones etiquetadas que se pueden inspeccionar y editar, con lo que es posible personalizarlas. Los [modelos de dominio creados previamente](luis-how-to-use-prebuilt-domains.md) incluyen todo el diseño y suponen una excelente manera de empezar a usar rápidamente LUIS. Además, hay entidades precompiladas, como la moneda y el número, que puede usar independientemente de los dominios precompilados.

* **Modelo personalizado**: LUIS le ofrece varias maneras de compilar sus propios modelos personalizados, incluidas las intenciones y las entidades. Las entidades incluyen entidades de aprendizaje automático, entidades de coincidencia de patrones y una combinación de entidades de aprendizaje automático y de coincidencia de patrones.

## <a name="build-the-luis-app"></a>Compilación de la aplicación de LUIS
Compile la aplicación con las API de [creación](https://go.microsoft.com/fwlink/?linkid=2092087) o con el [portal de LUIS](https://www.luis.ai).

La aplicación de LUIS comienza con categorías de texto de entrada llamadas **[intenciones](luis-concept-intent.md)** . Cada intención necesita ejemplos de **[expresiones](luis-concept-utterance.md)** del usuario. Cada expresión puede proporcionar datos que es necesario extraer. 

|Expresión de usuario de ejemplo|Intención|Datos extraídos|
|-----------|-----------|-----------|
|`Book a flight to __Seattle__?`|BookFlight|Seattle|
|`When does your store __open__?`|StoreHoursAndLocation|abierto|
|`Schedule a meeting at __1pm__ with __Bob__ in Distribution`|ScheduleMeeting|1 p.m., Bob|

## <a name="query-prediction-endpoint"></a>Consulta de un punto de conexión de predicción

Después de que la aplicación se ha entrenado y publicado en el punto de conexión, la aplicación cliente envía expresiones a la API del [punto de conexión](https://go.microsoft.com/fwlink/?linkid=2092356) de predicción. La API aplica la aplicación a la expresión para su análisis y responde con los resultados de predicción en formato JSON.  

La respuesta en formato JSON del punto de conexión contiene como mínimo la expresión de consulta y la intención con mayor puntuación. También puede extraer datos, como la siguiente entidad **Tipo de contacto** y la opinión general. 

```JSON
{
    "query": "I want to call my HR rep",
    "prediction": {
        "normalizedQuery": "i want to call my hr rep",
        "topIntent": "HRContact",
        "intents": {
            "HRContact": {
                "score": 0.8582669
            }
        },
        "entities": {
            "Contact Type": [
                "call"
            ]
        },
        "sentiment": {
            "label": "negative",
            "score": 0.103343368
        }
    }
}
```

## <a name="improve-model-prediction"></a>Mejora de la predicción del modelo

Después de publicar la aplicación de LUIS y recibir expresiones de usuario reales, LUIS proporciona [aprendizaje activo](luis-concept-review-endpoint-utterances.md) de las expresiones del punto de conexión para mejorar la precisión de la predicción. 

<a name="using-luis"></a>

## <a name="iterative-development-lifecycle"></a>Ciclo de vida de desarrollo iterativo
LUIS proporciona herramientas, control de versiones y colaboración con otros autores de LUIS para integrarse en el [ciclo de vida de desarrollo iterativo](luis-concept-app-iteration.md) completo. 

## <a name="implementing-luis"></a>Implementación de LUIS
Language Understanding (LUIS), como API REST, se puede usar con cualquier producto, servicio o marco con una solicitud HTTP. La lista siguiente contiene los principales productos y servicios de Microsoft que se utilizan con LUIS.

La principal aplicación cliente para LUIS es:
* [Bot de aplicación web](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0) para crear rápidamente un bot de chat habilitado para LUIS para hablar con un usuario mediante entrada de texto. Usa [Bot Framework][bot-framework] versión [4.x](https://github.com/Microsoft/botbuilder-dotnet) para una experiencia de bot completa.

Herramientas para usar de forma rápida y sencilla LUIS con un bot:
* [CLI de LUIS](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUIS): paquete NPM que proporciona creación y predicción mediante una herramienta independiente de la línea de comandos o como importación. 
* [LUISGen](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUISGen) LUISGen es una herramienta para generar código fuente de C# y de TypeScript fuertemente tipado a partir de un modelo de LUIS exportado.
* [Dispatch](https://aka.ms/dispatch-tool) permite que una aplicación primaria utilice varias aplicaciones de LUIS y QnA Maker mediante el uso de un modelo de distribuidor.
* [LUDown](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/Ludown) LUDown es una herramienta de la línea de comandos que ayuda a administrar los modelos de lenguaje para el bot.
* [Bot Framework Composer](https://github.com/microsoft/BotFramework-Composer): es una herramienta de desarrollo integrado para desarrolladores y equipos multidisciplinares que permite crear bots y experiencias de conversación con Microsoft Bot Framework.

Otros servicios de Cognitive Services que utilizan LUIS:
* [QnA Maker][qnamaker] permite la combinación de varios tipos de texto en una base de conocimiento de preguntas y respuestas.
* [Servicio Voz](../Speech-Service/overview.md) convierte solicitudes de lenguaje hablado en texto. 
* [Conversation Learner](https://docs.microsoft.com/azure/cognitive-services/labs/conversation-learner/overview) le permite crear conversaciones de bot más rápido con LUIS.

Ejemplos de uso de LUIS:
* Repositorio de GitHub para [Inteligencia artificial conversacional](https://github.com/Microsoft/AI).
* [Ejemplos de Bot Framework: bot](https://github.com/microsoft/BotBuilder-Samples)

## <a name="next-steps"></a>Pasos siguientes

* [Novedades](whats-new.md)
* Cree una nueva aplicación de LUIS con un dominio [precompilado](luis-get-started-create-app.md) o [personalizado](luis-quickstart-intents-only.md).
* [Consulte el punto de conexión de predicción](luis-get-started-get-intent-from-browser.md) de una aplicación de IoT pública. 
* [Recursos para el desarrollador](developer-reference-resource.md) de LUIS 

[bot-framework]: https://docs.microsoft.com/bot-framework/
[flow]: https://docs.microsoft.com/connectors/luis/
[authoring-apis]: https://go.microsoft.com/fwlink/?linkid=2092087
[endpoint-apis]: https://go.microsoft.com/fwlink/?linkid=2092356
[qnamaker]: https://qnamaker.ai/
