---
title: Modelo de datos de Power BI
description: En este artículo se explican los detalles del modelo de datos de Power BI para los informes de Azure Backup.
ms.topic: conceptual
ms.date: 06/26/2017
ms.openlocfilehash: a2f06da16280070448d7b42dc5e1dcfc46354cfa
ms.sourcegitcommit: 4821b7b644d251593e211b150fcafa430c1accf0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2019
ms.locfileid: "74172798"
---
# <a name="data-model-for-azure-backup-reports"></a>Modelo de datos para informes de Azure Backup

En este artículo se describe el modelo de datos de Power BI utilizado para crear informes de Azure Backup. Con este modelo de datos, puede filtrar los informes existentes en función de los campos correspondientes, y lo que es más importante, crear sus propios informes mediante el uso de las tablas y campos del modelo.

## <a name="creating-new-reports-in-power-bi"></a>Creación de informes nuevos en Power BI

Power BI proporciona características de personalización que se usan para [crear informes mediante el modelo de datos](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/).

## <a name="using-azure-backup-data-model"></a>Uso del modelo de datos de Azure Backup

Los siguientes campos se pueden usar como parte del modelo de datos para crear informes y personalizar los informes existentes.

### <a name="alert"></a>Alerta

Esta tabla proporciona los campos y agregaciones básicos de diversos campos relacionados con las alertas.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| #AlertsCreatedInPeriod |Número entero |Número de alertas creadas en el período seleccionado |
| %ActiveAlertsCreatedInPeriod |Porcentaje |Porcentaje de alertas activas en el período seleccionado |
| %CriticalAlertsCreatedInPeriod |Porcentaje |Porcentaje de alertas críticas en el período seleccionado |
| AlertOccurrenceDate |Date |Fecha de creación de la alerta |
| AlertSeverity |Texto |Gravedad de la alerta. Por ejemplo, Critical |
| AlertStatus |Texto |Estado de la alerta. Por ejemplo, Active |
| AlertType |Texto |Tipo de alerta generada. Por ejemplo, Backup |
| AlertUniqueId |Texto |Identificador único de la alerta generada |
| AsOnDateTime |Fecha y hora |Hora de la última actualización de la fila seleccionada |
| AvgResolutionTimeInMinsForAlertsCreatedInPeriod |Número decimal |Tiempo medio (en minutos) para resolver la alerta en el período seleccionado |
| EntityState |Texto |Estado actual del objeto de alerta. Por ejemplo, Activo o Eliminado |

### <a name="backup-item"></a>Elemento de copia de seguridad

Esta tabla proporciona campos y agregaciones básicos en diversos campos relacionados con el elemento de copia de seguridad.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| #BackupItems |Número entero |Número de elementos de copia de seguridad |
| #UnprotectedBackupItems |Número entero |Número de elementos de copia de seguridad detenidos para su protección o configurados para que se realicen copias de seguridad, pero las copias de seguridad no se han iniciado|
| AsOnDateTime |Fecha y hora |Hora de la última actualización de la fila seleccionada |
| BackupItemFriendlyName |Texto |Nombre descriptivo del elemento de copia de seguridad |
| BackupItemId |Texto |Identificador del elemento de copia de seguridad |
| BackupItemName |Texto |Nombre de elemento de copia de seguridad |
| BackupItemType |Texto |Tipo de elemento de copia de seguridad. Por ejemplo, VM o FileFolder |
| EntityState |Texto |Estado actual del objeto del elemento de copia de seguridad. Por ejemplo, Activo o Eliminado |
| LastBackupDateTime |Fecha y hora |Hora de la última copia de seguridad del elemento de copia de seguridad seleccionado |
| LastBackupState |Texto |Estado de la última copia de seguridad del elemento de copia de seguridad seleccionado. Por ejemplo, Successful o Failed |
| LastSuccessfulBackupDateTime |Fecha y hora |Hora de última copia de seguridad correcta del elemento de copia de seguridad seleccionado |
| ProtectionState |Texto |Estado de protección del elemento de copia de seguridad. Por ejemplo, Protected o ProtectionStopped |

### <a name="calendar"></a>Calendario

Esta tabla proporciona detalles acerca de los campos relacionados con el calendario.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| Date |Date |Fecha seleccionada para filtrar datos |
| DateKey |Texto |Clave única para cada elemento de fecha |
| DayDiff |Número decimal |Diferencia en el día al filtrar los datos. Por ejemplo, 0 indica los datos del día actual, -1 indica los datos de un día anterior y 0 y -1 indican los datos del día actual y del anterior  |
| Mes |Texto |Mes del año seleccionado para filtrar datos, el mes empieza el día 1 y termina el día 31 |
| MonthDate | Date |Fecha del mes en que finaliza el mes, se selecciona para filtrar datos |
| MonthDiff |Número decimal |Diferencia en el mes al filtrar los datos. Por ejemplo, 0 indica los datos del mes actual, -1 indica los datos de un mes anterior y 0 y -1 indican los datos del mes actual y del anterior |
| Semana |Texto |Semana seleccionada para filtrar los datos; la semana comienza el domingo y termina el sábado |
| WeekDate |Date |Fecha de la semana en que finaliza la semana, se selecciona para filtrar datos |
| WeekDiff |Número decimal |Diferencia en la semana al filtrar los datos. Por ejemplo, 0 indica los datos de la semana actual, -1 indica los datos de una semana anterior y 0 y -1 indican los datos de la semana actual y de la anterior |
| Year |Texto |Año natural para filtrar datos |
| YearDate |Fecha |Fecha del año en que finaliza el año, se selecciona para filtrar datos |

