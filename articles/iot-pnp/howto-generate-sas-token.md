---
title: Generación de un token de seguridad para acceder al repositorio de IoT Plug and Play (versión preliminar) | Microsoft Docs
description: Genere un token de firma de acceso compartido para usarlo al acceder a un repositorio del modelo de IoT Plug and Play (versión preliminar) mediante programación.
author: Philmea
ms.author: philmea
ms.date: 12/27/2019
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: f008627317588467d731ccc03aec7738f58e46e0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80159207"
---
# <a name="generate-sas-token"></a>Generación de un token de SAS

En esta guía paso a paso se muestra cómo generar mediante programación un token de firma de acceso compartido (SAS) para usarlo con las API del repositorio de modelos de IoT Plug and Play (versión preliminar).

## <a name="python"></a>Python

En el fragmento de código siguiente se muestra cómo generar un token de SAS mediante Python:

```python
from base64 import b64decode, b64encode
from hashlib import sha256
from hmac import digest
from time import time
from urllib.parse import quote_plus, urlencode

def calculate_sas_token(hostname, repo_id, key_name, key, expiry_in_second):
    expire = int(time() + expiry_in_second)
    sign_value = "{0}\n{1}\n{2}".format(repo_id, quote_plus(hostname), expire)
    sig = b64encode(digest(b64decode(key), sign_value.encode("utf-8"), sha256))
    token = "SharedAccessSignature " + urlencode({
        "sr": hostname, 
        "sig": sig,
        "se": str(expire),
        "skn": key_name,
        "rid": repo_id
        })
    return token
```

## <a name="c"></a>C\#

En el fragmento de código siguiente se muestra cómo generar un token de SAS mediante C\#:

```csharp
public static string generateSasToken(string hostName, string repoId, string key, string keyName, int expiryInSeconds = 3600)
{
    TimeSpan fromEpochStart = DateTime.UtcNow - new DateTime(1970, 1, 1);
    string expiry = Convert.ToString((int)fromEpochStart.TotalSeconds + expiryInSeconds);

    string stringToSign = repoId + "\n" + WebUtility.UrlEncode(hostName) + "\n" + expiry;

    HMACSHA256 hmac = new HMACSHA256(Convert.FromBase64String(key));
    string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));

    string token = String.Format(
        CultureInfo.InvariantCulture,
        "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}&rid={4}",
        WebUtility.UrlEncode(hostName),
        WebUtility.UrlEncode(signature),
        expiry,
        keyName,
        repoId);

    return token;
}
```

## <a name="use-the-sas-token"></a>Uso del token de SAS

Después de generar un token de SAS, puede usarlo para realizar una solicitud HTTP POST. Por ejemplo:

```text
POST https:///models/{modelId}?repositoryId={repositoryId}&api-version=2019-07-01-preview
```

Si proporciona a un cliente un token de SAS, este no tiene la clave principal del recurso y no puede invertir el hash para obtenerla. Un token de SAS le proporciona control sobre los recursos a los que puede acceder el cliente y durante cuánto tiempo puede hacerlo. Cuando se cambia la clave principal de la directiva, se invalidan los tokens de SAS creados a partir de ella.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a generar tokens de seguridad para acceder a los repositorios de modelo de IoT Plug and Play (versión preliminar), el siguiente paso que le recomendamos es obtener más información en la [Guía para desarrolladores del modelado de IoT Plug and Play (versión preliminar)](concepts-developer-guide.md).
