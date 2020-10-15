---
title: Docker pull para el contenedor de salud
titleSuffix: Azure Cognitive Services
description: Comando docker pull para el contenedor de Text Analytics for Health
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 07/07/2020
ms.author: aahi
ms.openlocfilehash: 519625f6468372ec7ace523dae7648212f4f3203
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91779482"
---
Rellene y envíe el [formulario de solicitud de contenedores de Cognitive Services](https://aka.ms/csgate) para solicitar acceso al contenedor.
El formulario solicita información acerca del usuario y de su empresa, así como del escenario de usuario para el que se va a usar el contenedor. Después de haber enviado el formulario, el equipo de Azure Cognitive Services lo revisa para asegurarse de que cumple los criterios de acceso al registro de contenedor privado.

> [!IMPORTANT]
> * En el formulario, debe usar una dirección de correo electrónico asociada a un identificador de suscripción de Azure.
> * El recurso de Azure que se usa para ejecutar el contenedor se debe haber creado con el identificador de la suscripción de Azure aprobada. 
> * Compruebe el correo electrónico (bandeja de entrada y carpetas de correo no deseado) para obtener las actualizaciones del estado de la aplicación por parte de Microsoft.

Use el comando de inicio de sesión de Docker con las credenciales proporcionadas en el correo electrónico de incorporación para conectarse a nuestro registro de contenedor privado para contenedores de Cognitive Services.


```Docker
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Use el comando [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) para descargar esta imagen de contenedor de nuestro registro de contenedores privados.

```
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-healthcare:latest
```
