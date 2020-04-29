---
title: Configuración de OpenSSL para Linux
titleSuffix: Azure Cognitive Services
description: Aprenda a configurar OpenSSL para Linux.
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/16/2020
ms.author: jhakulin
ms.openlocfilehash: 42960c25c4124203b64646fdc5cbca833b246e21
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81683160"
---
# <a name="configure-openssl-for-linux"></a>Configuración de OpenSSL para Linux

Cuando usa cualquier versión del SDK de Voz anterior a la 1.9.0, [OpenSSL](https://www.openssl.org) se configura dinámicamente en la versión del sistema host. En versiones posteriores del SDK de Voz, OpenSSL (versión [1.1.1 b](https://mta.openssl.org/pipermail/openssl-announce/2019-February/000147.html)) se vincula estáticamente a la biblioteca principal del SDK de Voz.

Para garantizar la conectividad, compruebe que se han instalado los certificados de OpenSSL en el sistema. Ejecute un comando:
```bash
openssl version -d
```

La salida en los sistemas basados en Ubuntu/Debian debe ser:
```
OPENSSLDIR: "/usr/lib/ssl"
```

Compruebe si existe el subdirectorio `certs` en OPENSSLDIR. En el ejemplo anterior, sería `/usr/lib/ssl/certs`.

* Si `/usr/lib/ssl/certs` existe y contiene muchos archivos de certificado individuales (con la extensión `.crt` o `.pem`), no es necesario realizar más acciones.

* Si OPENSSLDIR es algo distinto de `/usr/lib/ssl` y/o hay un único archivo de paquete de certificados en lugar de varios archivos individuales, debe establecer una variable de entorno SSL adecuada para indicar dónde se pueden encontrar los certificados.

## <a name="examples"></a>Ejemplos

- OPENSSLDIR is `/opt/ssl`. Existe un subdirectorio `certs` con muchos archivos `.crt` o `.pem`.
Establezca la variable de entorno `SSL_CERT_DIR` para que apunte a `/opt/ssl/certs` antes de ejecutar un programa que utilice el SDK de Voz. Por ejemplo:
```bash
export SSL_CERT_DIR=/opt/ssl/certs
```

- OPENSSLDIR es `/etc/pki/tls` (como en los sistemas basados en RHEL o CentOS). Existe un subdirectorio `certs` archivo de paquete de certificados, por ejemplo `ca-bundle.crt`.
Establezca la variable de entorno `SSL_CERT_FILE` para que apunte a ese archivo antes de ejecutar un programa que utilice el SDK de Voz. Por ejemplo:
```bash
export SSL_CERT_FILE=/etc/pki/tls/certs/ca-bundle.crt
```
> [!NOTE]
> También merece la pena mencionar que algunas distribuciones de Linux no tienen definida una variable de entorno TMP o TMPDIR. Como consecuencia, el SDK de Voz descargará la lista de revocación de certificados (CRL) cada vez, en lugar de almacenarla en caché en el disco para reutilizarla hasta que expire. Para mejorar el rendimiento inicial de la conexión, puede [crear una variable de entorno denominada TMPDIR y establecerla en la ruta de acceso del directorio temporal elegido](https://help.ubuntu.com/community/EnvironmentVariables).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Acerca del SDK de Voz](speech-sdk.md)
