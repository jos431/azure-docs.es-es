---
title: Métricas de Azure Monitor para Application Gateway
description: Obtenga información sobre cómo usar métricas para supervisar el rendimiento de la puerta de enlace de aplicaciones
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 8/29/2019
ms.author: absha
ms.openlocfilehash: f0937ee53e66cb1bf0c5d6b55a8dde045570e924
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70309721"
---
# <a name="metrics-for-application-gateway"></a>Métricas para Application Gateway

Application Gateway publica puntos de datos, denominados métricas, para [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) para el rendimiento de las instancias de Application Gateway y back-end. Estas métricas son valores numéricos de un conjunto ordenado de datos de serie temporal que describen algún aspecto de la puerta de enlace de aplicaciones en un momento determinado. Si hay solicitudes que fluyen a través de Application Gateway, mide y envía sus métricas en intervalos de 60 segundos. Si no hay ninguna solicitud que fluya a través de Application Gateway o no tiene datos para una métrica, no se informará de la métrica. Para obtener más información, vea [Información general sobre las métricas en Microsoft Azure](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics).

## <a name="metrics-supported-by-application-gateway-v2-sku"></a>Métricas compatibles con la SKU de Application Gateway V2

### <a name="timing-metrics"></a>Métricas de tiempo

Están disponibles las siguientes métricas relacionadas con el momento de la solicitud y la respuesta. Mediante el análisis de estas métricas, puede determinar si la ralentización en la aplicación se debe a la WAN, Application Gateway, la red entre Application Gateway y el back-end o el rendimiento de la aplicación.

- **Cliente RTT**

  Tiempo medio de ida y vuelta entre clientes y Application Gateway. Esta métrica indica cuánto tiempo se tarda en establecer conexiones y devolver confirmaciones.

- **Tiempo total de Application Gateway**

  Promedio de tiempo que se tarda en procesar una solicitud y en enviar la respuesta. Esto se calcula como promedio del intervalo desde el momento en que Application Gateway recibe el primer byte de una solicitud HTTP hasta el momento en que termina la operación de envío de la respuesta. Es importante tener en cuenta que esto normalmente incluye el tiempo de procesamiento de Application Gateway, el tiempo que los paquetes de solicitud y respuesta viajan por la red y el tiempo que el servidor back-end tardó en responder.

- **Tiempo de conexión de back-end**

  Tiempo empleado en establecer una conexión con un servidor back-end. 

- **Tiempo de respuesta del primer byte de back-end**

  Intervalo de tiempo entre el inicio del establecimiento de una conexión con el servidor back-end y la recepción del primer byte del encabezado de respuesta, el tiempo de procesamiento aproximado del servidor back-end

- **Tiempo de respuesta del último byte de back-end**

  Intervalo de tiempo entre el inicio del establecimiento de una conexión con el servidor back-end y la recepción del último byte del cuerpo de la respuesta

### <a name="application-gateway-metrics"></a>Métricas de Application Gateway

Para Application Gateway, están disponibles las métricas siguientes:

- **Bytes recibidos**

   Recuento de bytes recibidos por Application Gateway de los clientes

- **Bytes enviados**

   Recuento de bytes enviados por Application Gateway de los clientes

- **Protocolo TLS de cliente**

   Recuento de solicitudes TLS y no TLS iniciadas por el cliente que establecieron la conexión con Application Gateway. Para ver la distribución del protocolo TLS, filtre por el protocolo TLS de la dimensión.

- **Unidades de capacidad actuales**

   Recuento de unidades de capacidad consumidas. Las unidades de capacidad miden el costo basado en el consumo, que se suma al costo fijo. Hay tres determinantes para la unidad de capacidad: unidad de proceso, conexiones persistentes y rendimiento. Cada unidad de capacidad se compone a lo sumo de: 1 unidad de proceso, o 2500 conexiones persistentes, o rendimiento de 2,22 Mbps.

- **Unidades de proceso actuales**

   Recuento de la capacidad de procesador consumida. Los factores que afectan a la unidad de proceso son las conexiones TLS/seg, los cálculos de reescritura de direcciones URL y el procesamiento de reglas de WAF. 

- **Conexiones actuales**

   Recuento de conexiones actuales establecidas con Application Gateway

- **Solicitudes con error**

   Recuento de solicitudes con error que ha servido Application Gateway. El recuento de solicitudes puede filtrarse aún más para mostrar el recuento por cada combinación de configuración de grupo de back-end o http específica.


- **Estado de respuesta**

   Estado de la respuesta HTTP devuelta por Application Gateway. La distribución del código de estado de respuesta se puede categorizar aún más para mostrar las respuestas en categorías 2xx, 3xx, 4xx y 5xx.

- **Rendimiento**

   Número de bytes por segundo que ha ofrecido Application Gateway

- **Total de solicitudes**

   Recuento de solicitudes correctas que ha servido Application Gateway. El recuento de solicitudes puede filtrarse aún más para mostrar el recuento por cada combinación de configuración de grupo de back-end o http específica.

- **Reglas de coincidencia de Firewall de aplicaciones Web**

- **Reglas desencadenadas por el Firewall de aplicaciones Web**

### <a name="backend-metrics"></a>Métricas de back-end

Para Application Gateway, están disponibles las métricas siguientes:

- **Estado de respuesta de back-end**

  Recuento de códigos de estado de respuesta HTTP devueltos por los back-ends. No se incluyen los códigos de respuesta generados por Application Gateway. La distribución del código de estado de respuesta se puede categorizar aún más para mostrar las respuestas en categorías 2xx, 3xx, 4xx y 5xx.

