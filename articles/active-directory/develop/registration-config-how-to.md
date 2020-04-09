---
title: Configuración de extremos
description: Aprenda a buscar los puntos de conexión de autenticación de una aplicación personalizada que esté desarrollando o registrando con Azure AD.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.topic: conceptual
ms.date: 07/15/2019
ms.author: ryanwi
ms.openlocfilehash: 28d85736019a6fea6d977d813fdc1c9be6429748
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80883277"
---
# <a name="how-to-configure-endpoints"></a>Configuración de puntos de conexión

Puede buscar los puntos de conexión de autenticación para su aplicación en [Azure Portal](https://portal.azure.com).

-   Acceda a [Azure Portal](https://portal.azure.com).

-   En el panel de navegación izquierdo, haga clic en **Azure Active Directory**.

-   Haga clic en **Registros de aplicaciones** y elija **Puntos de conexión**.

-   Se abrirá la página **Puntos de conexión** con una lista de todos los puntos de conexión de autenticación del inquilino.

-   Utilice el punto de conexión que corresponda al protocolo de autenticación que esté utilizando, junto con el identificador de aplicación, para diseñar la solicitud de autenticación específica de la aplicación.

## <a name="next-steps"></a>Pasos siguientes
[Guía del desarrollador de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)
