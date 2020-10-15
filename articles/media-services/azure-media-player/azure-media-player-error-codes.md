---
title: Códigos de error de Azure Media Player
description: Un documento de referencia sobre códigos de error de Azure Media Player.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: error-reference
ms.date: 04/20/2020
ms.openlocfilehash: 13d804ec39c3d7753d4ee04962a88d4451fb04d9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "81725905"
---
# <a name="error-codes"></a>Códigos de error #

Cuando una reproducción no se puede iniciar o se detiene, se desencadena un evento de error y la función `error()` devuelve un código y un mensaje opcional que se usa para ayudar al desarrollador a obtener más detalles. `error().message` no es el mensaje que se muestra al usuario.  El mensaje que se muestra al usuario se basa en los bits 27-20 de `error().code`, consulte la tabla siguiente.

```javascript

    var myPlayer = amp('vid1');
    myPlayer.addEventListener('error', function() {
        var errorDetails = myPlayer.error();
        var code = errorDetails.code;
        var message = errorDetails.message;
    }
```

## <a name="error-codes-bits-31-28-4-bits"></a>Códigos de error, bits [31-28] (4 bits) ##

Describe el área del error.

- 0: Desconocido
- 1: AMP
- 2: AzureHtml5JS
- 3: FlashSS
- 4: SilverlightSS
- 5: Html5
- 6: Html5FairPlayHLS

## <a name="error-codes-bits-27-0-28-bits"></a>Códigos de error, bits [27-0] (28 bits) ##

Describe los detalles del error. Los bits 27-20 proporcionan un alto nivel, mientras que los bits 19-0 proporcionan más detalles si están disponibles.


| amp.errorCode.[name] | Códigos, bits [27-0] (28 bits) | Descripción |
|---|---:|---|
| **MEDIA_ERR_ABORTED errors range (0x0100000 - 0x01FFFFF)** | | |
| abortedErrUnknown | 0x0100000 | Error de anulación genérico |
| abortedErrNotImplemented | 0x0100001 | Error de anulación, no implementado |
| abortedErrHttpMixedContentBlocked | 0x0100002 | Error de anulación, contenido mixto bloqueado. Normalmente se produce cuando se carga una secuencia `http://` desde una página `https://`. |
| **MEDIA_ERR_NETWORK errors start value (0x0200000 - 0x02FFFFF)** | | |
| networkErrUnknown | 0x0200000 | Error de red genérico |
| networkErrHttpBadUrlFormat | 0x0200190 | Respuesta de errores Http 400 |
| networkErrHttpUserAuthRequired | 0x0200191 | Respuesta de errores Http 401 |
| networkErrHttpUserForbidden | 0x0200193 | Respuesta de errores Http 403 |
| networkErrHttpUrlNotFound | 0x0200194 | Respuesta de errores Http 404 |
| networkErrHttpNotAllowed | 0x0200195 | Respuesta de errores Http 405 |
| networkErrHttpGone | 0x020019A | Respuesta de errores Http 410 |
| networkErrHttpPreconditionFailed | 0x020019C | Respuesta de errores Http 412 |
| networkErrHttpInternalServerFailure | 0x02001F4 | Respuesta de errores Http 500
| networkErrHttpBadGateway | 0x02001F6 | Respuesta de errores Http 502 |
| networkErrHttpServiceUnavailable | 0x02001F7 | Respuesta de errores Http 503 |
| networkErrHttpGatewayTimeout | 0x02001F8 | Respuesta de errores Http 504 |
| networkErrTimeout | 0x0200258 | Error de tiempo de espera en la red
| networkErrErr | 0x0200259 | Respuesta de errores de conexión de red |
| **MEDIA_ERR_DECODE errors (0x0300000 - 0x03FFFFF)** | | |
| decodeErrUnknown | 0x0300000 | Error de descodificación genérico |
| **MEDIA_ERR_SRC_NOT_SUPPORTED errors (0x0400000 - 0x04FFFFF)** | | |
| srcErrUnknown | 0x0400000 | Error genérico de origen no admitido |
| srcErrParsePresentation | 0x0400001 | Error de análisis de presentación |
| srcErrParseSegment | 0x0400002 | Error de análisis de segmento |
| srcErrUnsupportedPresentation | 0x0400003 | No se admite la presentación |
| srcErrInvalidSegment | 0x0400004 | Segmento no válido |
| **MEDIA_ERR_ENCRYPTED errors start value(0x0500000 - 0x05FFFFF)** | | |
| encryptErrUnknown | 0x0500000 | Error de cifrado genérico | 
| encryptErrDecrypterNotFound | 0x0500001 | No se encuentra el descifrador |
| encryptErrDecrypterInit | 0x0500002 | Error de inicialización del descifrador |
| encryptErrDecrypterNotSupported | 0x0500003 | No se admite el descifrador |
| encryptErrKeyAcquire | 0x0500004 | Error en la adquisición de la clave |
| encryptErrDecryption | 0x0500005 | Error al descifrar el segmento |
| encryptErrLicenseAcquire | 0x0500006 | Error en la adquisición de la licencia |
| **SRC_PLAYER_MISMATCH errors start value(0x0600000 - 0x06FFFFF)** | | |
| srcPlayerMismatchUnknown | 0x0600000 | No hay tecnología coincidente genérica de reproducción para reproducir el origen |
| srcPlayerMismatchFlashNotInstalled | 0x0600001 |El complemento de Flash no se ha instalado, si se instala se podrá reproducir el origen. *O* Flash 30 está instalado y reproduciendo el contenido de AES.  Si este es el caso, pruebe con otro explorador. No se admite Flash 30 a partir del 7 de junio. Consulte los [problemas conocidos](azure-media-player-known-issues.md) para más información. Nota: Si se produce 0x00600003, tanto Flash como Silverlight no están instalados, si se especifican en techOrder.|
| srcPlayerMismatchSilverlightNotInstalled | 0x0600002 | El complemento de Silverlight no se ha instalado, si se instala se podrá reproducir el origen. Nota: Si se produce 0x00600003, tanto Flash como Silverlight no están instalados, si se especifican en techOrder. |
| | 0x00600003 | Tanto Flash como Silverlight no están instalados, si se especifican en techOrder. |
| **Errores desconocidos (0x0FF00000)** | | |
| errUnknown | 0xFF00000 | Errores desconocidos |

