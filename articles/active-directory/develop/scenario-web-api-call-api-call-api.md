---
title: 'API web que llama a otras API web: Plataforma de identidad de Microsoft | Azure'
description: Aprenda a compilar una API web que llama a otras API web.
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
ms.openlocfilehash: 6063d143e2f217426bdf1db217fde46f8542d314
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2019
ms.locfileid: "74965218"
---
# <a name="web-api-that-calls-web-apis---call-an-api"></a>API web que llama a otras API web (llamada a una API)

Una vez que disponga de un token, puede llamar a una API web protegida. Esto se hace desde el controlador de la API web de ASP.NET/ASP.NET Core.

## <a name="controller-code"></a>Código del controlador

Esta es la continuación del código de ejemplo que se muestra en [API web protegida que llama a las API web: adquisición de un token](scenario-web-api-call-api-acquire-token.md), llamado en las acciones de los controladores de API, al llamar a una API de bajada (denominada todolist).

Una vez que haya adquirido el token, úselo como token de portador para llamar a la API de bajada.

```CSharp
private async Task GetTodoList(bool isAppStarting)
{
 ...
 //
 // Get an access token to call the To Do service.
 //
 AuthenticationResult result = null;
 try
 {
  app = BuildConfidentialClient(HttpContext, HttpContext.User);
  result = await app.AcquireTokenSilent(Scopes, account)
                     .ExecuteAsync()
                     .ConfigureAwait(false);
 }
...

// Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
_httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the To Do list service.
HttpResponseMessage response = await _httpClient.GetAsync(TodoListBaseAddress + "/api/todolist");
...
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Paso a producción](scenario-web-api-call-api-production.md)
