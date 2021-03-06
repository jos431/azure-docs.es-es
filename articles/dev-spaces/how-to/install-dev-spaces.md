---
title: Instalación de Azure Dev Spaces en AKS y de las herramientas de cliente
services: azure-dev-spaces
ms.date: 07/24/2019
ms.topic: conceptual
description: Obtenga información sobre el proceso de instalación de Azure Dev Spaces en un clúster de AKS y de las herramientas de cliente.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores, Helm, service mesh, enrutamiento de service mesh, kubectl, k8s
ms.openlocfilehash: 2649b36c96313d4a7d878a1c72c3b175ad0f4d30
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2019
ms.locfileid: "74325778"
---
# <a name="install-azure-dev-spaces-on-aks-and-the-client-side-tooling"></a>Instalación de Azure Dev Spaces en AKS y de las herramientas de cliente

En este artículo se muestran varias maneras de instalar Azure Dev Spaces en un clúster de AKS, así como las herramientas de cliente.

## <a name="install-azure-dev-spaces-using-the-cli"></a>Instalación de Azure Dev Spaces con la CLI

Para poder instalar Dev Spaces mediante la CLI, necesitará lo siguiente:
* Una suscripción de Azure. Si no tiene una suscripción a Azure, puede crear una [cuenta gratuita][az-portal-create-account].
* [La CLI de Azure instalada][install-cli].
* [Un clúster de AKS][create-aks-cli] en una [región admitida][supported-regions].

Use el comando `use-dev-spaces` para habilitar Dev Spaces en el clúster de AKS y siga las indicaciones.

```cmd
az aks use-dev-spaces -g myResourceGroup -n myAKSCluster
```

El comando anterior habilita Dev Spaces en el clúster *myAKSCluster* del grupo *myResourceGroup* y crea un espacio de desarrollo *predeterminado*.

```cmd
$ az aks use-dev-spaces -g myResourceGroup -n myAKSCluster

'An Azure Dev Spaces Controller' will be created that targets resource 'myAKSCluster' in resource group 'myResourceGroup'. Continue? (y/N): y

Creating and selecting Azure Dev Spaces Controller 'myAKSCluster' in resource group 'myResourceGroup' that targets resource 'myAKSCluster' in resource group 'myResourceGroup'...2m 24s

Select a dev space or Kubernetes namespace to use as a dev space.
 [1] default
Type a number or a new name: 1

Kubernetes namespace 'default' will be configured as a dev space. This will enable Azure Dev Spaces instrumentation for new workloads in the namespace. Continue? (Y/n): Y

Configuring and selecting dev space 'default'...3s

Managed Kubernetes cluster 'myAKSCluster' in resource group 'myResourceGroup' is ready for development in dev space 'default'. Type `azds prep` to prepare a source directory for use with Azure Dev Spaces and `azds up` to run.
```

El comando `use-dev-spaces` también instala la CLI de Azure Dev Spaces.

## <a name="install-azure-dev-spaces-using-the-azure-portal"></a>Instalación de Azure Dev Spaces con Azure Portal

Para poder instalar Dev Spaces mediante Azure Portal, necesitará lo siguiente:
* Una suscripción de Azure. Si no tiene una suscripción a Azure, puede crear una [cuenta gratuita][az-portal-create-account].
* [Un clúster de AKS][create-aks-portal] en una [región admitida][supported-regions].

Para instalar Azure Dev Spaces con Azure Portal:
1. Inicie sesión en el [Azure Portal][az-portal].
1. Vaya al clúster de AKS.
1. Haga clic en *Dev Spaces*.
1. Cambie *Enable Dev Spaces* (Habilitar Dev Spaces) a *Sí* y haga clic en *Guardar*.

![Habilitar Dev Spaces en Azure Portal](../media/how-to-setup-dev-spaces/enable-dev-spaces-portal.png)

Al instalar Azure Dev Spaces con Azure Portal, **no** se instalan herramientas de cliente para Azure Dev Spaces.

## <a name="install-the-client-side-tooling"></a>Instalación de las herramientas de cliente

Puede usar las herramientas de cliente de Azure Dev Spaces para interactuar con Dev Spaces en un clúster de AKS desde la máquina local. Hay varias maneras de instalar las herramientas de cliente:

* En [Visual Studio Code][vscode], instale la [extensión de Azure Dev Spaces][vscode-extension].
* En [Visual Studio 2019][visual-studio], instale la carga de trabajo de desarrollo de Azure.
* En Visual Studio 2017, instale la carga de trabajo de desarrollo web y [Visual Studio Tools para Kubernetes][visual-studio-k8s-tools].
* Descargue e instale la CLI de [Windows][cli-win], [Mac][cli-mac] o [Linux][cli-linux].

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de la forma en que Azure Dev Spaces le ayuda a desarrollar aplicaciones más complejas en varios contenedores y cómo puede simplificar el desarrollo en colaboración mediante el uso de distintas versiones o bifurcaciones del código en distintos espacios.

> [!div class="nextstepaction"]
> [Desarrollo en equipo en Azure Dev Spaces][team-development-qs]

[create-aks-cli]: ../../aks/kubernetes-walkthrough.md#create-a-resource-group
[create-aks-portal]: ../../aks/kubernetes-walkthrough-portal.md#create-an-aks-cluster
[install-cli]: /cli/azure/install-azure-cli?view=azure-cli-latest
[supported-regions]: ../about.md#supported-regions-and-configurations
[team-development-qs]: ../quickstart-team-development.md

[az-portal]: https://portal.azure.com
[az-portal-create-account]: https://azure.microsoft.com/free
[cli-linux]: https://aka.ms/get-azds-linux
[cli-mac]: https://aka.ms/get-azds-mac
[cli-win]: https://aka.ms/get-azds-windows
[visual-studio]: https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs
[visual-studio-k8s-tools]: https://aka.ms/get-vsk8stools
[vscode]: https://code.visualstudio.com/download
[vscode-extension]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
