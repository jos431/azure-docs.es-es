---
title: Reentrada de actores de Azure Service Fabric | Microsoft Docs
description: Introducción a la reentrada de Reliable Actors de Service Fabric.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: amanbha
ms.assetid: be23464a-0eea-4eca-ae5a-2e1b650d365e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 46682787bac2d60d188384a4078ca2fa1f46ae7a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60725421"
---
# <a name="reliable-actors-reentrancy"></a>Reentrada de Reliable Actors
El runtime de Reliable Actors permite, de manera predeterminada, la reentrada basada en el contexto de llamadas lógicas. Esto permite que los actores sean reentrantes si están en la misma cadena de contexto de llamada. Por ejemplo, si el actor A envía un mensaje al actor B, quien envía el mensaje al actor C; como parte del procesamiento del mensaje, si el actor C llama al actor A, el mensaje es reentrante y, por los tanto, se permitirá. Los demás mensajes que formen parte de un contexto de llamada distinto se bloquearán en el actor A hasta que complete el procesamiento.

Existen dos opciones disponibles para la reentrada de actores definidas en la enumeración `ActorReentrancyMode` :

* `LogicalCallContext` (comportamiento predeterminado)
* `Disallowed` : deshabilita la reentrada.

```csharp
public enum ActorReentrancyMode
{
    LogicalCallContext = 1,
    Disallowed = 2
}
```
```Java
public enum ActorReentrancyMode
{
    LogicalCallContext(1),
    Disallowed(2)
}
```
Se puede establecer la reentrada en la configuración de una clase `ActorService`durante el registro. La configuración se aplica a todas las instancias de actor que creó en el servicio de actor.

En el ejemplo siguiente se muestra un servicio de actor que establece el modo de reentrada en `ActorReentrancyMode.Disallowed`. En este caso, si un actor envía un mensaje reentrante a otro actor, se generará una excepción de tipo `FabricException` .

```csharp
static class Program
{
    static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<Actor1>(
                (context, actorType) => new ActorService(
                    context,
                    actorType, () => new Actor1(),
                    settings: new ActorServiceSettings()
                    {
                        ActorConcurrencySettings = new ActorConcurrencySettings()
                        {
                            ReentrancyMode = ActorReentrancyMode.Disallowed
                        }
                    }))
                .GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}
```
```Java
static class Program
{
    static void Main()
    {
        try
        {
            ActorConcurrencySettings actorConcurrencySettings = new ActorConcurrencySettings();
            actorConcurrencySettings.setReentrancyMode(ActorReentrancyMode.Disallowed);

            ActorServiceSettings actorServiceSettings = new ActorServiceSettings();
            actorServiceSettings.setActorConcurrencySettings(actorConcurrencySettings);

            ActorRuntime.registerActorAsync(
                Actor1.getClass(),
                (context, actorType) -> new FabricActorService(
                    context,
                    actorType, () -> new Actor1(),
                    null,
                    stateProvider,
                    actorServiceSettings, timeout);

            Thread.sleep(Long.MAX_VALUE);
        }
        catch (Exception e)
        {
            throw e;
        }
    }
}
```


## <a name="next-steps"></a>Pasos siguientes
* Aprenda más sobre la reentrada en la [documentación de referencia de Actor API](https://msdn.microsoft.com/library/azure/dn971626.aspx)
