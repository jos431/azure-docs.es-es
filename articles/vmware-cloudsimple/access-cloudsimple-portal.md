---
title: 'Acceso a la solución de VMware en Azure de CloudSimple: Portal'
description: Descripción sobre cómo acceder al portal de la solución de VMware de CloudSimple desde Azure Portal
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/04/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 95d27bab243f6805436465f5027e519d33e20f6f
ms.sourcegitcommit: 39d95a11d5937364ca0b01d8ba099752c4128827
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69563224"
---
# <a name="access-the-vmware-solution-by-cloudsimple-portal-from-the-azure-portal"></a>Acceso a la solución de VMware mediante el portal de CloudSimple desde Azure Portal

El inicio de sesión único se admite para acceder al portal de CloudSimple. Después de iniciar sesión en Azure Portal, puede acceder al portal de CloudSimple sin volver a iniciar sesión. La primera vez que acceda al portal de CloudSimple, se le solicitará que autorice la aplicación [Autorización del servicio de CloudSimple](#consent-to-cloudsimple-service-authorization-application).  La autorización es una acción que solo se realiza una vez.

## <a name="before-you-begin"></a>Antes de empezar

Los usuarios con los roles **Propietario** y **Colaborador** integrados pueden acceder al portal de CloudSimple.  Los roles deben configurarse en el grupo de recursos donde está implementado el servicio CloudSimple.  Los roles también pueden configurarse en el objeto de servicio CloudSimple.  Para obtener más información sobre la comprobación de su rol, consulte el artículo [Visualización de asignaciones de roles](https://docs.microsoft.com/azure/role-based-access-control/check-access). Solo los usuarios con los roles **Propietario** y **Colaborador** integrados pueden acceder al portal de CloudSimple.  Los roles deben configurarse en la suscripción.  Para obtener más información sobre la comprobación de su rol, consulte el artículo [Visualización de asignaciones de roles](https://docs.microsoft.com/azure/role-based-access-control/check-access).

Si utiliza roles personalizados, el rol debe tener cualquiera de las operaciones que se especifican debajo de ```Actions```.  Para obtener información acerca de los roles personalizados, consulte [Roles personalizados en los recursos de Azure](https://docs.microsoft.com/azure/role-based-access-control/custom-roles).  Si alguna de las operaciones es una parte de ```NotActions```, el usuario no puede acceder al portal de CloudSimple.

```
Microsoft.VMwareCloudSimple/*
Microsoft.VMwareCloudSimple/*/write
Microsoft.VMwareCloudSimple/dedicatedCloudServices/*
Microsoft.VMwareCloudSimple/dedicatedCloudServices/*/write
```

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="access-the-cloudsimple-portal"></a>Acceso al portal de CloudSimple

1. Seleccione **Todos los servicios**.

2. Busque **Servicios de CloudSimple**.

3. Seleccione el servicio de CloudSimple en el que quiere crear la nube privada.

4. En la página **Información general**, haga clic en **Go to the CloudSimple portal** (Ir al portal de CloudSimple).  Si accede al portal de CloudSimple desde Azure Portal por primera vez, se le solicitará que autorice la aplicación [Autorización del servicio de CloudSimple](#consent-to-cloudsimple-service-authorization-application). 

    ![Inicio del portal de CloudSimple](media/launch-cloudsimple-portal.png)

> [!NOTE]
> Si selecciona una operación de nube privada (como la creación o expansión de una nube privada) directamente desde Azure Portal, el portal de CloudSimple se abre en la página indicada.

En el portal de CloudSimple, seleccione **Inicio** en el menú lateral para mostrar información de resumen sobre sus nubes privadas. Se muestran los recursos y la capacidad de sus nubes privadas, junto con las alertas y las tareas que requieren atención. Para las tareas comunes, haga clic en los iconos con nombre en la parte superior de la página.

![Página principal](media/cloudsimple-portal-home.png)

## <a name="consent-to-cloudsimple-service-authorization-application"></a>Consentimiento para la aplicación Autorización del servicio de CloudSimple

Al iniciar el portal de CloudSimple desde Azure Portal por primera vez, se requiere su consentimiento para la aplicación Autorización del servicio de CloudSimple.  Seleccione **Aceptar** para conceder los permisos solicitados y acceder al portal de CloudSimple.

![Consentimiento para Autorización del servicio de CloudSimple: administradores](media/cloudsimple-azure-consent.png)

Si tiene el privilegio de administrador global, puede dar el consentimiento para su organización.  Seleccione **Consentimiento en nombre de la organización**.

![Consentimiento para Autorización del servicio de CloudSimple: administrador global](media/cloudsimple-azure-consent-global-admin.png)

Si sus permisos no permiten acceder al portal de CloudSimple, póngase en contacto con el administrador global del inquilino para que le conceda los permisos necesarios.  Un administrador global puede dar consentimiento en nombre de la organización.

![Consentimiento para Autorización del servicio de CloudSimple: requiere administradores](media/cloudsimple-azure-consent-requires-administrator.png)

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre cómo [crear una nube privada](https://docs.azure.cloudsimple.com/create-private-cloud/)
* Más información sobre cómo [configurar un entorno de nube privada](quickstart-create-private-cloud.md)
