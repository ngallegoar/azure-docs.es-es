---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 03/02/2020
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 6fc1f4152b2e16e1597c018e5af6e0245b075c3b
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "78255819"
---
## <a name="robots933456-in-logs"></a>robots933456 en registros

Puede ver el mensaje siguiente en los registros del contenedor:

```
2019-04-08T14:07:56.641002476Z "-" - - [08/Apr/2019:14:07:56 +0000] "GET /robots933456.txt HTTP/1.1" 404 415 "-" "-"
```

Puede omitir este mensaje sin problemas. `/robots933456.txt` es una ruta de acceso de la dirección URL ficticia que utiliza App Service para comprobar si el contenedor es capaz de atender las solicitudes. Una respuesta 404 simplemente indica que la ruta de acceso no existe, pero permite a App Service saber que el contenedor está en buen estado y listo para responder a las solicitudes.

