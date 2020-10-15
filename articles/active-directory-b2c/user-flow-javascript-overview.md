---
title: JavaScript y versiones de diseño de página
titleSuffix: Azure AD B2C
description: Aprenda a habilitar JavaScript y a usar versiones de diseño de página en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 07/30/2020
ms.custom: project-no-code, devx-track-js
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 50644ad3be226648accba6a2f43d4ea068ff977c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91258853"
---
# <a name="javascript-and-page-layout-versions-in-azure-active-directory-b2c"></a>JavaScript y versiones de diseño de página en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure AD B2C proporciona un conjunto de contenido empaquetado que contiene HTML, CSS y JavaScript para los elementos de la interfaz de usuario de los flujos de usuarios y las directivas personalizadas.

Para habilitar JavaScript para las aplicaciones:

* Habilítelo en el flujo de usuario mediante Azure Portal
* Seleccione un [diseño de página](page-layout.md)
* Use [b2clogin.com](b2clogin.md) en las solicitudes

Si tiene pensado habilitar el código cliente de [JavaScript](javascript-samples.md), los elementos en los que se basa JavaScript deben ser inmutables. Si no son inmutables, cualquier cambio podría provocar un comportamiento inesperado en las páginas de usuario. Para evitar estos problemas, exija el uso de un diseño de página y especifique una versión de este para garantizar que las definiciones de contenido en las que ha basado JavaScript sean inmutables. Incluso si no piensa habilitar JavaScript, puede especificar una versión del diseño de página para las páginas.

## <a name="enable-javascript"></a>Habilitar JavaScript

En **Propiedades** del flujo de usuario puede habilitar JavaScript. La habilitación de JavaScript también exige el uso de un diseño de página. Después, puede establecer la versión del diseño de página para el flujo de usuario tal y como se describe en la sección siguiente.

![Página de propiedades del flujo de usuario con la opción Habilitar JavaScript resaltada](media/user-flow-javascript-overview/javascript-settings.png)

## <a name="select-a-page-layout-version"></a>Selección de una versión de diseño de página

Tanto si habilita JavaScript como si no lo hace en las propiedades de un flujo de usuario, puede especificar una versión del diseño de página para las páginas del flujo de usuario. Abra el flujo de usuario y seleccione **Diseños de página**. En **Nombre del diseño**, seleccione una página del flujo de usuario y elija la **versión del diseño de página (versión preliminar)** .

Para información sobre los distintos diseños de página, consulte el [registro de cambios en los diseños de página](page-layout.md).

![Configuración del diseño de página del portal que muestra el menú desplegable de la versión del diseño de página](media/user-flow-javascript-overview/page-layout-version.png)

[!INCLUDE [active-directory-b2c-javascript-guidelines](../../includes/active-directory-b2c-javascript-guidelines.md)]

## <a name="next-steps"></a>Pasos siguientes

Puede encontrar ejemplos de uso de JavaScript en [Ejemplos de JavaScript para usar en Azure Active Directory B2C](javascript-samples.md).
