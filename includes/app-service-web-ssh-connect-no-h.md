---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 03/29/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 458cd36a35ea37b2a317fe98fdeb5acc69a36ce8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91822823"
---
Para que una sesión de SSH directa sea abierta con el contenedor, la aplicación debe estar en ejecución.

Pegue la siguiente dirección URL en el explorador y reemplace `<app-name>` por el nombre de la aplicación:

```
https://<app-name>.scm.azurewebsites.net/webssh/host
```

Si aún no está autenticado, será preciso que se autentique con su suscripción a Azure para conectarse. Una vez autenticado, verá un shell del explorador en el que puede ejecutar comandos dentro del contenedor.

![Conexión SSH](./media/app-service-web-ssh-connect-no-h/app-service-linux-ssh-connection.png)
