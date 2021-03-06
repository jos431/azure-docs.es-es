---
title: Configuración de un punto de conexión del equilibrador de carga interno (ILB)
titleSuffix: Azure Application Gateway
description: Este artículo proporciona información sobre cómo configurar Application Gateway con una dirección IP de front-end privada
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 11/14/2019
ms.author: absha
ms.openlocfilehash: a9e3150a5382e4d690ddf66c43bbe51e125509d3
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2019
ms.locfileid: "74075215"
---
# <a name="configure-an-application-gateway-with-an-internal-load-balancer-ilb-endpoint"></a>Configuración de una puerta de enlace de aplicaciones con un punto de conexión de equilibrador de carga interno (ILB)

Azure Application Gateway se puede configurar con una VIP accesible desde Internet o con un punto de conexión interno que no esté expuesto a Internet (utilizando una IP privada para la dirección IP del front-end), también conocido como punto de conexión ILB (equilibrador de carga interno). La configuración de la puerta de enlace con una dirección IP privada es útil para aplicaciones de línea de negocio internas que no están expuestas a Internet. También es útil para los distintos servicios y niveles de una aplicación de niveles múltiples que se encuentran dentro de un límite de seguridad no expuesto a Internet, pero que aún así siguen necesitando distribución de carga round robin, permanencia de sesión o terminación SSL (Capa de sockets seguros).

Este artículo le guía por los pasos necesarios para configurar una puerta de enlace de aplicaciones con una dirección IP privada de front-end desde Azure Portal.

En este artículo, aprenderá a:

- Crear una configuración IP de front-end privada para una instancia de Application Gateway
- Crear una puerta de enlace de aplicaciones con configuración IP de front-end privada


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="log-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en <https://portal.azure.com>

## <a name="create-an-application-gateway"></a>Creación de una puerta de enlace de aplicaciones

Para que Azure se comunique entre los recursos que se crean, se necesita una red virtual. Puede crear una red virtual o usar una existente. En este ejemplo, crearemos una red virtual. Puede crear una red virtual a la vez que crea la puerta de enlace de aplicaciones. Se crean instancias de Application Gateway en subredes independientes. En este ejemplo se crean dos subredes: una para la puerta de enlace de aplicaciones y la otra para los servidores back-end.

1. Haga clic en **Nuevo** en la esquina superior izquierda de Azure Portal.
2. Seleccione **Redes** y **Application Gateway** en la lista de destacados.
3. Escriba *myAppGateway* para el nombre de la puerta de enlace de aplicaciones y *myResourceGroupAG* para el nuevo grupo de recursos.
4. Acepte los valores predeterminados para las demás opciones y haga clic en **Aceptar**.
5. Haga clic en **Elegir una red virtual**, luego en **Crear nueva** y, después, especifique estos valores para la red virtual:
   - myVNet*: como nombre de la red virtual.
   - 10.0.0.0/16*: como espacio de direcciones de la red virtual.
   - *myAGSubnet*: como nombre de subred.
   - *10.0.0.0/24*: como espacio de direcciones de la subred.  
     ![private-frontendip-1](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-1.png)
6. Haga clic en **Aceptar** para crear la red virtual y la subred.
7. Elija la configuración IP de front-end como Privada y de forma predeterminada se realiza una asignación de dirección IP dinámica. La primera dirección disponible de la subred seleccionada se asignará como dirección IP de front-end.
8. Si desea elegir una dirección IP privada desde el intervalo de direcciones de subred (asignación estática), haga clic en el cuadro **Elija una dirección IP privada específica** y especifique la dirección IP.
   > [!NOTE]
   > Una vez asignada, el tipo de dirección IP (dinámico o estático) no se puede cambiar más adelante.
9. Elija la configuración de cliente de escucha para el protocolo y el puerto, la configuración de WAF (si es necesario) y haga clic en Aceptar.
    ![private-frontendip-2](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-2.png)
