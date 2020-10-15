---
author: burkeholland
ms.service: static-web-apps
ms.topic: include
ms.date: 05/08/2020
ms.author: buhollan
ms.openlocfilehash: 879d0c2e8c4cd66ce04c0298d00f7d6a1987acf5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "83594404"
---
#### <a name="local-proxy"></a>Proxy local

Puede configurar un proxy para la extensión Live Server de Visual Studio Code que enrute todas las solicitudes hechas a `/api` al punto de conexión de la API en ejecución en `http://127.0.0.1:7071/api`.

1. Abra el archivo _.vscode/settings.json_.

1. Agregue la siguiente configuración para especificar un proxy para Live Server.

   ```json
   "liveServer.settings.proxy": {
      "enable": true,
      "baseUri": "/api",
      "proxyUri": "http://127.0.0.1:7071/api"
   }
   ```

   Esta configuración se guarda mejor en el archivo de configuración del proyecto, en lugar de en el archivo de configuración del usuario.

   El uso de la configuración del proyecto garantiza que el proxy no se aplique a todos los demás proyectos abiertos en Visual Studio Code.
