---
title: Adición de una barra de herramientas de dibujo a Azure Maps| Microsoft Docs
description: Cómo agregar una barra de herramientas de dibujo a un mapa mediante Azure Maps SDK Web
author: walsehgal
ms.author: v-musehg
ms.date: 09/04/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 6bc754c9a4f333da85e57c5ad9780da8df93e895
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70309734"
---
# <a name="add-a-drawing-tools-toolbar-to-a-map"></a>Agregar una barra de herramientas de dibujo a un mapa

Este artículo muestra cómo usar el módulo de herramientas de dibujo y mostrar la barra de herramientas de Dibujo en el mapa. El control [DrawingToolbar](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest) agrega la barra de herramientas de dibujo en el mapa. Aprenderá a crear mapas con solo una y todas las herramientas de dibujo y cómo personalizar la representación de las formas de dibujo en el administrador de dibujos.

## <a name="add-drawing-toolbar"></a>Incorporación de la barra de herramientas Dibujo

En el código siguiente crea una instancia del administrador de dibujos y muestra la barra de herramientas en el mapa.

```Javascript
//Create an instance of the drawing manager and display the drawing toolbar.
drawingManager = new atlas.drawing.DrawingManager(map, {
        toolbar: new atlas.control.DrawingToolbar({
            position: 'top-right',
            style: 'dark'
        })
    });
```

A continuación se muestra el ejemplo de código de ejecución completo de la funcionalidad anterior:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Incorporación de la barra de herramientas Dibujo" src="//codepen.io/azuremaps/embed/ZEzLeRg/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/ZEzLeRg/'>Adición de una barra de herramientas de dibujo</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="limit-displayed-toolbar-options"></a>Limite las opciones de la barra de herramientas mostradas

En el código siguiente crea una instancia del administrador de dibujos y muestra la barra de herramientas solo con una herramienta de dibujo de polígono en el mapa. 

```Javascript
//Create an instance of the drawing manager and display the drawing toolbar with polygon drawing tool.
drawingManager = new atlas.drawing.DrawingManager(map, {
        toolbar: new atlas.control.DrawingToolbar({
            position: 'top-right',
            style: 'light',
            buttons: ["draw-polygon"]
        })
    });
```

A continuación se muestra el ejemplo de código de ejecución completo de la funcionalidad anterior:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Agregar una herramienta de dibujo de polígono" src="//codepen.io/azuremaps/embed/OJLWWMy/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/OJLWWMy/'>Adición de una herramienta de dibujo de polígono</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="change-drawing-rendering-style"></a>Cambiar el estilo de representación de dibujo

El código siguiente obtiene las capas de representación del administrador de dibujos y modifica sus opciones para cambiar el estilo de representación del dibujo. En este caso, los puntos se representarán con un icono de marcador azul, las líneas serán de color rojo y de cuatro píxeles de ancho, los polígonos tendrán un color de relleno verde y un contorno naranja.

```Javascript
var layers = drawingManager.getLayers();
    layers.pointLayer.setOptions({
        iconOptions: {
            image: 'marker-blue'
        }
    });
    layers.lineLayer.setOptions({
        strokeColor: 'red',
        strokeWidth: 4
    });
    layers.polygonLayer.setOptions({
        fillColor: 'green'
    });
    layers.polygonOutlineLayer.setOptions({
        strokeColor: 'orange'
    });
```

A continuación se muestra el ejemplo de código de ejecución completo de la funcionalidad anterior:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Cambiar el estilo de representación de dibujo" src="//codepen.io/azuremaps/embed/OJLWpyj/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/OJLWpyj/'>Cambiar el estilo de representación de dibujo</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>)en <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Barra de herramientas de dibujo](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest)

> [!div class="nextstepaction"]
> [Administrador de dibujo](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest)