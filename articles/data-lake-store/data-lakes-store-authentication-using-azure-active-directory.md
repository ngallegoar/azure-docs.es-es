---
title: Autenticación en Azure Data Lake Storage Gen1 con Azure Active Directory | Microsoft Docs
description: Obtenga información sobre la autenticación con Azure Data Lake Storage Gen1 mediante Azure Active Directory
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: f83cf183bee930dd07c707b0eb49125cecd70b84
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "60193601"
---
# <a name="authentication-with-azure-data-lake-storage-gen1-using-azure-active-directory"></a>Autenticación en Azure Data Lake Storage Gen1 con Azure Active Directory

Azure Data Lake Storage Gen1 usa Azure Active Directory para la autenticación. Antes de crear una aplicación que funcione con Azure Data Lake Storage Gen1, debe decidir cómo autenticar la aplicación con Azure Active Directory (Azure AD).

## <a name="authentication-options"></a>Opciones de autenticación

* **Autenticación de usuario final**: las credenciales de Azure para usuarios finales se utilizan para la autenticación en Data Lake Storage Gen1. La aplicación que crea para trabajar con Data Lake Storage Gen1 solicita estas credenciales de usuario. Como resultado, este mecanismo de autenticación es *interactivo* y la aplicación se ejecuta en el contexto del usuario que ha iniciado sesión. Para más información e instrucciones, consulte [Autenticación de usuario final en Data Lake Storage Gen1](data-lake-store-end-user-authenticate-using-active-directory.md).

* **Autenticación entre servicios**: utilice esta opción si desea que una aplicación se autentique en Data Lake Storage Gen1. En tales casos, cree una aplicación de Azure Active Directory (AD) y use la clave de la aplicación de Azure AD para la autenticación en Data Lake Storage Gen1. Como resultado, este mecanismo de autenticación es *no interactivo*. Para más información e instrucciones, consulte [Autenticación entre servicios en Data Lake Storage Gen1](data-lake-store-service-to-service-authenticate-using-active-directory.md).

En la tabla siguiente se indica cómo se admiten los mecanismos de autenticación de usuario final y entre servicios en Data Lake Storage Gen1. Así es como se debe interpretar la tabla.

* El símbolo ✔* indica que la opción de autenticación es compatible y se vincula a un artículo en el que se muestra cómo usar la opción de autenticación. 
* El símbolo ✔ indica que se admite la opción de autenticación. 
* Las celdas vacías significan que la opción de autenticación no es compatible.


|Use esta opción de autenticación con…                   |.NET         |Java     |PowerShell |Azure CLI | Python   |REST     |
|:---------------------------------------------|:------------|:--------|:----------|:-------------|:---------|:--------|
|Usuario final (sin MFA\*\*)                        |   ✔ |    ✔    |    ✔      |       ✔      |    **[✔*](data-lake-store-end-user-authenticate-python.md#end-user-authentication-without-multi-factor-authentication)** (en desuso)     |    **[✔*](data-lake-store-end-user-authenticate-rest-api.md)**    |
|Usuario final (con MFA)                           |    **[✔*](data-lake-store-end-user-authenticate-net-sdk.md)**        |    **[✔*](data-lake-store-end-user-authenticate-java-sdk.md)**     |    ✔      |       **[✔*](data-lake-store-get-started-cli-2.0.md)**      |    **[✔*](data-lake-store-end-user-authenticate-python.md#end-user-authentication-with-multi-factor-authentication)**     |    ✔    |
|Entre servicios (con clave de cliente)         |    **[✔*](data-lake-store-service-to-service-authenticate-net-sdk.md#service-to-service-authentication-with-client-secret)** |    **[✔*](data-lake-store-service-to-service-authenticate-java.md)**    |    ✔      |       ✔      |    **[✔*](data-lake-store-service-to-service-authenticate-python.md#service-to-service-authentication-with-client-secret-for-account-management)**     |    **[✔*](data-lake-store-service-to-service-authenticate-rest-api.md)**    |
|Entre servicios (con certificado de cliente) |    **[✔*](data-lake-store-service-to-service-authenticate-net-sdk.md#service-to-service-authentication-with-certificate)**        |    ✔    |    ✔      |       ✔      |    ✔     |    ✔    |

<i>* Haga clic en el símbolo <b>✔\*</b>. Es un vínculo.</i><br>
<i>** MFA hace referencia a la autenticación multifactor</i>.

Vea [Escenarios de autenticación para Azure AD](../active-directory/develop/authentication-scenarios.md) para obtener más información sobre cómo usar Azure Active Directory para la autenticación.

## <a name="next-steps"></a>Pasos siguientes

* [Autenticación de usuario final](data-lake-store-end-user-authenticate-using-active-directory.md)
* [Autenticación entre servicios](data-lake-store-service-to-service-authenticate-using-active-directory.md)