### <a name="job"></a>Trabajo

Esta tabla proporciona campos y agregaciones básicos en diversos campos relacionados con el trabajo.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| #JobsCreatedInPeriod |Número entero |Número de trabajos creados en el período seleccionado |
| %FailuresForJobsCreatedInPeriod |Porcentaje |Porcentaje de errores de los trabajos globales en el período seleccionado |
| 80thPercentileDataTransferredInMBForBackupJobsCreatedInPeriod |Número decimal |Valor del percentil 80 de los datos transferidos, en MB, para los trabajos de **copia de seguridad** creados en el período seleccionado |
| AsOnDateTime |Fecha y hora |Hora de la última actualización de la fila seleccionada |
| AvgBackupDurationInMinsForJobsCreatedInPeriod |Número decimal |Tiempo medio, en minutos, de los trabajos de **copia de seguridad completados** creados en el período seleccionado |
| AvgRestoreDurationInMinsForJobsCreatedInPeriod |Número decimal |Tiempo medio, en minutos, de los trabajos de **restauración completados** creados en el período seleccionado |
| BackupStorageDestination |Texto |Destino del almacenamiento de copia de seguridad. Por ejemplo, Club, Disk  |
| EntityState |Texto |Estado actual del objeto de trabajo. Por ejemplo, Activo o Eliminado |
| JobFailureCode |Texto |Cadena del código de error por el que produjo el error del trabajo |
| JobOperation |Texto |Operación para la que se ejecuta el trabajo. Por ejemplo, Backup, Restore, Configure Backup |
| JobStartDate |Date |Fecha en que comenzó la ejecución del trabajo |
| JobStartTime |Hora |Hora en que comenzó la ejecución del trabajo |
| Estado del trabajo |Texto |Estado del trabajo finalizado. Por ejemplo, Completed, Failed |
| JobUniqueId |Texto |Identificador único para identificar el trabajo |

### <a name="policy"></a>Directiva

Esta tabla proporciona campos y agregaciones básicos en diversos campos relacionados con la directiva.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| #Directivas |Número entero |Número de directivas de copia de seguridad que existen en el sistema |
| #PoliciesInUse |Número entero |Número de directivas que se usan actualmente para configurar copias de seguridad |
| AsOnDateTime |Fecha y hora |Hora de la última actualización de la fila seleccionada |
| BackupDaysOfTheWeek |Texto |Días de la semana en los que se han programado copias de seguridad |
| BackupFrequency |Texto |Frecuencia con la que se ejecutan las copias de seguridad. Por ejemplo, daily o weekly |
| BackupTimes |Texto |Fecha y hora en que se programan las copias de seguridad |
| DailyRetentionDuration |Número entero |Duración de retención total de las copias de seguridad configuradas, en días |
| DailyRetentionTimes |Texto |Fecha y hora en que se configuró la retención diaria |
| EntityState |Texto |Estado actual del objeto de directiva. Por ejemplo, Activo o Eliminado |
| MonthlyRetentionDaysOfTheMonth |Texto |Fechas del mes seleccionadas para la retención mensual |
| MonthlyRetentionDaysOfTheWeek |Texto |Días de la semana seleccionados para la retención mensual |
| MonthlyRetentionDuration |Número decimal |Duración de retención total de las copias de seguridad configuradas, en meses |
| MonthlyRetentionFormat |Texto |Tipo de configuración para la retención mensual. Por ejemplo, Daily si se hace por días o Weekly si se hace por semanas |
| MonthlyRetentionTimes |Texto |Fecha y hora en que se ha configurado la retención mensual |
| MonthlyRetentionWeeksOfTheMonth |Texto |Semanas del mes en que se configura la retención mensual. Por ejemplo, First, Last, etc. |
| PolicyName |Texto |Nombre de la directiva definida |
| PolicyUniqueId |Texto |Identificador único para identificar la Directiva |
| RetentionType |Texto |Tipo de directiva de retención. Por ejemplo, Daily, Weekly, Monthly o Yearly |
| WeeklyRetentionDaysOfTheWeek |Texto |Días de la semana seleccionados para la retención semanal |
| WeeklyRetentionDuration |Número decimal |Duración total de la retención semanal de las copias de seguridad configuradas, en semanas |
| WeeklyRetentionTimes |Texto |Fecha y hora en que se ha configurado la retención semanal |
| YearlyRetentionDaysOfTheMonth |Texto |Fechas del mes seleccionadas para la retención anual |
| YearlyRetentionDaysOfTheWeek |Texto |Días de la semana seleccionados para la retención anual |
| YearlyRetentionDuration |Número decimal |Duración total de la retención de las copias de seguridad configuradas, en años |
| YearlyRetentionFormat |Texto |Tipo de configuración para la retención anual. Por ejemplo, Daily si se hace por días o Weekly si se hace por semanas |
| YearlyRetentionMonthsOfTheYear |Texto |Meses del año seleccionados para la retención anual |
| YearlyRetentionTimes |Texto |Fecha y hora en que se ha configurado la retención anual |
| YearlyRetentionWeeksOfTheMonth |Texto |Semanas del mes en que se configura la retención anual. Por ejemplo, First, Last, etc. |

