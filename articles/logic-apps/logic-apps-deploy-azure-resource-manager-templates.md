---
title: Implementación de plantillas de aplicaciones lógicas
description: Obtenga información sobre cómo implementar plantillas de Azure Resource Manager creadas para Azure Logic Apps.
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/01/2019
ms.openlocfilehash: 73c4c8839bd1d9e37f3dc4f0802ad5a648b1026b
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74793053"
---
# <a name="deploy-azure-resource-manager-templates-for-azure-logic-apps"></a>Implementación de plantillas de Azure Resource Manager para Azure Logic Apps

Después de crear una plantilla de Azure Resource Manager para la aplicación lógica, puede implementar la plantilla de las siguientes maneras:

* [Azure Portal](#portal)
* [Visual Studio](#visual-studio)
* [Azure PowerShell](#powershell)
* [CLI de Azure](#cli)
* [API de REST del Administrador de recursos de Azure](../azure-resource-manager/resource-group-template-deploy-rest.md)
* [Azure Pipelines para Azure DevOps](#azure-pipelines)

<a name="portal"></a>

## <a name="deploy-through-azure-portal"></a>Implementación a través de Azure Portal

Para implementar automáticamente una plantilla de aplicación lógica en Azure, puede elegir el siguiente botón **Implementar en Azure**, que inicia sesión en Azure Portal y le pide información sobre la aplicación lógica. A continuación, haga los cambios necesarios en la plantilla o los parámetros de la aplicación lógica.

[![Implementación en Azure](./media/logic-apps-deploy-azure-resource-manager-templates/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

Por ejemplo, se le pedirá la siguiente información después de iniciar sesión en Azure Portal:

* El nombre de la suscripción a Azure
* El grupo de recursos que desea usar
* La ubicación de la aplicación lógica
* Nombre de la aplicación lógica
* Un URI de prueba
* La aceptación de los términos y condiciones especificados

Para más información, consulte los temas siguientes:

* [Información general: Implementación automatizada de aplicaciones lógicas con plantillas de Azure Resource Manager](logic-apps-azure-resource-manager-templates-overview.md)
* [Implementación de recursos con las plantillas de Resource Manager y Azure Portal](../azure-resource-manager/resource-group-template-deploy-portal.md)

<a name="visual-studio"></a>

## <a name="deploy-with-visual-studio"></a>Implementación con Visual Studio

Para implementar una plantilla de aplicación lógica desde un proyecto de grupo de recursos de Azure creado con Visual Studio, siga estos [pasos para implementar manualmente la aplicación lógica](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#deploy-logic-app-to-azure) en Azure.

<a name="powershell"></a>

## <a name="deploy-with-azure-powershell"></a>Implementación con Azure PowerShell

Para implementar en un determinado *grupo de recursos de Azure*, use el siguiente comando:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName <Azure-resource-group-name> -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json
```

Para más información, consulte los temas siguientes:

* [Implementación de recursos con las plantillas de Resource Manager y Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)
* [`New-AzResourceGroupDeployment`](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment)

<a name="cli"></a>

## <a name="deploy-with-azure-cli"></a>Implementación con la CLI de Azure

Para implementar en un determinado *grupo de recursos de Azure*, use el siguiente comando:

```azurecli
az group deployment create -g <Azure-resource-group-name> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json
```

Para más información, consulte los temas siguientes:

* [Implementación de recursos con plantillas de Resource Manager y la CLI de Azure](../azure-resource-manager/resource-group-template-deploy-cli.md) 
* [`az group deployment create`](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)

<a name="azure-pipelines"></a>

## <a name="deploy-with-azure-devops"></a>Implementación con Azure DevOps

Para implementar las plantillas de aplicación lógica y administrar entornos, los equipos suelen usan una herramienta como [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines) en [Azure DevOps](https://docs.microsoft.com/azure/devops/user-guide/what-is-azure-devops-services). Azure Pipelines proporciona una [tarea Implementación de un grupo de recursos de Azure](https://github.com/Microsoft/azure-pipelines-tasks/tree/master/Tasks/AzureResourceGroupDeploymentV2), que se puede agregar a cualquier canalización de la compilación o versión. Para que la autorización implemente y genere la canalización de versión, también necesita una [entidad de servicio](../active-directory/develop/app-objects-and-service-principals.md) de Azure Active Directory. Obtenga más información sobre [el uso de entidades de servicio con Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/library/connect-to-azure).

Para más información acerca de la integración continua e implementación continua (CI/CD) para plantillas de Azure Resource Manager con Azure Pipelines, consulte estos temas y ejemplos:

* [Integración de plantillas de Resource Manager con Azure Pipelines](../azure-resource-manager/vs-resource-groups-project-devops-pipelines.md)
* [Tutorial: Integración continua de plantillas de Azure Resource Manager en Azure Pipelines](../azure-resource-manager/resource-manager-tutorial-use-azure-pipelines.md).
* [Ejemplo: Conexión a colas de Azure Service Bus desde Azure Logic Apps e implementación con Azure Pipelines en Azure DevOps](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-deployment-samples/connect-to-azure-service-bus-queues-from-azure-logic-apps-and-deploy-with-azure-devops-pipelines/)
* [Ejemplo: Conexión a cuentas de Azure Storage desde Azure Logic Apps e implementación con Azure Pipelines en Azure DevOps](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-deployment-samples/connect-to-azure-storage-accounts-from-azure-logic-apps-and-deploy-with-azure-devops-pipelines/)
* [Ejemplo: Conexión a una función de aplicación de funciones para Azure Logic Apps e implementación con Azure Pipelines en Azure DevOps](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-deployment-samples/set-up-an-azure-function-app-action-for-azure-logic-apps-and-deploy-with-azure-devops-pipelines/)
* [Ejemplo: Conexión a una cuenta de integración desde Azure Logic Apps e implementación con Azure Pipelines en Azure DevOps](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-deployment-samples/connect-to-an-integration-account-from-azure-logic-apps-and-deploy-by-using-azure-devops-pipelines/)

Aquí puede encontrar los pasos generales de alto nivel para usar Azure Pipelines:

1. En Azure Pipelines, cree una canalización vacía.

1. Elija los recursos que necesita para la canalización, como la plantilla de aplicación lógica y los archivos de parámetros de plantilla, que genera manualmente o como parte del proceso de compilación.

1. Para el trabajo del agente, busque y agregue la tarea de **implementación del grupo de recursos de Azure**.

   ![Incorporación de la tarea "Implementación de un grupo de recursos de Azure"](./media/logic-apps-deploy-azure-resource-manager-templates/add-azure-resource-group-deployment-task.png)

1. Configúrelo con una [entidad de servicio](https://docs.microsoft.com/azure/devops/pipelines/library/connect-to-azure).

1. Agregue referencias a la plantilla de la aplicación lógica y los archivos de parámetros de la plantilla.

1. Siga compilando los pasos del proceso de lanzamiento de otros entornos, pruebas automatizadas o aprobadores según sea necesario.

<a name="authorize-oauth-connections"></a>

## <a name="authorize-oauth-connections"></a>Autorización de conexiones de OAuth

Después de la implementación, la aplicación lógica funciona de un extremo a otro con parámetros válidos. Aun así, debe autorizar las conexiones de OAuth para generar tokens de acceso válidos para [autenticar las credenciales](../active-directory/develop/authentication-scenarios.md). A continuación se indican las formas en las que puede autorizar las conexiones de OAuth:

* Para las implementaciones automatizadas, puede usar un script que dé consentimiento a cada conexión de OAuth. Hay un script de ejemplo en GitHub, en el proyecto [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth).

* Para autorizar manualmente las conexiones de OAuth, abra la aplicación lógica en el Diseñador de Logic Apps, ya sea en Azure Portal o en Visual Studio. En el diseñador, autorice las conexiones necesarias.

Si usa una [entidad de servicio](../active-directory/develop/app-objects-and-service-principals.md) de Azure Active Directory (Azure AD) en lugar de autorizar las conexiones, aprenda a [especificar parámetros de entidad de servicio en la plantilla de aplicación lógica](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md#authenticate-connections).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Supervisión de aplicaciones lógicas](../logic-apps/logic-apps-monitor-your-logic-apps.md)
