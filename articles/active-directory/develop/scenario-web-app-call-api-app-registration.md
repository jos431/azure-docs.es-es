---
title: 'Registro de una aplicación web que llama a las API web: Plataforma de identidad de Microsoft | Azure'
description: Aprenda a registrar una aplicación web que llama a las API web.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 784de823e94aace6f91222c19c1ff8130c3f995f
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2019
ms.locfileid: "74962940"
---
# <a name="web-app-that-calls-web-apis---app-registration"></a>Aplicación web que llama a las API web: registro de la aplicación

Una aplicación web que llama a las API web tiene el mismo registro que una aplicación web que permite iniciar sesión a los usuarios. Por lo tanto, deberá seguir las instrucciones de [Aplicación web que llama a web API: registro de la aplicación](scenario-web-app-sign-user-app-registration.md)

Sin embargo, dado que la aplicación web ahora llama a las API web, se convierte en una aplicación cliente confidencial. Por eso se exigen algunos pasos adicionales de registro: tiene que compartir secretos (credenciales de cliente) con la Plataforma de identidad de Microsoft.

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="api-permissions"></a>Permisos de API

Las aplicaciones web llaman a las API en nombre del usuario con sesión iniciada. Tienen que solicitar permisos delegados. Para más detalles, consulte [Adición de permisos para acceder a las API web](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Configuración del código de la aplicación](scenario-web-app-call-api-app-configuration.md)
