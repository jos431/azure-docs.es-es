---
title: Conexión de datos de firewall de aplicaciones web de Microsoft a Azure Sentinel | Microsoft Docs
description: Aprenda a conectar datos de firewall de aplicaciones web de Microsoft a Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: bfa2eca4-abdc-49ce-b11a-0ee229770cdd
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: e7dc1e6c1bb1ca81ada59cb3dae8fecbc6452b7f
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029775"
---
# <a name="connect-data-from-microsoft-web-application-firewall"></a>Conectar datos del firewall de aplicaciones web de Microsoft



Los registros de firewall de aplicaciones web (WAF) de Microsoft de Azure Application Gateway se pueden transmitir. Este WAF protege las aplicaciones de vulnerabilidades web comunes, como la inyección de código SQL o los ataques de scripts de sitios, y permite personalizar reglas para reducir los falsos positivos. Siga estas instrucciones para transmitir los registros de firewall de aplicaciones web de Microsoft a Azure Sentinel.


## <a name="prerequisites"></a>Requisitos previos

- Un recurso de puerta de enlace de aplicaciones existente.

## <a name="connect-to-microsoft-web-application-firewall"></a>Una conexión a un firewall de aplicaciones web de Microsoft.

Si ya tiene un firewall de aplicaciones web de Microsoft, asegúrese de que tiene un recurso de puerta de enlace existente.
Cuando el firewall de aplicaciones web de Microsoft esté implementado y obteniendo datos, los datos de alertas se pueden transmitir a Azure Sentinel muy fácilmente.
    
1. En el portal de Azure Sentinel, seleccione **Data connectors** (Conectores de datos).
1. En la página Data connectors (Conectores de datos), seleccione el icono de **WAF**.
1. Vaya a [Application Gateway resource](https://ms.portal.azure.com/#blade/HubsExtension/BrowseAllResourcesBlade/resourceType/Microsoft.Network%2FapplicationGateways)  (Recurso de Application Gateway) y elija el WAF.
    1. Seleccione **Configuración de diagnóstico**.
    1. Seleccione **+ Add diagnostic setting** (+ Agregar configuración de diagnóstico) debajo de la tabla.
    1. En la página **Diagnostic settings** (Configuración de diagnóstico), indique un nombre en **Name** y seleccione **Send to Log Analytics** (Enviar a Log Analytics).
    1. En **Log Analytics Workspace** (Área de trabajo de Log Analytics), seleccione el área de trabajo Azure Sentinel.
    1. Seleccione los tipos de registro que quiere analizar. Nuestra recomendación: ApplicationGatewayAccessLog y ApplicationGatewayFirewallLog.
1. Para usar el esquema correspondiente de Log Analytics para las alertas de firewall de aplicaciones de web de Microsoft, busque **AzureDiagnostics**.

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar un firewall de aplicaciones de web de Microsoft a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