- **Recuento de hosts con estado correcto**

  El número de back-ends que el sondeo de Estado ha determinado que son correctos. También puede filtrar en función de grupos de back-end para mostrar hosts en buen/mal estado en un grupo de back-end específico.

- **Recuento de hosts con estado incorrecto**

  El número de back-ends que el sondeo de Estado ha determinado que son incorrectos. También puede filtrar en función de grupos de back-end para mostrar hosts en mal estado en un grupo de back-end específico.

## <a name="metrics-supported-by-application-gateway-v1-sku"></a>Métricas compatibles con la SKU de Application Gateway V1

### <a name="application-gateway-metrics"></a>Métricas de Application Gateway

Para Application Gateway, están disponibles las métricas siguientes:

- **Conexiones actuales**

  Recuento de conexiones actuales establecidas con Application Gateway

- **Solicitudes con error**

  Recuento de solicitudes con error que ha servido Application Gateway. El recuento de solicitudes puede filtrarse aún más para mostrar el recuento por cada combinación de configuración de grupo de back-end o http específica.

- **Estado de respuesta**

  Estado de la respuesta HTTP devuelta por Application Gateway. La distribución del código de estado de respuesta se puede categorizar aún más para mostrar las respuestas en categorías 2xx, 3xx, 4xx y 5xx.

- **Rendimiento**

  Número de bytes por segundo que ha ofrecido Application Gateway

- **Total de solicitudes**

  Recuento de solicitudes correctas que ha servido Application Gateway. El recuento de solicitudes puede filtrarse aún más para mostrar el recuento por cada combinación de configuración de grupo de back-end o http específica.

- **Reglas de coincidencia de Firewall de aplicaciones Web**

- **Reglas desencadenadas por el Firewall de aplicaciones Web**

### <a name="backend-metrics"></a>Métricas de back-end

Para Application Gateway, están disponibles las métricas siguientes:

- **Recuento de hosts con estado correcto**

  El número de back-ends que el sondeo de Estado ha determinado que son correctos. También puede filtrar en función de grupos de back-end para mostrar hosts en buen/mal estado en un grupo de back-end específico.

- **Recuento de hosts con estado incorrecto**

  El número de back-ends que el sondeo de Estado ha determinado que son incorrectos. También puede filtrar en función de grupos de back-end para mostrar hosts en mal estado en un grupo de back-end específico.

## <a name="metrics-visualization"></a>Visualización de las métricas

Navegue a una instancia de Application Gateway y, en **Supervisión**, seleccione **Métricas**. Para ver los valores disponibles, seleccione la lista desplegable **MÉTRICA**.

En la siguiente imagen, verá un ejemplo con tres métricas que se muestran para los últimos 30 minutos:

[![](media/application-gateway-diagnostics/figure5.png "Vista de métrica")](media/application-gateway-diagnostics/figure5-lb.png#lightbox)

Para ver una lista de métricas actuales, consulte el artículo de [métricas compatibles con Azure Monitor](../azure-monitor/platform/metrics-supported.md).

### <a name="alert-rules-on-metrics"></a>Reglas de alerta en métricas

Puede iniciar las reglas de alerta en función de las métricas de un recurso. Por ejemplo, una alerta puede llamar a un webhook o enviar un correo electrónico a un administrador si el rendimiento de la puerta de enlace de aplicaciones es superior, igual o inferior a un umbral durante un período especificado.

En el ejemplo siguiente, se explica paso a paso cómo crear una regla de alerta que envía un correo electrónico a un administrador cuando el rendimiento supera un umbral:

1. Seleccione **Agregar alerta de métrica** para abrir la hoja **Agregar regla**. También puede acceder a esta página desde la página de métricas.

   ![Botón “Agregar alerta de métrica”][6]

2. En la página **Agregar regla**, rellene las secciones de nombre, condición y notificación, y seleccione **Aceptar**.

   * En el selector **Condición**, seleccione uno de los cuatro valores: **Mayor que**, **Mayor o igual que**, **Menor que** o **Menor o igual que**.

   * En el selector **Período**, seleccione un período de cinco minutos a seis horas.

   * Al seleccionar **Lectores, colaboradores y propietarios de correo electrónico**, el correo electrónico puede ser dinámico según los usuarios que tengan acceso a ese recurso. De lo contrario, puede proporcionar una lista separada por comas de los usuarios en el cuadro de texto **Correos electrónicos de administrador adicionales**.

   ![Página Agregar regla][7]

Si se supera el umbral, llegará un correo electrónico similar al que se muestra en la siguiente imagen:

![Correo electrónico para el umbral infringido][8]

Después de crear una alerta de métrica, aparece una lista de alertas. Proporciona una visión general de todas las reglas de alerta.

![Lista de alertas y reglas][9]

Para más información acerca de las notificaciones de alerta, consulte [Recibir notificaciones de alerta](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

Para conocer más detalles sobre los webhooks y cómo usarlos con las alertas, visite [Configuración de un webhook en una alerta de métrica de Azure](../azure-monitor/platform/alerts-webhooks.md).

## <a name="next-steps"></a>Pasos siguientes

* Visualice el contador y los registros de eventos mediante los [registros de Azure Monitor](../azure-monitor/insights/azure-networking-analytics.md).
* Consulte la entrada de blog [Visualize your Azure Activity Logs with Power BI](https://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) (Visualizar los registros de actividades de Azure con Power BI).
* Consulte la entrada de blog [View and analyze Azure Audit Logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) (Consulta y análisis de registros de auditoría de Azure en Power BI y más).

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png
[10]: ./media/application-gateway-diagnostics/figure10.png
