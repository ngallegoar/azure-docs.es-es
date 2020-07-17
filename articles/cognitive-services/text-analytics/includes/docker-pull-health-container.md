---
title: Docker pull para el contenedor de salud
titleSuffix: Azure Cognitive Services
description: Comando docker pull para el contenedor de Text Analytics for Health
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 07/07/2020
ms.author: aahi
ms.openlocfilehash: 4c33952ed56399b94a7db1088cb60919a4f88137
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86108984"
---
Rellene y envíe el [formulario de solicitud de contenedores de Cognitive Services](https://aka.ms/cognitivegate) para solicitar acceso al contenedor.

[!INCLUDE [Request access to the container registry](../../../../includes/cognitive-services-containers-request-access-only.md)]

Use el comando de inicio de sesión de Docker con las credenciales proporcionadas en el correo electrónico de incorporación para conectarse a nuestro registro de contenedor privado para contenedores de Cognitive Services.

```bash
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Use el comando [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) para descargar esta imagen de contenedor de nuestro registro de contenedores privados.

```
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-healthcare:latest
```