## <a name="user-error-messages"></a>Mensajes de error del usuario ##

El mensaje de usuario que se muestra está basado en los bits 27-20 del código de error.

- MEDIA_ERR_ABORTED (1): "Ha anulado la reproducción de vídeo"
- MEDIA_ERR_NETWORK (2): "Un error de red hizo que la descarga de vídeo no se realizara correctamente".
- MEDIA_ERR_DECODE (3): "La reproducción de vídeo se ha anulado debido a un problema de daños o porque el vídeo usó características que el explorador no admitía".
- MEDIA_ERR_SRC_NOT_SUPPORTED (4): "No se pudo cargar el vídeo, ya sea porque se produjo un error en el servidor o la red o porque no se admite el formato".
- MEDIA_ERR_ENCRYPTED (5): "El vídeo está cifrado y no tenemos las claves para descifrarlo".
- SRC_PLAYER_MISMATCH (6): "No se encontró ningún origen compatible para este vídeo".
- MEDIA_ERR_UNKNOWN (0xFF): "Se ha producido un error desconocido".

## <a name="examples"></a>Ejemplos ##

### <a name="0x10600001"></a>0x10600001 ##

El error "No se encontró ningún origen compatible para este vídeo" se muestra al usuario final.

No hay ningún reproductor que pueda reproducir los orígenes solicitados, pero si está instalado el complemento de Flash, es probable que se reproduzca un origen.  

### <a name="0x20200194"></a>0x20200194 ###

El error "Un error de red hizo que la descarga de vídeo no se realizara correctamente". se muestra al usuario final.

Error de AzureHtml5JS al reproducir desde una respuesta de http 404.

### <a name="categorizing-errors"></a>Errores de categorización ###

```javascript
    if(myPlayer.error().code & amp.errorCode.abortedErrStart) {
        // MEDIA_ERR_ABORTED errors
    }
    else if(myPlayer.error().code & amp.errorCode.networkErrStart) {
        // MEDIA_ERR_NETWORK errors
    }
    else if(myPlayer.error().code & amp.errorCode.decodeErrStart) {
        // MEDIA_ERR_DECODE errors
    }
    else if(myPlayer.error().code & amp.errorCode.srcErrStart) {
        // MEDIA_ERR_SRC_NOT_SUPPORTED errors
    }
    else if(myPlayer.error().code & amp.errorCode.encryptErrStart) {
        // MEDIA_ERR_ENCRYPTED errors
    }
    else if(myPlayer.error().code & amp.errorCode.srcPlayerMismatchStart) {
        // SRC_PLAYER_MISMATCH errors
    }
    else {
        // unknown errors
    }
```

### <a name="catching-a-specific-error"></a>Detección de un error específico ###

El código siguiente solo detecta los errores 404:

```javascript
    if(myPlayer.error().code & amp.errorCode.networkErrHttpUrlNotFound) {
        // all http 404 errors
    }
```

## <a name="next-steps"></a>Pasos siguientes ##

- [Inicio rápido de Azure Media Player](azure-media-player-quickstart.md)