---
title: Eliminación y recuperación de un área de trabajo de Azure Log Analytics | Microsoft Docs
description: Obtenga información para eliminar el área de trabajo de Log Analytics si creó uno en una suscripción personal o reestructure el modelo de área de trabajo.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: MGoedtel
ms.author: magoedte
ms.date: 10/28/2019
ms.openlocfilehash: b8fdefb5e8555e90b5c9065672f4593e5bf98e06
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2019
ms.locfileid: "74326511"
---
# <a name="delete-and-restore-azure-log-analytics-workspace"></a>Eliminación y restauración de un área de trabajo de Azure Log Analytics

En este artículo se explica el concepto de eliminación temporal de áreas de trabajo de Azure Log Analytics y cómo recuperarlas. 

## <a name="considerations-when-deleting-a-workspace"></a>Consideraciones al eliminar áreas de trabajo

Al eliminar un área de trabajo de Log Analytics, se realiza una operación de eliminación temporal para que se pueda recuperar el área de trabajo, incluidos los datos y los agentes conectados en un plazo de 14 días, tanto si la eliminación fue accidental como intencionada. Después del período de eliminación temporal, el área de trabajo y sus datos no son recuperables: los datos se ponen en cola para su eliminación permanente en un plazo de 30 días, y el nombre del área de trabajo está disponible y se puede usar para crear una nueva área de trabajo.

Debe tener cuidado al eliminar un área de trabajo porque puede haber datos importantes y una configuración que puede afectar negativamente a las operaciones de servicio. Revise qué agentes, soluciones y otros servicios y orígenes de Azure almacenan sus datos en Log Analytics. Por ejemplo:

* Soluciones de administración
* Azure Automation
* Agentes que se ejecutan en máquinas virtuales Windows y Linux
* Agentes que se ejecutan en equipos Windows y Linux en su entorno
* System Center Operations Manager

La operación de eliminación temporal quita el recurso del área de trabajo y los permisos de los usuarios asociados. Si los usuarios están asociados a otras áreas de trabajo, podrán seguir utilizando Log Analytics con esas otras áreas de trabajo.

## <a name="soft-delete-behavior"></a>Comportamiento de eliminación temporal

La operación de eliminación del área de trabajo quita el recurso de Resource Manager del área de trabajo, pero su configuración y sus datos se conservan durante 14 días (y da la sensación de que el área de trabajo se ha eliminado). Los agentes y grupos de administración de System Center Operations Manager configurados para enviar notificaciones al área de trabajo continúan en un estado huérfano durante el período de eliminación temporal. El servicio proporciona además un mecanismo para recuperar el área de trabajo eliminada (incluidos sus datos y recursos conectados), básicamente, deshaciendo la eliminación.

> [!NOTE] 
> Tanto las soluciones instaladas como los servicios vinculados, como su cuenta de Azure Automation, se quitan permanentemente del área de trabajo en el momento de la eliminación y no se pueden recuperar. Deben volver a configurarse después de la operación de recuperación para que el área de trabajo vuelva al estado en que estaba configurado anteriormente.

Las áreas de trabajo se pueden eliminar un mediante [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/remove-azurermoperationalinsightsworkspace?view=azurermps-6.13.0), [API REST](https://docs.microsoft.com/rest/api/loganalytics/workspaces/delete) o en [Azure Portal](https://portal.azure.com).

### <a name="delete-workspace-in-azure-portal"></a>Eliminación de áreas de trabajo en Azure Portal

1. Para iniciar sesión, vaya a [Azure Portal](https://portal.azure.com). 
2. En Azure Portal, seleccione **Todos los servicios**. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Áreas de trabajo de Log Analytics**.
3. En la lista de áreas de trabajo de Log Analytics, seleccione un área de trabajo y, a continuación, haga clic en **Eliminar** en la parte superior del panel intermedio.
   ![Opción Eliminar desde el panel de propiedades del área de trabajo](media/delete-workspace/log-analytics-delete-workspace.png)
4. Cuando aparezca de la ventana de mensaje de confirmación que le solicite que confirme la eliminación del área de trabajo, haga clic en **Sí**.
   ![Confirmación de la eliminación del área de trabajo](media/delete-workspace/log-analytics-delete-workspace-confirm.png)

## <a name="recover-workspace"></a>Recuperación de áreas de trabajo

Si tiene permisos de colaborador en la suscripción y en el grupo de recursos a los que estaba asociada el área de trabajo antes de la operación de eliminación temporal, puede recuperarla durante el periodo de dicha eliminación, incluidos sus datos, configuración y agentes conectados. Después del período de eliminación temporal, el área de trabajo no podrá recuperarse y se pondrá en cola para su eliminación permanente. Los nombres de las áreas de trabajo eliminadas se conservan durante el periodo de eliminación temporal y no se pueden usar al intentar crear un área de trabajo nueva.  

Para recuperar un área de trabajo, puede volver a crearla con los siguientes métodos de creación de áreas de trabajo: [PowerShell](https://docs.microsoft.com/powershell/module/az.operationalinsights/New-AzOperationalInsightsWorkspace) o [API REST]( https://docs.microsoft.com/rest/api/loganalytics/workspaces/createorupdate), siempre que se rellenen las siguientes propiedades con los detalles del área de trabajo eliminada:

* Id. de suscripción
* Nombre del grupo de recursos
* Nombre del área de trabajo
* Region

El área de trabajo y todos sus datos se restauran después de la operación de recuperación. Las soluciones y los servicios vinculados se quitaron permanentemente del área de trabajo cuando se eliminó y se deben volver a configurar para restaurarla a su estado configurado anterior. Puede que algunos de los datos no estén disponibles para la consulta después de la recuperación del área de trabajo hasta que se vuelvan a instalar las soluciones asociadas y se agreguen sus esquemas al área de trabajo.

> [!NOTE]
> * Desde [Azure Portal](https://portal.azure.com) no se pueden recuperar áreas de trabajo. 
> * Al volver a crear un área de trabajo durante el período de eliminación temporal, se indica que este nombre de área de trabajo ya está en uso. 
> 
