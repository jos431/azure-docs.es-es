---
title: 'Inicio rápido: Reconocimiento de voz, intenciones y entidades en Python: servicio de voz'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/28/2019
ms.author: erhopf
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 9a4c7f24a2e28743679e312e3dce0bc605db6749
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74815894"
---
## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, compruebe lo siguiente:

> [!div class="checklist"]
>
> * [Ha creado un recurso de Voz de Azure](../../../../get-started.md)
> * [Ha creado una aplicación de Language Understanding LUIS y ha obtenido una clave de punto de conexión](../../../../quickstarts/create-luis.md)
> * [Ha configurado el entorno de desarrollo](../../../../quickstarts/setup-platform.md)
> * [Ha creado un proyecto de ejemplo vacío](../../../../quickstarts/create-project.md)

## <a name="open-your-project"></a>Apertura del proyecto

Abra Quickstart.py en el editor de Python.

## <a name="start-with-some-boilerplate-code"></a>Inicio con código reutilizable

Vamos a agregar código que funcione como el esqueleto del proyecto.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=5-7)]

## <a name="create-a-speech-configuration"></a>Creación de una configuración de Voz

Antes de inicializar un objeto `IntentRecognizer`, debe crear una configuración que use la clave y la región del punto de conexión de LUIS. Inserte este código a continuación.

En este ejemplo se crea el objeto `SpeechConfig` mediante la clave y la región de LUIS. Para ver una lista completa de los métodos disponibles, consulte [Clase SpeechConfig](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig).
El SDK de Voz se usará de forma predeterminada para reconocer el uso de en-us como idioma. Para más información sobre cómo elegir el idioma de origen, consulte [Especificación del idioma de origen para la conversión de voz a texto](../../../../how-to-specify-source-language.md).

> [!NOTE]
> Es importante usar la clave del punto de conexión de LUIS y no las claves de inicio o creación, ya que solo la clave del punto de conexión es válida para el reconocimiento de la conversión de voz en intención. Consulte [Creación de una aplicación de LUIS y obtención de una clave de punto de conexión](~/articles/cognitive-services/Speech-Service/quickstarts/create-luis.md) para instrucciones sobre cómo conseguir la clave correcta.

[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=12)]

## <a name="initialize-an-intentrecognizer"></a>Inicialización de IntentRecognizer

Ahora, vamos a crear un objeto `IntentRecognizer`. Inserte este código justo debajo de la configuración de Voz.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=15)]

## <a name="add-a-languageunderstandingmodel-and-intents"></a>Adición de un objeto LanguageUnderstandingModel e intenciones

Ahora debe asociar un objeto `LanguageUnderstandingModel` con el reconocedor de intenciones y agregar las intenciones que quiera que se reconozcan.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=19-27)]

## <a name="recognize-an-intent"></a>Reconocimiento de una intención

En el objeto `IntentRecognizer`, va a llamar al método `recognize_once()`. Este método permite que el servicio Voz sepa que solo va a enviar una frase para el reconocimiento y que, una vez que se identifica la frase, se detendrá el reconocimiento de voz.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=35)]

## <a name="display-the-recognition-results-or-errors"></a>Visualización de los resultados (o errores) del reconocimiento

Cuando el servicio Voz devuelva el resultado del reconocimiento, querrá hacer algo con él. Vamos a hacer algo tan sencillo como imprimir el resultado en la consola.

Dentro de la instrucción using, debajo de la llamada a `recognize_once()`, agregue este código: [!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=38-47)].

## <a name="check-your-code"></a>Comprobación del código

En este momento, el código debe tener esta apariencia:  
(Se han agregado algunos comentarios a esta versión) [!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=5-47)]

## <a name="build-and-run-your-app"></a>Compilación y ejecución de la aplicación

Ejecute el ejemplo desde la consola o en el IDE:

    ````
    python quickstart.py
    ````

Los próximos 15 segundos de la entrada de voz del micrófono se reconocen y se registran en la ventana de consola.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [footer](./footer.md)]
