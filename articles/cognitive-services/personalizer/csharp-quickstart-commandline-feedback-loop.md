---
title: 'Inicio rápido: Biblioteca cliente de Personalizer para .NET | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: Introducción a la biblioteca de cliente de Personalizer para .NET con un bucle de aprendizaje.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: quickstart
ms.date: 10/24/2019
ms.author: diberry
ms.openlocfilehash: 411bd82ade2ca7b904b36a3a4408c1a00852fc2c
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2019
ms.locfileid: "74927836"
---
# <a name="quickstart-personalizer-client-library-for-net"></a>Inicio rápido: Biblioteca cliente de Personalizer para .NET

Muestre contenido personalizado en este inicio rápido de C# con el servicio Personalizer.

Introducción a la biblioteca de cliente de Personalizer para .NET. Siga estos pasos para instalar el paquete y probar el código de ejemplo para realizar tareas básicas.

 * Clasificar una lista de acciones para su personalización.
 * Informar sobre la puntuación de recompensa para indicar el éxito de la acción con clasificación superior.

[Documentación de referencia](https://docs.microsoft.com/dotnet/api/Microsoft.Azure.CognitiveServices.Personalizer?view=azure-dotnet-preview) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Personalizer) | [Paquete (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Personalizer/) | [Ejemplos](https://github.com/Azure-Samples/cognitive-services-personalizer-samples)

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/)
* La versión actual de [.NET Core](https://dotnet.microsoft.com/download/dotnet-core).

## <a name="using-this-quickstart"></a>Uso de este inicio rápido

Hay que dar varios pasos para usar este inicio rápido:

* En Azure Portal, cree un recurso de Personalizer
* En la página **Configuración** del recurso Personalizer de Azure Portal, cambie la frecuencia de actualización del modelo.
* En un editor de código, cree un archivo de código y edítelo
* En la línea de comandos o en el terminal, instale el SDK desde la línea de comandos
* En la línea de comandos o en el terminal, ejecute el archivo de código

## <a name="create-a-personalizer-azure-resource"></a>Creación de un recurso de Azure para Personalizer

Cree un recurso para Personalizer mediante [Azure Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) o la [CLI de Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) en la máquina local. También puede:

* Obtener una [clave de prueba](https://azure.microsoft.com/try/cognitive-services) válida durante siete días de forma gratuita. Después de registrarse, estará disponible en el [sitio web de Azure](https://azure.microsoft.com/try/cognitive-services/my-apis/).  
* Ver el recurso en [Azure Portal](https://portal.azure.com/)

Después de obtener una clave del recurso o la suscripción de evaluación, cree dos [variables de entorno](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication):

* `PERSONALIZER_RESOURCE_KEY` para la clave de recurso.
* `PERSONALIZER_RESOURCE_ENDPOINT` para el punto de conexión de recurso.

En Azure Portal, los valores de clave y punto de conexión están disponibles en la página **Inicio rápido**.

## <a name="change-the-model-update-frequency"></a>Cambio de la frecuencia de actualización del modelo

En la página **Configuración** del recurso Personalizer de Azure Portal, cambie el valor de **Model update frequency** (Frecuencia de actualización del modelo) a 10 segundos. Con esta duración breve, el modelo se entrenará rápidamente, lo que permite ver cómo cambia la acción principal en cada iteración.

![Cambiar la frecuencia de actualización del modelo](./media/settings/configure-model-update-frequency-settings.png)

La primera vez que se instancia un bucle de Personalizer, no hay ningún modelo porque no se ha producido ninguna llamada a API Reward a partir de la que realizar el entrenamiento. Las llamadas de Rank devolverán las mismas probabilidades para cada elemento. La aplicación siempre debe clasificar el contenido mediante la salida de RewardActionId.

## <a name="create-a-new-c-application"></a>Creación de una aplicación de C#

Cree una nueva aplicación de consola de .NET Core en el IDE o editor que prefiera. 

En una ventana de consola (por ejemplo, cmd, PowerShell o Bash), use el comando `new` dotnet para crear una nueva aplicación de consola con el nombre `personalizer-quickstart`. Este comando crea un sencillo proyecto "Hola mundo" de C# con un solo archivo de origen: `Program.cs`. 

```console
dotnet new console -n personalizer-quickstart
```

Cambie el directorio a la carpeta de aplicaciones recién creada. Para compilar la aplicación:

```console
dotnet build
```

La salida de la compilación no debe contener advertencias ni errores. 

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

## <a name="install-the-sdk"></a>Instalación del SDK

Dentro del directorio de aplicaciones, instale la biblioteca cliente de Personalizer para .NET con el siguiente comando:

```console
dotnet add package Microsoft.Azure.CognitiveServices.Personalizer --version 0.8.0-preview
```

Si usa el IDE de Visual Studio, la biblioteca cliente estará disponible como un paquete de NuGet descargable.

## <a name="object-model"></a>Modelo de objetos

El cliente de Personalizer es un objeto [PersonalizerClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclient?view=azure-dotnet) que se autentica en Azure mediante Microsoft.Rest.ServiceClientCredentials, que contiene la clave.

Para solicitar una clasificación del contenido, cree un [RankRequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.models.rankrequest?view=azure-dotnet-preview)y, a continuación, páselo al método [cliente.Rank](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclientextensions.rank?view=azure-dotnet-preview). El método Rank devuelve un elemento RankResponse, que contiene el contenido clasificado. 

Para enviar una recompensa a Personalizer, cree un [RewardRequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.models.rewardrequest?view=azure-dotnet-preview) y, a continuación, páselo al método [client.Reward](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclientextensions.reward?view=azure-dotnet-preview). 

La determinación de la recompensa en este inicio rápido es trivial. En un sistema de producción, la determinación de lo que afecta a la [puntuación de recompensa](concept-rewards.md) y cuánto le afecta, puede ser un proceso complejo que decida cambiar con el tiempo. Esta debe ser una de las decisiones de diseño principales en la arquitectura de Personalizer. 

## <a name="code-examples"></a>Ejemplos de código

Estos fragmentos de código muestran cómo realizar las siguientes tareas con la biblioteca cliente de Personalizer para .NET:

* [Creación de un cliente de Personalizer](#create-a-personalizer-client)
* [Solicitud de una clasificación](#request-a-rank)
* [Envío de una recompensa](#send-a-reward)

## <a name="add-the-dependencies"></a>Adición de las dependencias

En el directorio del proyecto, abra el archivo **Program.cs** en el editor o IDE que prefiera. Reemplace el código existente `using` por las siguientes directivas `using`:

[!code-csharp[Using statements](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=Dependencies)]

## <a name="add-personalizer-resource-information"></a>Incorporación de la información de recursos de Personalizer

En la clase **Program**, cree variables para la clave y el punto de conexión de Azure del recurso extraídos de las variables de entorno, denominadas `PERSONALIZER_RESOURCE_KEY` y `PERSONALIZER_RESOURCE_ENDPOINT`. Si ha creado las variables de entorno una vez iniciada la aplicación, el editor, IDE o shell que se esté ejecutando se tendrá que cerrar y volver a cargar para acceder a la variable. Los métodos se crearán más adelante en este inicio rápido.

[!code-csharp[Create variables to hold the Personalizer resource key and endpoint values found in the Azure portal.](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=classVariables)]

## <a name="create-a-personalizer-client"></a>Creación de un cliente de Personalizer

A continuación, cree un método para devolver un cliente de Personalizer. El parámetro para el método es `PERSONALIZER_RESOURCE_ENDPOINT` y el elemento ApiKey es `PERSONALIZER_RESOURCE_KEY`.

[!code-csharp[Create the Personalizer client](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=authorization)]

## <a name="get-food-items-as-rankable-actions"></a>Obtención de elementos alimenticios como acciones clasificables

Las acciones representan las opciones de contenido que desea que Personalizer clasifique. Agregue los métodos siguientes a la clase Program para representar el conjunto de acciones que se va a clasificar.

[!code-csharp[Food items as actions](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=createAction)]

## <a name="get-user-preferences-for-context"></a>Obtención de las preferencias del usuario para el contexto

Agregue los métodos siguientes a la clase Program para obtener una entrada de usuario desde la línea de comandos para la hora del día y la preferencia alimentaria actual. Se utilizarán como contexto al clasificar las acciones.

[!code-csharp[Present time out day preference to the user](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=createUserFeatureTimeOfDay)]

[!code-csharp[Present food taste preference to the user](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=createUserFeatureTastePreference)]

Ambos métodos usan el método `GetKey` para leer la selección del usuario desde la línea de comandos. 

[!code-csharp[Read user's choice from the command line](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=readCommandLine)]

## <a name="create-the-learning-loop"></a>Creación del bucle de aprendizaje

El bucle de aprendizaje de Personalizer es un ciclo de llamadas de clasificación y de recompensa. En este inicio rápido, cada llamada de clasificación para personalizar el contenido, va seguida de una llamada de recompensa para indicar a Personalizer cuál es el grado de precisión con que el servicio ha clasificado el contenido. 

El código siguiente en el método `main` del programa hace un recorrido en bucle con un ciclo en el que se pide al usuario sus preferencias en la línea de comandos, y se envía esa información a Personalizer para que la clasifique, luego se presenta la selección clasificada al cliente para que elija en el lista y, a continuación, se envía una recompensa a Personalizer señalando el grado de acierto que tuvo el servicio al clasificar la selección.

[!code-csharp[Learning loop](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=mainLoop)]

Agregue los métodos siguientes, que [obtienen las opciones de contenido](#get-food-items-as-rankable-actions), antes de ejecutar el archivo de código:

* `GetActions`
* `GetUsersTimeOfDay`
* `GetUsersTastePreference`
* `GetKey`

## <a name="request-a-rank"></a>Solicitud de una clasificación

Para completar la solicitud de clasificación, el programa solicita las preferencias del usuario para crear un elemento `currentContent`de las opciones de contenido. El proceso puede crear contenido para excluir de la clasificación, que se muestra como `excludeActions`. La solicitud de clasificación necesita las acciones, currentContext, excludeActions y un identificador de evento de clasificación único (como un GUID), para recibir la respuesta clasificada. 

Este inicio rápido tiene características de contexto simples de hora del día y preferencias alimentarias del usuario. En los sistemas de producción, la determinación y [evaluación](concept-feature-evaluation.md) de [acciones y características](concepts-features.md) puede ser una cuestión menos trivial.  

[!code-csharp[The Personalizer learning loop ranks the request.](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=rank)]

## <a name="send-a-reward"></a>Envío de una recompensa

Para completar la solicitud de recompensa, el programa obtiene la selección del usuario desde la línea de comandos, asigna un valor numérico a cada selección y, después, envía el identificador único del evento de clasificación y el valor numérico al método de recompensa.

En este inicio rápido se asigna un número simple como recompensa, o cero o 1. En sistemas de producción, determinar cuándo y qué enviar a la llamada de [recompensa](concept-rewards.md) puede ser más complicado, en función de sus necesidades específicas. 

[!code-csharp[The Personalizer learning loop ranks the request.](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=reward)]

## <a name="run-the-program"></a>Ejecución del programa

Ejecute la aplicación con el comando `run` dotnet desde el directorio de aplicaciones.

```console
dotnet run
```

![El programa del inicio rápido realiza dos preguntas para recopilar las preferencias del usuario, conocidas como características, y, después, proporciona la acción principal.](media/csharp-quickstart-commandline-feedback-loop/quickstart-program-feedback-loop-example.png)

El [código fuente de este inicio rápido](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/blob/master/quickstarts/csharp/PersonalizerExample/Program.cs) está disponible en el repositorio de ejemplos de GitHub para Personalizer.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y eliminar una suscripción a Cognitive Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a él.

* [Portal](../cognitive-services-apis-create-account.md#clean-up-resources)
* [CLI de Azure](../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
>[Funcionamiento de Personalizer](how-personalizer-works.md)

* [¿Qué es Personalizer?](what-is-personalizer.md)
* [¿Dónde se puede utilizar Personalizer?](where-can-you-use-personalizer.md)
* [Solución de problemas](troubleshooting.md)

