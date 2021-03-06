---
title: Migración de Indexer v1 y v2 a Video Indexer de Azure Media Services | Microsoft Docs
description: En este tema se aborda cómo migrar de Azure Media Indexer v1 y v2 a Video Indexer de Azure Media Services.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2019
ms.author: juliako
ms.openlocfilehash: 791287d693903007d09c2e82025bfe195f9f15d1
ms.sourcegitcommit: 95931aa19a9a2f208dedc9733b22c4cdff38addc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74464044"
---
# <a name="migrate-from-media-indexer-and-media-indexer-2-to-video-indexer"></a>Migración de Media Indexer y Media Indexer 2 a Video Indexer

El procesador de multimedia de [Azure Media Indexer](media-services-index-content.md) se retirará el 1 de octubre de 2020. Los procesadores multimedia [Azure Media Indexer v2 Preview](media-services-process-content-with-indexer2.md) se retirarán el 1 de enero de 2020.  [Video Indexer de Azure Media Services ](https://docs.microsoft.com/azure/media-services/video-indexer/) reemplaza a estos procesadores multimedia heredados.

Video Indexer de Azure Media Services se basa en Azure Media Analytics, Azure Cognitive Search y Cognitive Services (como Face API, Microsoft Translator, Computer Vision API y Custom Speech Service). Permite extraer conclusiones de los vídeos con los modelos de vídeo y audio de Video Indexer. Para ver en qué escenarios se puede usar Video Indexer, qué características ofrece y cómo empezar, consulte [Modelos de audio y vídeo de Video Indexer](../video-indexer/video-indexer-overview.md). 

También puede extraer la información de los archivos de audio y vídeo mediante los [valores preestablecidos del analizador de Azure Media Services v3](../latest/analyzing-video-audio-files-concept.md) o directamente mediante las [API de Video Indexer](https://api-portal.videoindexer.ai/). Actualmente hay una superposición entre las características que ofrecen las API de Video Indexer y las API de Media Services v3.

> [!NOTE]
> Para saber cuándo se recomienda utilizar los valores preestablecidos del analizador de Video Indexer, en lugar de los del de Media Services, consulte el [documento en que se realiza una comparación de ambos](../video-indexer/compare-video-indexer-with-media-services-presets.md). 

En este artículo se describen los pasos para migrar desde Azure Media Indexer y Azure Media Indexer 2 a Video Indexer de Azure Media Services.  

## <a name="migration-options"></a>Opciones de migración 

|Si requiere  |y luego |
|---|---|
|una solución que proporcione una transcripción de voz a texto para cualquier formato de archivo multimedia en formatos de archivo de subtítulos VTT, SRT o TTML,<br/>así como información de audio adicional, como palabras clave, inferencia de temas, eventos acústicos, diarización de hablantes, traducción y extracción de entidades| actualice las aplicaciones para usar las capacidades de Video Indexer de Azure a través de la API REST de Video Indexer v2 o los valores preestablecidos del analizador de audio de Azure Media Services v3.|
|capacidades de voz a texto| use Cognitive Services Speech API directamente.|  

## <a name="getting-started-with-video-indexer"></a>Introducción a Video Indexer

En la siguiente sección se indican vínculos relevantes: [¿Cómo puedo empezar a usar Video Indexer?](https://docs.microsoft.com/azure/media-services/video-indexer/video-indexer-overview#how-can-i-get-started-with-video-indexer) 

## <a name="getting-started-with-media-services-v3-apis"></a>Introducción a las API de Media Services v3

La API de Azure Media Services v3 permite extraer la información de los archivos de audio y vídeo mediante los [valores preestablecidos del analizador de Azure Media Services v3](../latest/analyzing-video-audio-files-concept.md). 

**AudioAnalyzerPreset** permite extraer diversa información de audio de un archivo de audio o vídeo. La salida incluye un archivo VTT o TTML para la transcripción del audio y un archivo JSON (con toda la información de audio adicional). La información de audio incluye palabras clave, indexación de hablantes y análisis de sentimiento de voz. AudioAnalyzerPreset también admite la detección de idioma para idiomas específicos. Para obtener más información, consulte [Transforms](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#audioanalyzerpreset) (Transformaciones).

### <a name="get-started"></a>Primeros pasos

Para empezar, consulte:

* [Tutorial](../latest/analyze-videos-tutorial-with-api.md)
* Ejemplos de AudioAnalyzerPreset: [SDK de Java](https://github.com/Azure-Samples/media-services-v3-java/tree/master/AudioAnalytics/AudioAnalyzer) o [SDK de .NET](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/master/AudioAnalytics/AudioAnalyzer)
* Ejemplos de VideoAnalyzerPreset: [SDK de Java](https://github.com/Azure-Samples/media-services-v3-java/tree/master/VideoAnalytics/VideoAnalyzer) o [SDK de .NET](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/master/VideoAnalytics/VideoAnalyzer)

## <a name="getting-started-with-cognitive-services-speech-services"></a>Introducción a los servicios de voz de Cognitive Services

[Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/) proporciona un servicio de voz a texto que transcribe secuencias de audio a texto en tiempo real que sus aplicaciones, herramientas o dispositivos pueden usar o mostrar. Puede usar la conversión de voz a texto para [personalizar sus propios modelos acústicos, modelos de lenguaje o modelos de pronunciación](../../cognitive-services/speech-service/how-to-custom-speech-train-model.md). Para más información, consulte [Conversión de voz a texto de Cognitive Services](../../cognitive-services/speech-service/speech-to-text.md). 

> [!NOTE] 
> El servicio de conversión de voz a texto no acepta formatos de archivo de vídeo y solo acepta [determinados formatos de audio](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-speech-to-text#audio-formats). 

Para obtener más información sobre el servicio de texto a voz y cómo empezar, consulte [¿Qué es la conversión de voz a texto?](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-to-text)

## <a name="known-differences-from-deprecated-services"></a>Diferencias conocidas de servicios en desuso 

Observará que Video Indexer, AudioAnalyzerPreset de Azure Media Services v3 y los servicios de voz de Cognitive Services son más confiables y producen salidas de mejor calidad que los procesadores retirados de Azure Media Indexer 1 y Azure Media Indexer 2.  

Entre las diferencias conocidas se incluyen las siguientes: 

* Los servicios de voz de Cognitive Services no admiten la extracción de palabras clave. Sin embargo, Video Indexer y AudioAnalyzerPreset de Media Services v3 ofrecen un conjunto sólido de palabras clave en formato de archivo JSON. 

## <a name="need-help"></a>¿Necesita ayuda?

Puede abrir una incidencia de soporte técnico si se desplaza a la [nueva solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Pasos siguientes

* [Componentes heredados](legacy-components.md)
* [Página de precios](https://azure.microsoft.com/pricing/details/media-services/#encoding)


