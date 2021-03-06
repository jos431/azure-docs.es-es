---
title: azcopy jobs show | Microsoft Docs
description: En este artículo se proporciona información de referencia del comando azcopy jobs show.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 7b5f566757dd77a61f252b123d0c9c1b74303fbe
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2019
ms.locfileid: "74034135"
---
# <a name="azcopy-jobs-show"></a>azcopy jobs show

Muestra información detallada del identificador de trabajo especificado.

## <a name="synopsis"></a>Sinopsis

Si solo se proporciona el identificador de trabajo sin ninguna marca, se devolverá el resumen de progreso del trabajo.

Los recuentos en bytes y el porcentaje completado que aparecen al ejecutar este comando solo reflejan los archivos que se completan en el trabajo. No reflejan los archivos completados parcialmente.

Si se establece la marca `with-status`, se mostrará la lista de transferencias del trabajo con el valor especificado.

```azcopy
azcopy jobs show [jobID] [flags]
```

## <a name="related-conceptual-articles"></a>Artículos conceptuales relacionados

- [Introducción a AzCopy](storage-use-azcopy-v10.md)
- [Transferencia de datos con AzCopy y Blob Storage](storage-use-azcopy-blobs.md)
- [Transferencia de datos con AzCopy y File Storage](storage-use-azcopy-files.md)
- [Configurar, optimizar y solucionar problemas de AzCopy](storage-use-azcopy-configure.md)

## <a name="options"></a>Opciones

|Opción|DESCRIPCIÓN|
|--|--|
|-h, --help|Muestra el contenido de la ayuda para el comando show.|
|--with-status string|Solo muestra la lista de las transferencias de un trabajo con este estado. Valores disponibles: Started (Iniciado), Success (Correcto), Failed (Con errores)|

## <a name="options-inherited-from-parent-commands"></a>Opciones heredadas de comandos primarios

|Opción|DESCRIPCIÓN|
|---|---|
|--cap-mbps uint32|Limita la velocidad de transferencia, en megabits por segundo. El rendimiento en un momento dado puede variar ligeramente del límite. Si esta opción se establece en cero o se omite, el rendimiento no se limita.|
|--output-type string|Formato de la salida del comando. Las opciones incluyen: text, json. El valor predeterminado es "text".|

## <a name="see-also"></a>Otras referencias

- [azcopy jobs](storage-ref-azcopy-jobs.md)
