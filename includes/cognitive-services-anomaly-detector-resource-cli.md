---
author: aahill
ms.author: aahi
ms.service: cognitive-services
ms.topic: include
ms.date: 10/08/2019
ms.openlocfilehash: 5089af4a4e1714d49b844a1b6823487a3f6a8dcf
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74483001"
---
Comience a usar el servicio Anomaly Detector mediante la creación de uno de los recursos de Azure que se indican a continuación.

* <a href="https://azure.microsoft.com/try/cognitive-services/#decision" target="_blank" rel="noopener">Cree un recurso de prueba (se abre en una pestaña nueva)</a>:
    * No se necesita ninguna suscripción de Azure: 
    * Válido durante siete días de forma gratuita. Después de suscribirse, tendrá una clave de prueba y un punto de conexión disponible en el [sitio web de Azure](https://azure.microsoft.com/try/cognitive-services/my-apis/). 
    * Esta es una buena opción si desea probar Anomaly Detector, pero no tiene una suscripción de Azure.

* <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector" target="_blank" rel="noopener">Cree un recurso de Anomaly Detector (se abre en una nueva pestaña)</a>:
    * Disponible en Azure Portal hasta que lo elimine.
    * Use el plan de tarifa gratuito para probar el servicio y actualícelo más adelante a un nivel de pago para producción.



### <a name="create-an-environment-variable"></a>Creación de una variable de entorno

>[!NOTE]
> Los puntos de conexión de los recursos creados que no son de prueba usan desde el 1 de julio de 2019 el formato de subdominio personalizado que se muestra a continuación. Para más información y para obtener una lista completa de los puntos de conexión regionales, consulte [Nombres de subdominios personalizados para Cognitive Services.](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains) 

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