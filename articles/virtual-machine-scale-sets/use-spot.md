---
title: Creación de un conjunto de escalado que use máquinas virtuales de Azure Spot (versión preliminar)
description: Aprenda a crear conjuntos de escalado de Azure que usen máquinas virtuales de Spot para ahorrar costos.
services: virtual-machine-scale-sets
author: cynthn
manager: gwallace
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.topic: article
ms.date: 10/23/2019
ms.author: cynthn
ms.openlocfilehash: 68315b1b0d290b107fe2d28a9e3b49be009b78b8
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74781928"
---
# <a name="preview-azure-spot-vms-for-virtual-machine-scale-sets"></a>Vista previa: Máquinas virtuales de Azure Spot para los conjuntos de escalado 

El uso de Azure Spot en los conjuntos de escalado permite aprovechar las ventajas de nuestra capacidad sin usar con un importante ahorro en los costos. Siempre que Azure necesite recuperar la capacidad, su infraestructura expulsará las máquinas virtuales de Spot. Por lo tanto, las instancias de Spot son excelentes para las cargas de trabajo que soporten las interrupciones, como los trabajos de procesamiento por lotes, los entornos de desarrollo y pruebas, las grandes cargas de trabajo de proceso, etc.

La cantidad de capacidad sin usar disponible varía, por ejemplo, en función del tamaño, la región o la hora del día. Al implementar máquinas virtuales de Spot en los conjuntos de escalado, Azure asigna las instancias solo si hay capacidad disponible, aunque estas no tienen un Acuerdo de Nivel de Servicio. Los conjuntos de escalado de Spot se implementan en un dominio de error único y no ofrecen garantías de alta disponibilidad.

> [!IMPORTANT]
> Las instancias de Spot se encuentran actualmente en versión preliminar pública,
> la cual no se recomienda para las cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> En la primera parte de la versión preliminar pública, las instancias de Spot tendrán un precio fijo, por lo que no habrá expulsiones basadas en el precio.

## <a name="pricing"></a>Precios

Los precios de las instancias de Spot varían en función de la región y la SKU. Para más información, consulte los precios para [Linux](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) y [Windows](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/windows/). 


La variabilidad en los precios permite establecer un precio máximo, en dólares estadounidenses (USD), con un máximo de 5 decimales. Por ejemplo, el valor `0.98765` correspondería a un precio máximo de 0,98765 USD por hora. Si establece el precio máximo en `-1`, la instancia no se expulsará según el precio. El precio de la instancia será el actual de Spot o el de una instancia estándar, el menor de los dos, siempre que haya capacidad y cuota disponibles.

## <a name="eviction-policy"></a>Directiva de expulsión

Al crear conjuntos de escalado de Spot, puede establecer la directiva de expulsión en *Deallocate* (Desasignar) (valor predeterminado) o *Delete* (Eliminar). 

La directiva *Deallocate* (Desasignar) cambia las instancias expulsadas al estado stopped-deallocated para que pueda volver a implementarlas. Sin embargo, no hay ninguna garantía de que la asignación se realizará correctamente. Las máquinas virtuales desasignadas se siguen teniendo en cuenta en la cuota de instancias del conjunto de escalado y se le cobra por los discos subyacentes. 

Si quiere que las instancias del conjunto de escalado de Spot se eliminen al expulsarse, establezca la directiva de expulsión en *Delete* (Eliminar). Con la directiva de expulsión establecida para eliminar, puede crear nuevas máquinas virtuales mediante el aumento de la propiedad de recuento de instancias del conjunto de escalado. Las máquinas virtuales expulsadas se eliminan junto con sus discos subyacentes y, por tanto, no se le cobra el almacenamiento. También puede usar la característica de escalado automático de los conjuntos de escalado para probar y compensar automáticamente las máquinas virtuales expulsadas; sin embargo, no hay ninguna garantía de que la asignación se realice correctamente. Con el fin de evitar el costo de los discos y alcanzar los límites de cuota, se recomienda usar únicamente la característica de escalado automático en los conjuntos de escalado de Spot al establecer la directiva de expulsión en Delete (Eliminar). 

