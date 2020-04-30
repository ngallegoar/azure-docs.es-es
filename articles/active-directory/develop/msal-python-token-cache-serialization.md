---
title: Serialización de la memoria caché de tokens personalizados (MSAL Python) | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo serializar la memoria caché de tokens en MSAL para Python
services: active-directory
author: rayluo
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/13/2019
ms.author: rayluo
ms.reviewer: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 9c6edd0b3cfd6620f04553f9f6dfe89f1c7b7024
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81536206"
---
# <a name="custom-token-cache-serialization-in-msal-for-python"></a>Serialización de la memoria caché de tokens personalizados en MSAL para Python

En MSAL para Python, se proporciona de forma predeterminada una memoria caché de tokens en memoria que se conserva durante la sesión de aplicación, cuando se crea una instancia de [ClientApplication](https://msal-python.readthedocs.io/en/latest/#confidentialclientapplication).

La serialización de la memoria caché de tokens, de modo que las distintas sesiones de la aplicación puedan acceder a ella, no se proporcionan "preestablecidas". Esto se debe a que MSAL para Python se puede usar en tipos de aplicaciones que no tienen acceso al sistema de archivos, como aplicaciones web. Para tener una memoria caché de tokens persistente en una aplicación MSAL para Python, debe proporcionar la serialización de la memoria caché de tokens personalizados.

Las estrategias para serializar la memoria caché de tokens difiere en función de si va a escribir una aplicación cliente pública (escritorio) o una aplicación cliente confidencial (aplicación web, API web o aplicación de demonio).

## <a name="token-cache-for-a-public-client-application"></a>Caché de tokens para una aplicación cliente pública

Las aplicaciones cliente públicas se ejecutan en el dispositivo de un usuario y administran los tokens para un solo usuario. En este caso, puede serializar toda la memoria caché en un archivo. Recuerde proporcionar el bloqueo de archivos si su aplicación o alguna otra pueden acceder a la memoria caché simultáneamente. Para ver un ejemplo sencillo de cómo serializar una memoria caché de tokens en un archivo sin bloqueo, vea el ejemplo de la documentación de referencia de la clase [SerializableTokenCache](https://msal-python.readthedocs.io/en/latest/#msal.SerializableTokenCache).

## <a name="token-cache-for-a-web-app-confidential-client-application"></a>Caché de tokens para una aplicación web (aplicación cliente confidencial)

En el caso de aplicaciones web o de API web, puede usar la sesión, una caché en Redis o una base de datos para almacenar la caché de tokens. Debe haber una caché de tokens por usuario (por cuenta), por lo que debe asegurarse de que serialice la memoria caché de tokens por cuenta.

## <a name="next-steps"></a>Pasos siguientes

Consulte [ms-identity-python-webapp](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/master/app.py#L64-L72) para ver un ejemplo de cómo usar la caché de tokens para una aplicación web o una API web de Windows o Linux. El ejemplo es para una aplicación web que llama a Microsoft Graph API.
