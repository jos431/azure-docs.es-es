---
title: Diseñe tablas escalables y de alto rendimiento en el almacenamiento en tablas de Azure. | Microsoft Docs
description: Diseñe tablas escalables y de alto rendimiento en el almacenamiento en tablas de Azure.
services: storage
author: SnehaGunda
ms.service: storage
ms.topic: article
ms.date: 04/23/2018
ms.author: sngun
ms.subservice: tables
ms.openlocfilehash: 8387e41d57edfa0e54ac930c9462714aca571f2a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60848289"
---
# <a name="design-scalable-and-performant-tables"></a>Diseño de tablas escalables y eficaces

[!INCLUDE [storage-table-cosmos-db-tip-include](../../../includes/storage-table-cosmos-db-tip-include.md)]

Para diseñar tablas escalables y de alto rendimiento, debe tener en cuenta una serie de factores, como el rendimiento, la escalabilidad y el costo. Si anteriormente ha diseñado esquemas de bases de datos relacionales, estas consideraciones le serán familiares, pero aunque hay algunas similitudes entre los modelos relacionales y el modelo de almacenamiento de Azure Table service, también existen diferencias importantes. Normalmente, estas diferencias conducen a diseños diferentes que pueden parecer no intuitivos o incorrectos para alguien que esté familiarizado con las bases de datos relacionales, pero que sí tienen sentido si va a diseñar un almacén de claves/valores de NoSQL como Azure Table service. Muchas de sus diferencias de diseño reflejan el hecho de que Table service está diseñado para admitir aplicaciones de escala de nube que pueden contener miles de millones de entidades (o filas en terminología de bases de datos relacionales) de datos o conjuntos de datos que deben admitir volúmenes elevados de transacciones. Por lo tanto, debe pensar de forma diferente en cómo almacenar los datos y comprender cómo funciona Table service. Un almacén de datos NoSQL bien diseñado puede permitir que su solución se escale mucho más (y a un costo más bajo) que una solución que utiliza una base de datos relacional. Esta guía le ayuda con estos temas.  

## <a name="about-the-azure-table-service"></a>Acerca de Azure Table service
En esta sección se resaltan algunas de las características clave de Table service que son especialmente importantes para obtener un diseño que confiera rendimiento y escalabilidad. Si no está familiarizado con Azure Storage y Table service, consulte primero [Introducción a Microsoft Azure Storage](../../storage/common/storage-introduction.md) e [Introducción a Azure Table Storage mediante .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md) antes de leer el resto de este artículo. Aunque esta guía se centra en Table service, incluye información sobre los servicios Azure Queue y Blob service, y cómo usarlos con Table service.  

¿Qué es Table service? Como cabría esperar por su nombre, Table service usa un formato tabular para almacenar los datos. En la terminología estándar, cada fila de la tabla representa una entidad y las columnas almacenan las distintas propiedades de la entidad. Cada entidad tiene un par de claves que la identifican de forma única, y una columna de marca de tiempo que usa Table service para realizar el seguimiento de cuándo se actualizó la entidad por última vez. La marca de tiempo se aplica automáticamente y no se puede sobrescribir manualmente con un valor arbitrario. Table service usa esta última marca de tiempo modificada (LMT) para administrar la simultaneidad optimista.  

> [!NOTE]
> Las operaciones de API REST de Table service también devuelven un valor **ETag** que se obtiene del LMT. En este documento se usan los términos ETag y LMT indistintamente porque hacen referencia a los mismos datos subyacentes.  
> 
> 

