---
title: Creación de directivas de seguridad personalizadas en Azure Security Center | Microsoft Docs
description: Definiciones de directivas personalizadas de Azure supervisadas en Azure Security Center.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 3e60c0e165b8f9ec79aac2048011d9e315b0ce18
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74483972"
---
# <a name="using-custom-security-policies-preview"></a>Uso de directivas de seguridad personalizadas (versión preliminar)

Para ayudarle a proteger los sistemas y el entorno, Azure Security Center genera recomendaciones de seguridad. Estas recomendaciones se basan en los procedimientos recomendados del sector, que se incorporan a la directiva de seguridad predeterminada genérica que se proporciona a todos los clientes. Estas recomendaciones también pueden provenir de los conocimientos de Security Center acerca del sector y de los estándares normativos.

Gracias a esta característica de vista previa, puede agregar sus propias iniciativas *personalizadas*. A continuación, recibirá recomendaciones si el entorno no sigue las directivas que creó.

Como se explicó [aquí](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure#definition-location) en la documentación de Azure Policy, cuando se especifica una ubicación para la iniciativa personalizada, debe ser un grupo de administración o una suscripción. 

## <a name="to-add-a-custom-initiative-to-your-subscription"></a>Agregar una iniciativa personalizada a la suscripción 

1. En la barra lateral de Security Center, abra la página **Directiva de seguridad**.

1. Seleccione una suscripción o un grupo de administración al que quiera agregar una iniciativa personalizada.

    [![Seleccione la suscripción para la que se creará la directiva personalizada](media/custom-security-policies/custom-policy-selecting-a-subscription.png)](media/custom-security-policies/custom-policy-selecting-a-subscription.png#lightbox)

    > [!NOTE]
    > Debe agregar estándares personalizados en el nivel de suscripción (o superior) para que se evalúen y se muestren en Security Center. 
    >
    > Cuando se agrega un estándar personalizado, este asigna una *iniciativa* a ese ámbito. Por lo tanto, se recomienda seleccionar el ámbito más amplio necesario para esa asignación.

1. En la página de la directiva de seguridad, en las iniciativas personalizadas (versión preliminar), haga clic en **Agregar una iniciativa personalizada**.

    [![Haga clic en **Agregar una iniciativa personalizada**](media/custom-security-policies/custom-policy-add-initiative.png)](media/custom-security-policies/custom-policy-add-initiative.png#lightbox)

    Aparece la siguiente página:

    ![Crear o agregar una directiva](media/custom-security-policies/create-or-add-custom-policy.png)

1. En la página para agregar iniciativas personalizadas, revise la lista de directivas personalizadas ya creadas en la organización. Si ve una que quiera asignar a la suscripción, haga clic en **Agregar**. Si no hay una iniciativa en la lista que satisfaga sus necesidades, omita este paso.

1. Para crear una nueva iniciativa personalizada:

    1. Haga clic en **Crear nueva**.
    1. Escriba la ubicación y el nombre de la definición.
    1. Seleccione las directivas que quiera incluir y haga clic en **Agregar**.
    1. Escriba los parámetros que quiera.
    1. Haga clic en **Save**(Guardar).
    1. En la página para agregar iniciativas personalizadas, haga clic en actualizar y la nueva iniciativa se mostrará como disponible.
    1. Haga clic en **Agregar** y asígnela a su suscripción.

    > [!NOTE]
    > Recuerde que la creación de nuevas iniciativas requiere credenciales de propietario de la suscripción. Para obtener más información sobre los roles de Azure, consulte los [ permisos en Azure Security Center](security-center-permissions.md).

1. Para ver las recomendaciones resultantes de la directiva, haga clic en **Recomendaciones** de la barra lateral para abrir la página recomendaciones. Las recomendaciones aparecerán con una etiqueta "Personalizada" y estarán disponibles en, aproximadamente, una hora.

    [![Recomendaciones personalizadas](media/custom-security-policies/custom-policy-recommendations.png)](media/custom-security-policies/custom-policy-recommendations-in-context.png#lightbox)


## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido a crear directivas de seguridad personalizadas. 

Para obtener material relacionado, consulte los siguientes artículos: 

- [Introducción a las directivas de seguridad](tutorial-security-policy.md)
- [Lista de directivas de seguridad integradas](security-center-policy-definitions.md)