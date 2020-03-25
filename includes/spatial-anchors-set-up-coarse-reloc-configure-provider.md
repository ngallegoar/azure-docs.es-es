---
author: bucurb
ms.author: bobuc
ms.date: 09/18/2019
ms.service: azure-spatial-anchors
ms.topic: include
ms.openlocfilehash: a3db5535e65e036ab10c35b7bc066a6286a9cad7
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "76545230"
---
## <a name="configure-the-sensor-fingerprint-provider"></a>Configuración del proveedor de huellas digitales del sensor

Para empezar, es preciso crear y configurar un proveedor de huellas digitales del sensor. Este proveedor de huella digital del sensor se encargará de leer los sensores específicos de la plataforma en el dispositivo y de convertir sus lecturas en una representación común, que es la que usa la sesión del anclaje espacial en la nube.