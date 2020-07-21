---
title: Variables de entorno
description: Establecimiento de variables de entorno
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 05/06/2020
ms.author: pafarley
ms.openlocfilehash: d63e902a59411a549235c955a39d7dbc4be068ba
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86156479"
---
Con la clave y el punto de conexión del recurso que ha creado, cree dos variables de entorno para la autenticación:

* `FORM_RECOGNIZER_KEY`: la clave de recurso para autenticar las solicitudes.
* `FORM_RECOGNIZER_ENDPOINT`: el punto de conexión del recurso para enviar solicitudes de API. Tendrá el siguiente aspecto: 
  * `https://<your-custom-subdomain>.cognitiveservices.azure.com`

>[!NOTE]
> Los puntos de conexión de los recursos creados después del 1 de julio de 2019 usan el formato de subdominio personalizado que se muestra a continuación. Para más información y para obtener una lista completa de los puntos de conexión regionales, consulte [Nombres de subdominios personalizados para Cognitive Services.](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains) 

Siga estas instrucciones para establecer las variables de entorno en el sistema operativo.

#### <a name="windows"></a>[Windows](#tab/windows)

```console
setx FORM_RECOGNIZER_KEY <replace-with-your-form-recognizer-key>
setx FORM_RECOGNIZER_ENDPOINT <replace-with-your-form-recognizer-endpoint>
```

Después de agregar las variables de entorno, cierre y vuelva a abrir la ventana de la consola.

#### <a name="linux"></a>[Linux](#tab/linux)

```bash
export FORM_RECOGNIZER_KEY=<replace-with-your-product-name-key>
export FORM_RECOGNIZER_ENDPOINT=<replace-with-your-product-name-endpoint>
```

Después de agregar la variable de entorno, ejecute `source ~/.bashrc` desde la ventana de consola para que los cambios surtan efecto.

#### <a name="macos"></a>[macOS](#tab/unix)

Edite `.bash_profile` y agregue la variable de entorno:

```bash
export FORM_RECOGNIZER_KEY=<replace-with-your-product-name-key>
export FORM_RECOGNIZER_ENDPOINT=<replace-with-your-product-name-endpoint>
```

Después de agregar las variables de entorno ejecute `source .bash_profile` en la ventana de consola para que los cambios surtan efecto.
***
