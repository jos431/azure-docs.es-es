---
title: Configuración de datos en directo (versión preliminar) de Azure Monitor para contenedores | Microsoft Docs
description: En este artículo se describe cómo configurar la vista en tiempo real de los registros de contenedor (stdout/stderr) y eventos sin usar kubectl con Azure Monitor para contenedores.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/16/2019
ms.author: magoedte
ms.openlocfilehash: 596c5ad378d471c6c98616a48f44e96c365ee0bb
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510757"
---
# <a name="how-to-setup-the-live-data-preview-feature"></a>Cómo configurar la característica de datos en directo (versión preliminar)

Para ver los datos en directo (versión preliminar) con Azure Monitor para contenedores de clústeres de Azure Kubernetes Service (AKS), debe configurar la autenticación para conceder permiso de acceso a los datos de Kubernetes. Esta configuración de seguridad permite el acceso en tiempo real a los datos a través de la API de Kubernetes directamente en el Azure Portal.

Esta característica admite tres métodos diferentes para controlar el acceso a los registros, eventos y métricas:

- AKS sin autorización RBAC de Kubernetes habilitada
- AKS habilitado con autorización de RBAC de Kubernetes
- AKS habilitado con inicio de sesión único basado en SAML de Azure Active Directory (AD)

Estas instrucciones requieren acceso administrativo al clúster de Kubernetes y, si se configura para usar Azure Active Directory (AD) para la autenticación de usuario, el acceso administrativo a Azure AD.  

En este artículo se explica cómo configurar la autenticación para controlar el acceso a la característica datos en directo (versión preliminar) del clúster:

- Clúster AKS habilitado para el control de acceso basado en roles (RBAC)
- Azure Active Directory integrado en el clúster AKS. 

