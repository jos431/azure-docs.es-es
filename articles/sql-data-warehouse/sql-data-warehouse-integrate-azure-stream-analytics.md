---
title: Uso de Azure Stream Analytics
description: Sugerencias para usar Azure Stream Analytics con Azure SQL Data Warehouse para el desarrollo de soluciones.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: integration
ms.date: 03/22/2019
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 63803f3ac477e48d8d1c14a72e2ee9b9d4860047
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73685724"
---
# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a>Uso de Azure Stream Analytics con SQL Data Warehouse
Azure Stream Analytics es un servicio totalmente administrado que proporciona un procesamiento completo de eventos de baja latencia, alta disponibilidad y escalable a través el streaming de datos en la nube. Para aprender los conceptos básicos, lea [Introducción a Azure Stream Analytics][Introduction to Azure Stream Analytics]. Después puede aprender a crear una solución de extremo a extremo siguiendo el tutorial [Introducción al uso de Azure Stream Analytics][Get started using Azure Stream Analytics] .

En este artículo, aprenderá a usar la base de datos de Azure SQL Data Warehouse como receptor de salida para los trabajos de Stream Analytics.

## <a name="prerequisites"></a>Requisitos previos
En primer lugar, ejecute los pasos siguientes del tutorial [Introducción al uso de Azure Stream Analytics][Get started using Azure Stream Analytics] .  

1. Creación de una entrada de Centro de eventos
2. Configuración e inicio de la aplicación del generador de eventos
3. Aprovisionamiento de un trabajo de Stream Analytics
4. Especificación de una consulta y entrada de trabajo

Luego cree una base de datos de Azure SQL Data Warehouse.

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a>Especificación de la salida del trabajo: Base de datos de Azure SQL Data Warehouse
### <a name="step-1"></a>Paso 1
En el trabajo de Stream Analytics, haga clic en **SALIDA** en la parte superior de la página y luego en **AGREGAR**.

### <a name="step-2"></a>Paso 2
Seleccione SQL Database.

### <a name="step-3"></a>Paso 3
Escriba estos valores en la página siguiente:

* *Alias de salida*: escriba un nombre descriptivo para esta salida de trabajo.
* *Suscripción*:
  * Si la base de datos de SQL Data Warehouse está en la misma suscripción que el trabajo de Stream Analytics, seleccione Usar SQL Database de la suscripción actual".
  * Si la base de datos está en una suscripción diferente, seleccione Usar SQL Database de otra suscripción.
* *Base de datos*: especifique el nombre de una base de datos de destino.
* *Server Name* (Nombre del servidor): especifique el nombre del servidor para la base de datos especificada. Para encontrarlo, puede usar Azure Portal.

![][server-name]

* *Nombre de usuario*: especifique el nombre de usuario de una cuenta que tenga permisos de escritura para la base de datos.
* *Contraseña*: proporcione la contraseña de la cuenta de usuario especificada.
* *Tabla*: especifique el nombre de la tabla de destino en la base de datos.

![][add-database]

### <a name="step-4"></a>Paso 4
Haga clic en el botón de comprobación para agregar esta salida de trabajo y comprobar que Stream Analytics puede conectarse correctamente a la base de datos.

Cuando la conexión a la base de datos se realice correctamente, verá una notificación en el portal. Puede hacer clic en Probar para probar la conexión a la base de datos.

## <a name="next-steps"></a>Pasos siguientes
Para obtener información general sobre la integración, consulte [Información general de la integración de SQL Data Warehouse][SQL Data Warehouse integration overview].

Para obtener más sugerencias sobre desarrollo, consulte la [información general sobre desarrollo de SQL Data Warehouse][SQL Data Warehouse development overview].

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Introduction to Azure Stream Analytics]: ../stream-analytics/stream-analytics-introduction.md
[Get started using Azure Stream Analytics]: ../stream-analytics/stream-analytics-real-time-fraud-detection.md
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: https://azure.microsoft.com/documentation/services/stream-analytics/
