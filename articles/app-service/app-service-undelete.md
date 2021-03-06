---
title: Restauración de aplicaciones eliminadas
description: Aprenda a restaurar una aplicación eliminada en Azure App Service. Evite los problemas que supone eliminar una aplicación accidentalmente.
author: btardif
ms.author: byvinyal
ms.date: 9/23/2019
ms.topic: article
ms.openlocfilehash: a30ac638422f99134ebe9cc26e4b418f5de079b9
ms.sourcegitcommit: 265f1d6f3f4703daa8d0fc8a85cbd8acf0a17d30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2019
ms.locfileid: "74672144"
---
# <a name="restore-deleted-app-service-app-using-powershell"></a>Restauración de aplicaciones de App Service eliminadas con PowerShell

Si por accidente ha eliminado su aplicación de Azure App Service, puede restaurarla mediante los comandos del [módulo de PowerShell Az](https://docs.microsoft.com/powershell/azure/?view=azps-2.6.0&viewFallbackFrom=azps-2.2.0).

## <a name="re-register-app-service-resource-provider"></a>Volver a registrar el proveedor de recursos de App Service
Algunos clientes pueden encontrarse con un problema por el que no se recupera la lista de aplicaciones eliminadas. Para resolver el problema, ejecute el siguiente comando:

```powershell
 Register-AzResourceProvider -ProviderNamespace "Microsoft.Web"
```

## <a name="list-deleted-apps"></a>Mostrar una lista de aplicaciones eliminadas

Para obtener la colección de aplicaciones eliminadas, puede usar `Get-AzDeletedWebApp`.

Para más información sobre una aplicación eliminada específica, puede usar:

```powershell
Get-AzDeletedWebApp -Name <your_deleted_app> -Location <your_deleted_app_location> 
```

La información detallada incluye:

- **DeletedSiteId**: identificador único de la aplicación, que se usa con escenarios en los que se han eliminado varias aplicaciones con el mismo nombre.
- **SubscriptionID**: suscripción que contiene el recurso eliminado.
- **Ubicación**: ubicación de la aplicación original.
- **ResourceGroupName**: nombre del grupo de recursos original.
- **Nombre**: nombre de la aplicación original.
- **Ranura**: el nombre de la ranura.
- **Hora de eliminación**: cuándo se eliminó la aplicación.  

## <a name="restore-deleted-app"></a>Restauración de la aplicación eliminada

Una vez identificada la aplicación que quiere restaurar, puede hacerlo mediante `Restore-AzDeletedWebApp`.

```powershell
Restore-AzDeletedWebApp -ResourceGroupName <my_rg> -Name <my_app> -TargetAppServicePlanName <my_asp>
```

Las entradas del comando son:

- **Grupo de recursos**: grupo de recursos de destino donde se restaurará la aplicación.
- **Nombre**: nombre de la aplicación, debe ser globalmente único.
- **TargetAppServicePlanName**: plan de App Service vinculado a la aplicación.

De forma predeterminada `Restore-AzDeletedWebApp` restaurará también la configuración de la aplicación y el contenido. Si solo quiere restaurar el contenido, use la marca `-RestoreContentOnly` con este cmdlet.

> [!NOTE]
> Si la aplicación se hospedaba en una instancia de App Service Environment y después se eliminó de ella, solo se puede restaurar si la instancia de App Service Environment correspondiente sigue existiendo.
>

Aquí puede encontrar la referencia de commandlet completa: [Restore-AzDeletedWebApp](https://docs.microsoft.com/powershell/module/az.websites/restore-azdeletedwebapp).