>[!NOTE]
>No se admiten los clústeres de AKS habilitados como [clústeres privados](https://azure.microsoft.com/updates/aks-private-cluster/) con esta característica. Esta característica se basa en el acceso directo a la API de Kubernetes a través de un servidor proxy desde el explorador. La habilitación de la seguridad de red para bloquear la API de Kubernetes desde este proxy bloqueará este tráfico. 

>[!NOTE]
>Esta característica está disponible en todas las regiones de Azure, incluida Azure China. Actualmente no está disponible en Azure Gobierno de EE. UU.

## <a name="authentication-model"></a>Modelo de autenticación

La característica de datos en directo (versión preliminar) usa la API de Kubernetes, idéntica a la herramienta de línea de comandos de `kubectl`. Los puntos de conexión de la API de Kubernetes usan un certificado autofirmado, que no podrá validar el explorador. Esta característica emplea un proxy interno para validar el certificado con el servicio AKS, asegurándose de que se confía en el tráfico.

En el Azure Portal se le pedirá que valide las credenciales de inicio de sesión para un clúster de Azure Active Directory y le redirigirá a la configuración de registro del cliente durante la creación del clúster (y se volverá a configurar en este artículo). Este comportamiento es similar al proceso de autenticación que requiere `kubectl`. 

>[!NOTE]
>La autorización para el clúster se administra mediante Kubernetes y el modelo de seguridad con el que está configurado. Los usuarios que tienen acceso a esta característica requieren permiso para descargar la configuración de Kubernetes (*kubeconfig*), similar a la ejecución de `az aks get-credentials -n {your cluster name} -g {your resource group}`. Este archivo de configuración contiene el token de autorización y autenticación para **rol de usuario de clúster de Azure Kubernetes Service**, en el caso de los clústeres habilitados para RBAC de Azure y AKS sin la autorización RBAC habilitada. Contiene información sobre Azure AD y detalles de registro del cliente cuando AKS se habilita con el inicio de sesión único basado en SAML de Azure Active Directory (AD).

>[!IMPORTANT]
>Los usuarios de estas características requieren [rol de usuario de clúster de Azure Kubernetes](../../azure/role-based-access-control/built-in-roles.md#azure-kubernetes-service-cluster-user-role permissions) al clúster para descargar el `kubeconfig` y usar esta característica. Los usuarios **no**  necesitan el acceso de colaborador al clúster para utilizar esta característica. 

## <a name="kubernetes-cluster-without-rbac-enabled"></a>Clúster de Kubernetes sin RBAC habilitado

Si tiene un clúster de Kubernetes que no está configurado con autorización de RBAC de Kubernetes ni integrado en el inicio de sesión único de Azure AD, no es necesario seguir estos pasos. Esto se debe a que tiene permisos administrativos de forma predeterminada en una configuración no RBAC.

## <a name="configure-kubernetes-rbac-authentication"></a>Configuración de la autenticación RBAC de Kubernetes

Cuando habilita la autorización RBAC de Kubernetes, se utilizan dos usuarios: **clusterUser** y **clusterAdmin** para acceder a la API de Kubernetes. Esto es similar a ejecutar `az aks get-credentials -n {cluster_name} -g {rg_name}` sin la opción administrativa. Esto significa que el **clusterUser** debe tener acceso a los puntos de conexión de la API de Kubernetes.

Los pasos de ejemplo siguientes muestran cómo configurar el enlace de rol de clúster desde esta plantilla de configuración de yaml.

1. Copie el archivo yaml, péguelo y guárdelo como LogReaderRBAC.yaml.  

    ```
    apiVersion: rbac.authorization.k8s.io/v1 
    kind: ClusterRole 
    metadata: 
       name: containerHealth-log-reader 
    rules: 
        - apiGroups: ["", "metrics.k8s.io", "extensions", "apps"] 
          resources: 
             - "pods/log" 
             - "events" 
             - "nodes" 
             - "pods" 
             - "deployments" 
             - "replicasets" 
          verbs: ["get", "list"] 
    --- 
    apiVersion: rbac.authorization.k8s.io/v1 
    kind: ClusterRoleBinding 
    metadata: 
       name: containerHealth-read-logs-global 
    roleRef: 
       kind: ClusterRole 
       name: containerHealth-log-reader 
       apiGroup: rbac.authorization.k8s.io 
    subjects: 
    - kind: User 
      name: clusterUser 
      apiGroup: rbac.authorization.k8s.io 
    ```

2. Para actualizar la configuración, ejecute el comando siguiente: `kubectl apply -f LogReaderRBAC.yaml`.

>[!NOTE] 
> Si ha aplicado una versión anterior del archivo `LogReaderRBAC.yaml` al clúster, actualícelo copiando y pegando el nuevo código que se muestra en el paso 1 de arriba y luego ejecute el comando que se muestra en el paso 2 para aplicarlo al clúster.

## <a name="configure-ad-integrated-authentication"></a>Configuración de la autenticación integrada de AD 

Un clúster de AKS configurado para usar Azure Active Directory (AD) para la autenticación de usuario usa las credenciales de inicio de sesión de la persona que tiene acceso a esta característica. En esta configuración, puede iniciar sesión en un clúster de AKS mediante su token de autenticación de Azure AD.

El registro del cliente de Azure AD debe volver a configurarse para permitir que el Azure Portal redirija las páginas de autorización como una dirección URL de redireccionamiento de confianza. A los usuarios de Azure AD se les concede acceso directamente a los mismos puntos de conexión de la API de Kubernetes a través de **ClusterRoles** y **ClusterRoleBindings**. 

Para más información sobre la configuración de seguridad avanzada en Kubernetes, revise la [documentación de Kubernetes](https://kubernetes.io/docs/reference/access-authn-authz/rbac/). 

>[!NOTE]
>Si va a crear un nuevo clúster habilitado para RBAC, consulte [Integración de Azure Active Directory con Azure Kubernetes Service](../../aks/azure-ad-integration.md) y siga los pasos para configurar la autenticación de Azure AD. Durante los pasos para crear la aplicación cliente, una nota en esa sección resalta las dos URL de redireccionamiento que necesita crear para Azure Monitor para contenedores. 

### <a name="client-registration-reconfiguration"></a>Reconfiguración del registro de cliente

1. Busque el registro de cliente de su clúster de Kubernetes en Azure AD en **Azure Active Directory > Registros de aplicaciones** en el Azure Portal.

2. Seleccione **Authentication** en el menú izquierdo. 

3. Agregue dos URL de redireccionamiento a esta lista como tipos de aplicación **Web**. El primer valor de URL base debe ser `https://afd.hosting.portal.azure.net/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` y el segundo, `https://monitoring.hosting.portal.azure.net/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html`.

    >[!NOTE]
    >Si usa esta característica en Azure China, el primer valor de dirección URL base debe ser `https://afd.hosting.azureportal.chinaloudapi.cn/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` y el segundo, `https://monitoring.hosting.azureportal.chinaloudapi.cn/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html`. 
    
4. Después de registrar las direcciones URL de redireccionamiento, en **Configuración avanzada**, seleccione las opciones **Tokens de acceso** y **Tokens de identificador** y luego guarde los cambios.

>[!NOTE]
>La configuración de la autenticación con Azure Active Directory para el inicio de sesión único solo puede lograrse durante la implementación inicial de un nuevo clúster de AKS. No puede configurar el inicio de sesión único en un clúster de AKS ya implementado.
  
>[!IMPORTANT]
>Si se vuelve a configurar Azure AD para la autenticación de usuarios con el URI actualizado, borre la caché del explorador para garantizar que se descarga y aplica el token de autenticación actualizado.

## <a name="grant-permission"></a>Concesión de permiso

A cada cuenta de Azure AD se le debe conceder permiso para las API adecuadas en Kubernetes, de modo que tengan acceso a la característica datos en directo (versión preliminar). Los pasos para conceder la cuenta Azure Active Directory son similares a los pasos descritos en la sección [Autenticación RBAC de Kubernetes](#configure-kubernetes-rbac-authentication). Antes de aplicar la plantilla de configuración YAML al clúster, reemplace **clusterUser** en **ClusterRoleBinding** por el usuario deseado. 

>[!IMPORTANT]
>Si el usuario al que se le concede el enlace de RBAC se encuentra en el mismo inquilino de Azure AD, asigne permisos según el userPrincipalName. Si el usuario se encuentra en un inquilino distinto de Azure AD, en su lugar, consulte y use la propiedad objectId.

Para obtener ayuda adicional para configurar el clúster de AKS **ClusterRoleBinding**, consulte [Creación del enlace de RBAC](../../aks/azure-ad-integration-cli.md#create-rbac-binding).

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha configurado la autenticación, puede ver [métricas](container-insights-livedata-metrics.md), [implementaciones](container-insights-livedata-deployments.md) y [eventos y registros](container-insights-livedata-overview.md) en tiempo real desde el clúster.