Los usuarios pueden optar por recibir notificaciones en las máquinas virtuales mediante [Azure Scheduled Events](../virtual-machines/linux/scheduled-events.md). De este modo se le notificará que se van a expulsar las máquinas virtuales y tendrá 30 segundos para terminar los trabajos y cerrar las tareas antes de que esto ocurra. 


## <a name="deploying-spot-vms-in-scale-sets"></a>Implementación de máquinas virtuales de Spot en conjuntos de escalado

Para implementar máquinas virtuales de Spot en conjuntos de escalado, puede establecer la nueva marca *Priority* (Prioridad) en *Spot*. Todas las máquinas virtuales del conjunto de escalado se establecerán para Spot. Para crear un conjunto de escalado con máquinas virtuales de Spot, use uno de los métodos siguientes:
- [Azure Portal](#portal)
- [CLI de Azure](#azure-cli)
- [Azure PowerShell](#powershell)
- [Plantillas del Administrador de recursos de Azure](#resource-manager-templates)

## <a name="portal"></a>Portal

El proceso para crear un conjunto de escalado que use máquinas virtuales de Spot es igual que el que se detalla en el [artículo de introducción](quick-create-portal.md). Cuando vaya a implementar un conjunto de escalado, puede elegir establecer la marca de Spot y la directiva de expulsión: ![Creación de un conjunto de escalado con máquinas virtuales de Spot](media/virtual-machine-scale-sets-use-spot/vmss-spot-portal-max-price.png)


## <a name="azure-cli"></a>CLI de Azure

El proceso para crear un conjunto de escalado con máquinas virtuales de Spot es igual que el que se detalla en el [artículo de introducción](quick-create-cli.md). Basta con agregar "--Priority Spot" y `--max-price`. En este ejemplo, usamos `-1` para `--max-price`, por lo que la instancia no se expulsará según el precio.

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --admin-username azureuser \
    --generate-ssh-keys \
    --priority Spot \
    --max-price -1 
```

## <a name="powershell"></a>PowerShell

El proceso para crear un conjunto de escalado con máquinas virtuales de Spot es igual que el que se detalla en el [artículo de introducción](quick-create-powershell.md).
Basta con agregar "-Priority Spot" y proporcionar un valor de `-max-price` a [New-AzVmssConfig](/powershell/module/az.compute/new-azvmssconfig).

```powershell
$vmssConfig = New-AzVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Priority "Spot" `
    --max-price -1
```

## <a name="resource-manager-templates"></a>Plantillas de Resource Manager

El proceso para crear un conjunto de escalado que use máquinas virtuales de Spot es igual que el que se detalla en el artículo de introducción para [Linux](quick-create-template-linux.md) o [Windows](quick-create-template-windows.md). Agregue la propiedad "priority" al tipo de recurso *Microsoft.Compute/virtualMachineScaleSets/virtualMachineProfile* en la plantilla y especifique el valor *Spot*. Asegúrese de usar la versión de API *2019-03-01* o superior. 

Para establecer la directiva de expulsión en eliminación, agregue el parámetro "evictionPolicy" y establézcalo en *Eliminar*.

En el ejemplo siguiente se crea un conjunto de escalado de Linux de Spot llamado *myScaleSet* en *Centro de EE. UU.* , que *elimina* las máquinas virtuales del conjunto de escalado tras su expulsión:

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US 2",
  "apiVersion": "2019-03-01",
  "sku": {
    "name": "Standard_DS2_v2",
    "capacity": "2"
  },
  "properties": {
    "upgradePolicy": {
      "mode": "Automatic"
    },
    "virtualMachineProfile": {
       "priority": "Spot",
       "evictionPolicy": "delete",
       "storageProfile": {
        "osDisk": {
          "caching": "ReadWrite",
          "createOption": "FromImage"
        },
        "imageReference":  {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "16.04-LTS",
          "version": "latest"
        }
      },
      "osProfile": {
        "computerNamePrefix": "myvmss",
        "adminUsername": "azureuser",
        "adminPassword": "P@ssw0rd!"
      }
    }
  }
}
```
## <a name="faq"></a>Preguntas más frecuentes

**P:** Una vez creada, ¿es la instancia de Spot la misma que la estándar?

**R:** Sí, salvo que no hay ningún Acuerdo de Nivel de Servicio para las máquinas virtuales de Spot y se pueden expulsar en cualquier momento.


**P:** ¿Qué se debe hacer cuando se produce la expulsión, pero aún se necesita capacidad?

**R:** Si necesita capacidad de inmediato, se recomienda usar máquinas virtuales estándar en lugar de máquinas virtuales de Spot.


**P:** ¿Cómo se administra la cuota para Spot?

**R:** Las instancias de Spot y las estándar tendrán grupos de cuotas independientes. La cuota de Spot se compartirá entre las máquinas virtuales y los conjuntos de escalado. Para más información, consulte [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](https://docs.microsoft.com/azure/azure-subscription-service-limits).


**P:** ¿Puedo solicitar una cuota adicional para Spot?

**R:** Sí, podrá enviar la solicitud de aumentar su cuota para las máquinas virtuales de Spot mediante el [proceso de solicitud de cuota estándar](https://docs.microsoft.com/azure/azure-supportability/per-vm-quota-requests).


**P:** ¿Puedo convertir los conjuntos de escalado existentes en conjuntos de escalado de Spot?

**R:** No, solo se permite establecer la marca `Spot` durante la creación.


**P:** Si uso `low` para los conjuntos de escalado de prioridad baja, ¿necesito empezar a usar `Spot` en su lugar?

**R:** Por ahora, `low` y `Spot` funcionarán, pero debe empezar a usar `Spot`.


**P:** ¿Puedo crear un conjunto de escalado con tanto con máquinas virtuales normales como de Spot?

**R:** No, un conjunto de escalado no admite más de un tipo de prioridad.


**P:**  ¿Puedo usar el escalado automático con conjuntos de escalado de Spot?

**R:** Sí, en el conjunto de escalado de Spot puede establecer reglas de escalado automático. Si se expulsan las máquinas virtuales, el escalado automático puede intentar crear nuevas máquinas virtuales de Spot. Recuerde que, aun así, no se garantiza esta capacidad. 


**P:**  ¿Funciona el escalado automático con las dos directivas de expulsión (desasignar y eliminar)?

**R:** Se recomienda establecer la directiva de expulsión en Eliminar cuando se usa el escalado automático. Esto es porque las instancias desasignadas se cuentan para el número de capacidad en el conjunto de escalado. Cuando se usa el escalado automático, es probable que alcance el número de instancias de destino rápidamente debido a las instancias expulsadas desasignadas. 


**P:** ¿Qué canales admiten las máquinas virtuales de Spot?

**R:** Consulte la tabla siguiente para la disponibilidad de máquinas virtuales de Spot.

<a name="channel"></a>

| Canales de Azure               | Disponibilidad de las máquinas virtuales de Azure       |
|------------------------------|-----------------------------------|
| Contrato Enterprise         | Sí                               |
| Pago por uso                | Sí                               |
| Proveedor de servicios en la nube (CSP) | [Póngase en contacto con su asociado](https://docs.microsoft.com/partner-center/azure-plan-get-started) |
| Ventajas                     | No disponible                     |
| Patrocinados                    | No disponible                     |
| Versión de prueba gratuita                   | No disponible                     |


**P:** ¿Dónde puedo publicar preguntas?

**R:** Puede publicar y etiquetar la pregunta con `azure-spot` en [Q&A](https://docs.microsoft.com/answers/topics/azure-spot.html) (Preguntas y respuestas). 

## <a name="next-steps"></a>Pasos siguientes
Ahora que ha creado un conjunto de escalado con máquinas virtuales de Spot, pruebe a implementar nuestra [plantilla de escalado automático con Spot](https://github.com/Azure/vm-scale-sets/tree/master/preview/lowpri).

Consulte la [página de precios del conjunto de escalado de máquinas virtuales](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) para ver información de precios.
