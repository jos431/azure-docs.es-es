---
title: Cláusula OFFSET LIMIT en Azure Cosmos DB
description: Obtenga información sobre cómo usar la cláusula OFFSET LIMIT para omitir y tomar algunos valores concretos al realizar consultas en Azure Cosmos DB
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mjbrown
ms.openlocfilehash: 68515c51862ada0b1aa794c09b3a6730504a57ee
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2019
ms.locfileid: "74873257"
---
# <a name="offset-limit-clause-in-azure-cosmos-db"></a>Cláusula OFFSET LIMIT en Azure Cosmos DB

La cláusula OFFSET LIMIT es una cláusula opcional para omitir y luego tomar cierto número de valores de la consulta. El recuento de OFFSET y el recuento de LIMIT son necesarios en la cláusula OFFSET LIMIT.

Cuando OFFSET LIMIT se usa junto con una cláusula ORDER BY, el conjunto de resultados se genera mediante una operación Skip y Take en los valores ordenados. Si no se usa ninguna cláusula ORDER BY, se producirá un error en un orden determinista de valores.

## <a name="syntax"></a>Sintaxis
  
```sql  
OFFSET <offset_amount> LIMIT <limit_amount>
```  
  
## <a name="arguments"></a>Argumentos

- `<offset_amount>`

   Especifica el número entero de elementos que los resultados de la consulta deben omitir.

- `<limit_amount>`
  
   Especifica el número entero de elementos que los resultados de la consulta deben incluir.

## <a name="remarks"></a>Comentarios
  
  Tanto el recuento de OFFSET como el recuento de LIMIT son necesarios en la cláusula OFFSET LIMIT. Si se usa una cláusula `ORDER BY` opcional, se genera el conjunto de resultados mediante la omisión de los valores ordenados. En caso contrario, la consulta devolverá un orden fijo de valores. Actualmente esta cláusula es compatible con consultas dentro de una única partición, las consultas entre particiones aún no se admiten.

## <a name="examples"></a>Ejemplos

Por ejemplo, he aquí una consulta que omite el primer valor y devuelve el segundo valor (en el orden del nombre de la ciudad de residencia):

```sql
    SELECT f.id, f.address.city
    FROM Families f
    ORDER BY f.address.city
    OFFSET 1 LIMIT 1
```

Los resultados son:

```json
    [
      {
        "id": "AndersenFamily",
        "city": "Seattle"
      }
    ]
```

Esta es una consulta que omite el primer valor y devuelve el segundo (sin ordenar):

```sql
   SELECT f.id, f.address.city
    FROM Families f
    OFFSET 1 LIMIT 1
```

Los resultados son:

```json
    [
      {
        "id": "WakefieldFamily",
        "city": "Seattle"
      }
    ]
```

## <a name="next-steps"></a>Pasos siguientes

- [Introducción](sql-query-getting-started.md)
- [Cláusula SELECT](sql-query-select.md)
- [Cláusula ORDER BY](sql-query-order-by.md)