10. Revise la configuración en la página de resumen y, a continuación, haga clic en **Aceptar** para crear los recursos de red y la puerta de enlace de aplicaciones. La creación de la puerta de enlace de aplicaciones puede tardar varios minutos, espere a que finalice correctamente la implementación antes de pasar a la sección siguiente.

## <a name="add-backend-pool"></a>Agregar grupo de back-end

El grupo de back-end se usa para enrutar las solicitudes a los servidores back-end que atenderán la solicitud. Los servidores back-end pueden constar de NIC, conjuntos de escalado de máquinas virtuales, direcciones IP públicas e internas, nombres de dominio completos (FQDN) y servidores back-end multiinquilino como Azure App Service. En este ejemplo, se usan máquinas virtuales como back-end de destino. Pueden usarse máquinas virtuales existentes o crear otras nuevas. En este ejemplo, se crean dos máquinas virtuales que Azure usa como servidores back-end para la puerta de enlace de aplicaciones. Para ello, se hará lo siguiente:

1. Crear dos máquinas virtuales, *myVM* y *myVM2*, que se usarán como servidores back-end.
2. Instalar IIS en las máquinas virtuales para comprobar que la puerta de enlace de aplicaciones se ha creado correctamente.
3. Agregar los servidores back-end al grupo de back-end.

### <a name="create-a-virtual-machine"></a>Creación de una máquina virtual

1. Haga clic en **Nuevo**.
2. Haga clic en **Compute** y, después, seleccione **Windows Server 2016 Datacenter** en la lista de destacados.
3. Especifique estos valores para la máquina virtual:
   - *myVM*: como nombre de la máquina virtual.
   - *azureuser*: como nombre del usuario administrador.
   - *Azure123456!* como contraseña.
   - Seleccione **Usar existente** y *myResourceGroupAG*.
4. Haga clic en **OK**.
5. Seleccione **DS1_V2** para el tamaño de la máquina virtual y haga clic en **Seleccionar**.
6. Asegúrese de que **myVNet** está seleccionada como red virtual y que la subred es **myBackendSubnet**.
7. Haga clic en **Deshabilitado** para deshabilitar los diagnósticos de arranque.
8. Haga clic en **Aceptar**, revise la configuración en la página de resumen y haga clic en **Crear**.

### <a name="install-iis"></a>Instalación de IIS

1. Abra el shell interactivo y asegúrese de que está establecido en **PowerShell**.
    ![private-frontendip-3](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-3.png)
2. Ejecute el siguiente comando para instalar IIS en la máquina virtual:

   ```azurepowershell
   Set-AzVMExtension `
   
     -ResourceGroupName myResourceGroupAG `
   
     -ExtensionName IIS `
   
     -VMName myVM `
   
     -Publisher Microsoft.Compute `
   
     -ExtensionType CustomScriptExtension `
   
     -TypeHandlerVersion 1.4 `
   
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' -Location EastUS  ```



3. Create a second virtual machine and install IIS using the steps that you just finished. Enter myVM2 for its name and for VMName in Set-AzVMExtension.

### Add backend servers to backend pool

1. Click **All resources**, and then click **myAppGateway**.
2. Click **Backend pools**. A default pool was automatically created with the application gateway. Click **appGatewayBackendPool**.
3. Click **Add target** to add each virtual machine that you created to the backend pool.
   ![private-frontendip-4](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-4.png)
4. Click **Save.**

## Test the application gateway

1. Check your frontend IP that got assigned by clicking the **Frontend IP Configurations** blade in the portal.
    ![private-frontendip-5](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-5.png)
2. Copy the private IP address, and then paste it into the address bar of your browser of a VM in the same VNet or on-premises which has connectivity to this VNet and try to access the Application Gateway.

## Next steps

In this tutorial, you learned how to:

- Create a private frontend IP configuration for an Application Gateway
- Create an application gateway with private frontend IP configuration

If you want to monitor the health of your backend, see [Application Gateway Diagnostics](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).
