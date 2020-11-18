---
title: 'API REST de Azure Active Directory: prueba con Fiddler'
description: Uso de Fiddler para probar la API REST de Azure App Configuration
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 3766567fe58e8d2eb86556d3defa7a85efd9b2fb
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423775"
---
# <a name="test-the-azure-app-configuration-rest-api-using-fiddler"></a>Prueba de la API REST de Azure App Configuration con Fiddler

Para probar la API REST con [Fiddler](https://www.telerik.com/fiddler), deberá incluir los encabezados HTTP necesarios para la [autenticación](./rest-api-authentication-hmac.md) en las solicitudes. Aquí se muestra cómo configurar Fiddler para probar la API REST mediante la generación automática de los encabezados de autenticación:

1. Asegúrese de que TLS 1.2 es un protocolo permitido:
    1. Vaya a **Tools** > **Options** > **HTTPS** (Herramientas > Opciones > HTTPS).
    1. Asegúrese de que la opción **Decrypt HTTPS traffic** (Descifrar el tráfico HTTPS) está activada.
    1. En la lista de protocolos, agregue **TLS 1.2** si no está presente.
1. Abra el **editor de scripts de Fiddler** o pulse **Ctrl-R** en Fiddler.
1. Agregue el código siguiente dentro de la clase `Handlers` antes de la función `OnBeforeRequest`.

    ```js
    static function SignRequest(oSession: Session, credential: String, secret: String) {
        var utcNow = DateTimeOffset.UtcNow.ToString("r", System.Globalization.DateTimeFormatInfo.InvariantInfo);
        var contentHash = ComputeSHA256Hash(oSession.RequestBody);
        var stringToSign = oSession.RequestMethod.ToUpperInvariant() + "\n" + oSession.PathAndQuery + "\n" + utcNow +";" + oSession.hostname + ";" + contentHash;
        var signature = ComputeHMACHash(secret, stringToSign);

        oSession.oRequest.headers["x-ms-date"] = utcNow;
        oSession.oRequest.headers["x-ms-content-sha256"] = contentHash;
        oSession.oRequest.headers["Authorization"] = "HMAC-SHA256 Credential=" + credential + "&SignedHeaders=x-ms-date;host;x-ms-content-sha256&Signature=" + signature;
    }

    static function ComputeSHA256Hash(content: Byte[]) {
        var sha256 = System.Security.Cryptography.SHA256.Create();
        try {
            return Convert.ToBase64String(sha256.ComputeHash(content));
        }
        finally {
            sha256.Dispose();
        }
    }

    static function ComputeHMACHash(secret: String, content: String) {
        var hmac = new System.Security.Cryptography.HMACSHA256(Convert.FromBase64String(secret));
        try {
            return Convert.ToBase64String(hmac.ComputeHash(System.Text.Encoding.ASCII.GetBytes(content)));
        }
        finally {
            hmac.Dispose();
        }
    }
    ```

1. Agregue el siguiente código al final de la función `OnBeforeRequest`. Actualice la clave de acceso como indica el comentario TODO.

    ```js
    if (oSession.isFlagSet(SessionFlags.RequestGeneratedByFiddler) &&
        oSession.hostname.EndsWith(".azconfig.io", StringComparison.OrdinalIgnoreCase)) {

        // TODO: Replace the following placeholders with your access key
        var credential = "<Credential>"; // Id
        var secret = "<Secret>"; // Value

        SignRequest(oSession, credential, secret);
    }
    ```
1. Use [Composer de Fiddler](https://docs.telerik.com/fiddler/Generate-Traffic/Tasks/CreateNewRequest) para generar y enviar una solicitud.