### <a name="protected-server"></a>Servidor protegido

Esta tabla proporciona campos y agregaciones básicos en diversos campos relacionados con el servidor protegido.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| #ProtectedServers |Número entero |Número de servidores protegidos |
| AsOnDateTime |Fecha y hora |Hora de la última actualización de la fila seleccionada |
| AzureBackupAgentOSType |Texto |Tipo de sistema operativo de Azure Backup Agent |
| AzureBackupAgentOSVersion |Texto |Versión del sistema operativo de Azure Backup Agent |
| AzureBackupAgentUpdateDate |Texto |Fecha de actualización de Azure Backup Agent |
| AzureBackupAgentVersion |Texto |Número de versión de Azure Backup Agent |
| BackupManagementType |Texto |Tipo de proveedor para realizar una copia de seguridad. Por ejemplo, IaaSVM o FileFolder |
| EntityState |Texto |Estado actual del objeto de servidor protegido. Por ejemplo, Activo o Eliminado |
| ProtectedServerFriendlyName |Texto |Nombre descriptivo del servidor protegido |
| ProtectedServerName |Texto |Nombre del servidor protegido |
| ProtectedServerType |Texto |Tipo de servidor protegido del que se realiza la copia de seguridad. Por ejemplo, IaaSVMContainer |
| ProtectedServerName |Texto |Nombre del servidor protegido al que pertenece el elemento de copia de seguridad |
| RegisteredContainerId |Texto |Identificador del contenedor registrado para la copia de seguridad |

### <a name="storage"></a>Storage

Esta tabla proporciona campos y agregaciones básicos en diversos campos relacionados con el almacenamiento.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| #ProtectedInstances |Número decimal |Número de instancias protegidas que se utilizan para calcular el almacenamiento de front-end en la facturación; se calcula en función del valor más reciente del tiempo seleccionado |
| AsOnDateTime |Fecha y hora |Hora de la última actualización de la fila seleccionada |
| CloudStorageInMB |Número decimal |Almacenamiento de copia de seguridad en la nube utilizado por las copias de seguridad, calculados basándose en el último valor de tiempo seleccionado |
| EntityState |Texto |Estado actual del objeto. Por ejemplo, Activo o Eliminado |
| LastUpdatedDate |Date |Fecha de última actualización de la fila seleccionada |

### <a name="time"></a>Hora

Esta tabla proporciona detalles acerca de los campos relacionados con el tiempo.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| Hora |Hora |Hora del día. Por ejemplo, 1:00:00 p.m. |
| HourNumber |Número decimal |Número de hora del día. Por ejemplo, 13.00 |
| Minuto |Número decimal |Minuto de la hora |
| PeriodOfTheDay |Texto |Intervalo de período del día. Por ejemplo, 12-3 a.m. |
| Hora |Hora |Hora del día. Por ejemplo, 12:00:01 a.m. |
| TimeKey |Texto |Valor de clave que representa el tiempo |

### <a name="vault"></a>Almacén

Esta tabla proporciona campos y agregaciones básicos en diversos campos relacionados con el almacén.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| #Vaults |Número entero |Número de almacenes |
| AsOnDateTime |Fecha y hora |Hora de la última actualización de la fila seleccionada |
| AzureDataCenter |Texto |Centro de datos donde se encuentra el almacén |
| EntityState |Texto |Estado actual del objeto de almacén. Por ejemplo, Activo o Eliminado |
| StorageReplicationType |Texto |Tipo de replicación de almacenamiento para el almacén. Por ejemplo, GeoRedundant |
| SubscriptionId |Texto |Identificador de suscripción del cliente seleccionado para generar informes |
| VaultName |Texto |Nombre del almacén |
| VaultTags |Texto |Etiquetas asociadas al almacén |

## <a name="next-steps"></a>Pasos siguientes

Una vez que revise el modelo de datos para crear informes de Azure Backup, consulte los siguientes artículos para más información acerca de cómo crear y ver informes en Power BI.

* [Creación de un informe de Power BI nuevo mediante la importación de un conjunto de datos](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)
* [Filtros y resaltado en informes de Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