En el ejemplo siguiente se muestra el diseño de una tabla sencilla para almacenar las entidades employee y department. Muchos de los ejemplos que se muestran más adelante en esta guía se basan en este diseño simple.  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Timestamp</th>
<th></th>
</tr>
<tr>
<td>Marketing</td>
<td>00001</td>
<td>2014-08-22T00:50:32Z</td>
<td>
<table>
<tr>
<th>Nombre</th>
<th>Apellidos</th>
<th>Edad</th>
<th>Email</th>
</tr>
<tr>
<td>Don</td>
<td>Hall</td>
<td>34</td>
<td>donh@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Marketing</td>
<td>00002</td>
<td>2014-08-22T00:50:34Z</td>
<td>
<table>
<tr>
<th>Nombre</th>
<th>Apellidos</th>
<th>Edad</th>
<th>Email</th>
</tr>
<tr>
<td>Jun</td>
<td>Cao</td>
<td>47</td>
<td>junc@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Marketing</td>
<td>department</td>
<td>2014-08-22T00:50:30Z</td>
<td>
<table>
<tr>
<th>DepartmentName</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td>Marketing</td>
<td>153</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>Ventas</td>
<td>00010</td>
<td>2014-08-22T00:50:44Z</td>
<td>
<table>
<tr>
<th>Nombre</th>
<th>Apellidos</th>
<th>Edad</th>
<th>Email</th>
</tr>
<tr>
<td>Ken</td>
<td>Kwok</td>
<td>23</td>
<td>kenk@contoso.com</td>
</tr>
</table>
</td>
</tr>
</table>


