---
title: Variables de entorno de Anomaly Detector
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/30/2020
ms.author: mbullwin
ms.openlocfilehash: 5148114da7fcc654b1f0af431440ce74c7bc8d31
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2020
ms.locfileid: "94371903"
---
### <a name="create-an-environment-variable"></a>Creación de una variable de entorno

>[!NOTE]
> Los puntos de conexión de los recursos creados que no son de prueba usan desde el 1 de julio de 2019 el formato de subdominio personalizado que se muestra a continuación. Para más información y para obtener una lista completa de los puntos de conexión regionales, consulte [Nombres de subdominios personalizados para Cognitive Services.](../../cognitive-services-custom-subdomains.md) 

Con la clave y el punto de conexión del recurso que ha creado, cree dos variables de entorno para la autenticación:

* `ANOMALY_DETECTOR_KEY`: la clave de recurso para autenticar las solicitudes.
* `ANOMALY_DETECTOR_ENDPOINT`: el punto de conexión del recurso para enviar solicitudes de API. Tendrá el siguiente aspecto: 
  * `https://<your-custom-subdomain>.api.cognitive.microsoft.com` 

Siga las instrucciones adecuadas para su sistema operativo.

#### <a name="windows"></a>[Windows](#tab/windows)

```console
setx ANOMALY_DETECTOR_KEY <replace-with-your-anomaly-detector-key>
setx ANOMALY_DETECTOR_ENDPOINT <replace-with-your-anomaly-detector-endpoint>
```

Después de agregar la variable de entorno, reinicie la ventana de la consola.

#### <a name="linux"></a>[Linux](#tab/linux)

```bash
export ANOMALY_DETECTOR_KEY=<replace-with-your-anomaly-detector-key>
export ANOMALY_DETECTOR_ENDPOINT=<replace-with-your-anomaly-detector-endpoint>
```

Después de agregar la variable de entorno, ejecute `source ~/.bashrc` desde la ventana de consola para que los cambios surtan efecto.

#### <a name="macos"></a>[macOS](#tab/unix)

Edite `.bash_profile` y agregue la variable de entorno:

```bash
export ANOMALY_DETECTOR_KEY=<replace-with-your-anomaly-detector-key>
export ANOMALY_DETECTOR_ENDPOINT=<replace-with-your-anomaly-detector-endpoint>
```

Después de agregar la variable de entorno, ejecute `source .bash_profile` desde la ventana de consola para que los cambios surtan efecto.

***