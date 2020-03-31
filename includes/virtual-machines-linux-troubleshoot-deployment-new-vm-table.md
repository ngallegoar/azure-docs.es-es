---
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: d93de4ed758afb5e951bb5e19f4f7adb290e461c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "67186318"
---
En la tabla siguiente se enumeran la posibles combinaciones de carga y captura de imágenes del sistema operativo de Linux generalizadas y especializadas. Las combinaciones que se procesarán sin errores se indican mediante una letra Y y las que generarán errores se indican mediante una letra N. En la tabla siguiente, se proporcionan las causas y soluciones para los diferentes errores que se va a encontrar.

| SO | Cargar espec. | Cargar gen. | Capturar espec. | Capturar gen. |
| --- | --- | --- | --- | --- |
| Linux gen. |N<sup>1</sup> |Y |N<sup>3</sup> |Y |
| Linux espec. |Y |N<sup>2</sup> |Y |N<sup>4</sup> |

