---
title: Creación de una aplicación en Cordova
description: Siga este tutorial para aprender a usar back-ends de aplicación móvil de Azure para el desarrollo de Apache Cordova.
keywords: cordova, javascript, móvil, cliente
ms.assetid: 0b08fc12-0a80-42d3-9cc1-9b3f8d3e3a3f
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: conceptual
ms.date: 06/25/2019
ms.openlocfilehash: 35670543b9716cad8165d5b5ea48f3888786f0a2
ms.sourcegitcommit: 3d4917ed58603ab59d1902c5d8388b954147fe50
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2019
ms.locfileid: "74668877"
---
# <a name="create-an-apache-cordova-app"></a>Creación de una aplicación de Apache Cordova
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

> [!NOTE]
> Visual Studio App Center admite servicios integrados de un extremo a otro fundamentales para el desarrollo de aplicaciones móviles. Los desarrolladores pueden usar los servicios de **compilación**, **prueba** y **distribución** para configurar la canalización de integración y entrega continuas. Una vez que se ha implementado la aplicación, los desarrolladores pueden supervisar el estado y el uso de su aplicación con los servicios de **análisis** y **diagnóstico**, e interactuar con los usuarios que utilizan el servicio de **Push** (inserción). Además, los desarrolladores pueden aprovechar **Auth** para autenticar a los usuarios y el servicio de **datos** para almacenar y sincronizar los datos de la aplicación en la nube.
>
> Si está pensando en integrar servicios en la nube en su aplicación para dispositivos móviles, regístrese en [App Center](https://appcenter.ms/?utm_source=zumo&utm_medium=Azure&utm_campaign=zumo%20doc) hoy mismo.

## <a name="overview"></a>Información general
En este tutorial se muestra cómo agregar un servicio back-end basado en la nube a una aplicación móvil de Apache Cordova usando un back-end de la aplicación móvil de Azure.  Creará un back-end de aplicación móvil nuevo y una aplicación simple de *lista de tareas pendientes* de Apache Cordova que almacena los datos de la aplicación en Azure.

Completar este tutorial es un requisito previo para todos los demás tutoriales de Apache Cordova acerca de cómo usar la característica Mobile Apps en Azure App Service.

## <a name="prerequisites"></a>Requisitos previos
Para completar este tutorial, debe cumplir los siguientes requisitos previos:

* Un equipo con [Visual Studio Community 2017], o cualquier versión posterior.
* [Visual Studio Tools para Apache Cordova].
* Una [cuenta de Azure activa](https://azure.microsoft.com/pricing/free-trial/).

También puede omitir Visual Studio y utilizar la línea de comandos de Apache Cordova directamente.  El uso de la línea de comandos es útil si realiza el tutorial en un equipo Mac.  Este tutorial no aborda la compilación de aplicaciones de cliente de Apache Cordova con la línea de comandos.

## <a name="create-an-azure-mobile-app-backend"></a>Creación de un nuevo back-end de Aplicaciones móviles de Azure
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Creación de una conexión de base de datos y configuración del proyecto de cliente y servidor
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-apache-cordova-app"></a>Descarga y ejecución de la aplicación Apache Cordova
[!INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/

[Visual Studio Community 2017]: https://www.visualstudio.com/
[Visual Studio Tools para Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
