---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 09/28/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: a852807ab685e85b76d26e5b39c99a32f645bbd7
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2019
ms.locfileid: "71838160"
---
Las rutas definidas por el usuario con un destino 0.0.0.0/0 y NSG en GatewaySubnet **are no se admiten**. Se bloqueará la creación de puertas de enlace creadas con esta configuración. Las puertas de enlace requieren acceso a los controladores de administración para que funcionen correctamente.