Hasta ahora, estos datos aparecen como en una tabla de una base de datos relacional, siendo las diferencias principales las columnas obligatorias y la posibilidad de almacenar varios tipos de entidad en la misma tabla. Además, cada una de las propiedades definidas por el usuario como **FirstName** o **Age** tienen un tipo de datos, como un número entero o una cadena, lo mismo que una columna de una base de datos relacional. Aunque a diferencia de una base de datos relacional, la naturaleza sin esquema de Table service significa que una propiedad no necesita tener los mismos tipos de datos en cada entidad. Para almacenar tipos de datos complejos en una sola propiedad, debe utilizar un formato serializado como JSON o XML. Para obtener más información sobre Table service, como los tipos de datos admitidos, los intervalos de fechas admitidos, las reglas de nomenclatura y las restricciones de tamaño, consulte [Introducción al modelo de datos de Table service](https://msdn.microsoft.com/library/azure/dd179338.aspx).

La elección de **PartitionKey** y **RowKey** es fundamental para un buen diseño de tabla. Todas las entidades almacenadas en una tabla deben tener una combinación única de **PartitionKey** y **RowKey**. Al igual que con las claves de una tabla de base de datos relacional, los valores **PartitionKey** y **RowKey** se indexan para crear un índice agrupado para permitir búsquedas rápidas. Sin embargo, Table service no crea índices secundarios, por lo que **PartitionKey** y **RowKey** son las únicas propiedades indexadas. Algunos de los patrones descritos en [Patrones de diseño de tablas](table-storage-design-patterns.md) ilustran cómo puede solucionar esta limitación aparente.  

Una tabla consta de una o varias particiones, y muchas de las decisiones de diseño que tome estarán relacionadas con la elección de un valor adecuado de **PartitionKey** y **RowKey** para optimizar la solución. Una solución puede constar de una única tabla que contenga todas las entidades organizadas en particiones, pero normalmente las soluciones tendrán varias tablas. Tablas le ayuda a organizar las entidades de manera lógica, le ayudará a administrar el acceso a los datos mediante listas de control de acceso y puede quitar una tabla completa mediante una sola operación de almacenamiento.  

## <a name="table-partitions"></a>Particiones de tabla
El nombre de la cuenta, el nombre de la tabla y **PartitionKey** juntos identifican la partición dentro del servicio de almacenamiento donde Table service almacena la entidad. Además de ser parte del esquema de direccionamiento de las entidades, las particiones definen un ámbito para las transacciones (vea [Transacciones de grupo de entidad](#entity-group-transactions) a continuación) y forman la base de cómo escala Table service. Para más información sobre las particiones, consulte [Objetivos de rendimiento y escalabilidad de Azure Storage](../../storage/common/storage-scalability-targets.md).  

En Table service, un nodo individual da servicio a una o más particiones completas y el servicio se escala equilibrando dinámicamente la carga de las particiones entre los nodos. Si un nodo está bajo carga, Table Service puede *dividir* el intervalo de particiones atendidas por ese nodo en nodos diferentes; cuando el tráfico disminuye, el servicio puede *combinar* los intervalos de la partición de nodos silenciosos a un único nodo.  

Para más información sobre los detalles internos de Table service y saber en particular cómo administra las particiones, consulte el artículo [Microsoft Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx) (Microsoft Azure Storage: Un servicio de almacenamiento en nube altamente disponible de gran coherencia).  

## <a name="entity-group-transactions"></a>Transacciones de grupo de entidad
En Table service, las transacciones de grupo de entidad (EGT) son el único mecanismo integrado para realizar actualizaciones atómicas en varias entidades. Las EGT se conocen en ocasiones como *transacciones por lotes*. Las EGT solo pueden funcionar en entidades almacenadas en la misma partición (es decir, que comparten la misma clave de partición en una tabla determinada). Así que cada vez que necesite un comportamiento de transacciones atómico entre varias entidades, debe asegurarse de que esas entidades estén en la misma partición. Este suele ser un motivo para mantener varios tipos de entidad en la misma tabla (y partición) y no utilizar varias tablas para diferentes tipos de entidad. Una sola EGT puede operar en 100 entidades como máximo.  Si envía varias EGT simultáneas para procesamiento, es importante asegurarse de que esas EGT no se usan en las entidades que son comunes a todas las EGT, ya que de lo contrario se puede retrasar el procesamiento.

Las EGT también presentan posibles ventajas e inconvenientes que es necesario evaluar en el diseño. Es decir, usar más particiones aumenta la escalabilidad de la aplicación, porque Azure tiene más oportunidades para equilibrar la carga de solicitudes entre los nodos. Sin embargo, también podría ser un inconveniente al limitar la capacidad de la aplicación para realizar transacciones atómicas y mantener una fuerte coherencia de los datos. Además, existen objetivos de escalabilidad específicos en el nivel de una partición que podrían limitar el rendimiento de las transacciones que se puede esperar de un único nodo. Para más información sobre los objetivos de escalabilidad en cuentas de almacenamiento de Azure y sobre Table service, consulte [Objetivos de escalabilidad y rendimiento de Azure Storage](../../storage/common/storage-scalability-targets.md).   

## <a name="capacity-considerations"></a>Consideraciones de capacidad
En la tabla siguiente se describen algunos de los valores de clave a tener en cuenta al diseñar una solución de Table service:  

| Capacidad total de una cuenta de Azure Storage | 500 TB |
| --- | --- |
| Número de tablas en una cuenta de Azure Storage |Solo limitadas por la capacidad de la cuenta de almacenamiento |
| Número de particiones en una tabla |Solo limitadas por la capacidad de la cuenta de almacenamiento |
| Número de entidades de una partición |Solo limitadas por la capacidad de la cuenta de almacenamiento |
| Tamaño de una entidad individual |Hasta 1 MB con un máximo de 255 propiedades (incluyendo **PartitionKey**, **RowKey** y **Timestamp**) |
| Tamaño de la **PartitionKey** |Una cadena de hasta 1 KB |
| Tamaño de la **RowKey** |Una cadena de hasta 1 KB |
| Tamaño de una transacción de un grupo de entidades |Una transacción puede incluir como máximo 100 entidades y la carga debe ser inferior a 4 MB. Un EGT solo puede actualizar una entidad una vez. |

Para más información, consulte [Descripción del modelo de datos de Table service](https://msdn.microsoft.com/library/azure/dd179338.aspx).  

## <a name="cost-considerations"></a>Consideraciones sobre el costo
El almacenamiento en tablas es relativamente económico, pero debe incluir las estimaciones de costos por el uso de capacidad y la cantidad de transacciones, como parte de la evaluación de cualquier solución de Table service. Sin embargo, en muchos escenarios, el almacenamiento de datos duplicados o sin normalizar para mejorar el rendimiento o la escalabilidad de su solución es un enfoque válido. Para obtener más información sobre los precios, consulte [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/).  

## <a name="next-steps"></a>Pasos siguientes

- [Patrones de diseño de tablas](table-storage-design-patterns.md)
- [Modelado de relaciones](table-storage-design-modeling.md)
- [Diseño de consulta](table-storage-design-for-query.md)
- [Cifrado de datos de tablas](table-storage-design-encrypt-data.md)
- [Diseño para la modificación de datos](table-storage-design-for-modification.md)
