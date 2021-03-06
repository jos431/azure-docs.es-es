---
title: Agregar o cambiar los administradores de la suscripción de Azure
description: Describe cómo agregar o cambiar un administrador de suscripción de Azure mediante control de acceso basado en roles (RBAC).
author: genlin
manager: dcscontentpm
tags: billing
ms.service: cost-management-billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: banders
ms.openlocfilehash: b6a269b1b267cb88c3c0cae42cffb3ba9684e291
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74224129"
---
# <a name="add-or-change-azure-subscription-administrators"></a>Agregar o cambiar los administradores de la suscripción de Azure


Para administrar el acceso a recursos de Azure, debe tener el rol de administrador adecuado. Azure tiene un sistema de autorización denominado [control de acceso basado en rol](../role-based-access-control/overview.md) (RBAC) con varios roles integrados para elegir. Puede asignar estos roles en distintos ámbitos, como grupo de administración, suscripción o grupo de recursos. De forma predeterminada, la persona que crea una nueva suscripción de Azure puede asignar a otros usuarios acceso administrativo a una suscripción.

En este artículo, se describe cómo agregar o cambiar el rol de administrador para un usuario que usa RBAC en el ámbito de la suscripción.

Microsoft recomienda que administre el acceso a los recursos mediante RBAC. No obstante, si todavía usa el modelo de implementación clásico y administra los recursos clásicos mediante el [módulo de PowerShell de Azure Service Management](https://docs.microsoft.com/powershell/module/servicemanagement/azure), necesitará usar un administrador clásico.

> [!TIP]
> Si solo usa Azure Portal para administrar los recursos clásicos, no será preciso que utilice el administrador clásico.

Para más información, consulte [Implementación mediante Azure Resource Manager frente al modelo clásico](../azure-resource-manager/resource-manager-deployment-model.md) y [Azure classic subscription administrators](../role-based-access-control/classic-administrators.md) (Administradores de la suscripción clásica de Azure).

<a name="add-an-admin-for-a-subscription"></a>

## <a name="assign-a-subscription-administrator"></a>Asignación de un administrador de suscripción

Para que un usuario sea administrador de una suscripción a Azure, un administrador existente le asigna el rol [Propietario](../role-based-access-control/built-in-roles.md#owner) (un rol RBAC) en el ámbito de la suscripción. El rol Propietario da al usuario acceso completo a todos los recursos de la suscripción, incluido el derecho a delegar este acceso a otros. Estos pasos son los mismos que para la asignación de cualquier otro rol.

Si no está seguro de quién es el administrador de cuenta de la suscripción, use los pasos siguientes para averiguarlo.

1. Abra la [página Suscripciones en Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Seleccione la suscripción que desee comprobar y, luego, consulte **Configuración**.
1. Seleccione **Propiedades**. El administrador de cuentas de la suscripción se muestra en el cuadro **Administrador de cuentas** .

### <a name="to-assign-a-user-as-an-administrator"></a>Para asignar un usuario como administrador

1. Inicie sesión en Azure Portal como administrador de la suscripción y abra [Suscripciones](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

1. Haga clic en la suscripción a la que quiere conceder acceso.

1. Haga clic en **Control de acceso (IAM)** .

1. Haga clic en la pestaña **Asignaciones de roles** para ver todas las asignaciones de roles para esta suscripción.

    ![Captura de pantalla que muestra las asignaciones de roles](./media/billing-add-change-azure-subscription-administrator/role-assignments.png)

1. Haga clic en **Agregar** > **Agregar asignación de roles** para abrir el panel **Agregar asignación de roles**.

    Si no tiene permisos para asignar roles, la opción se deshabilitará.

1. En la lista desplegable **Rol**, seleccione el rol **Propietario**.

1. En la lista **Seleccionar**, seleccione un usuario. Si no ve el usuario en la lista, puede escribir en el cuadro **Seleccionar** para buscar en el directorio por nombres para mostrar y direcciones de correo electrónico.

    ![Captura de pantalla que muestra el rol de propietario seleccionado](./media/billing-add-change-azure-subscription-administrator/add-role.png)

1. Haga clic en **Guardar** para asignar el rol.

    Transcurridos unos instantes, al usuario se le asigna el rol Propietario en el ámbito de la suscripción.

## <a name="next-steps"></a>Pasos siguientes

* [¿Qué es el control de acceso basado en rol (RBAC)?](../role-based-access-control/overview.md)
* [Descripción de los distintos roles en Azure](../role-based-access-control/rbac-and-directory-admin-roles.md)
* [Uso de Asociar o agregar una suscripción de Azure a Azure Active Directory](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)
* [Permisos de roles de administrador en Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md)

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si sigue necesitando ayuda, [póngase en contacto con el servicio de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.
