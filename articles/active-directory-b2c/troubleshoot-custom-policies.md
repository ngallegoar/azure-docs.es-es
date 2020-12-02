---
title: Solución de problemas de directivas personalizadas en Azure Active Directory B2C
description: Información sobre los métodos para resolver errores al trabajar con directivas personalizadas en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: troubleshooting
ms.date: 08/13/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 87ea77540e5fcaac0c4231403473d25ebae46aac
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94840260"
---
# <a name="troubleshoot-azure-ad-b2c-custom-policies-and-identity-experience-framework"></a>Solución de problemas de directivas personalizadas de Azure AD B2C y el marco de experiencia de identidad

Si usa directivas personalizadas de Azure Active Directory B2C (Azure AD B2C), es posible que experimente problemas al configurar el marco de experiencia de identidad en el formato XML de su lenguaje de directiva. Aprender a escribir directivas personalizadas es casi como aprender un idioma nuevo. En este artículo se describen algunas herramientas y sugerencias que pueden ayudarle a detectar y solucionar problemas.

Este artículo se centra en la solución de problemas de configuración de directivas personalizadas de Azure AD B2C. No trata sobre las aplicaciones de usuarios de confianza ni sobre su biblioteca de identidades.

## <a name="xml-editing"></a>Edición de XML

El error más común al configurar directivas personalizadas es el formato incorrecto del código XML. Es fundamental contar con un buen editor XML. Permite mostrar XML nativo y contenido codificado por colores, rellenar previamente los términos comunes, mantener los elementos XML indexados y realizar una validación con un esquema XML.

Dos de nuestros editores favoritos son [Visual Studio Code](https://code.visualstudio.com/) y [Notepad++](https://notepad-plus-plus.org/).

La validación del esquema XML identifica los errores antes de cargar el archivo XML. En la carpeta raíz del [paquete de inicio](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack), obtenga la definición del esquema XML *TrustFrameworkPolicy_0.3.0.0.xsd*. Para saber cómo utilizar el archivo del esquema XSD para la validación en el editor, consulte los temas sobre las *herramientas XML* y la *validación XML*, u otros similares, en la documentación del editor.

Podría resultarle útil revisar las reglas del código XML. Azure AD B2C rechaza todos los errores de formato XML que detecta. En ocasiones, un formato XML incorrecto puede dar lugar a mensajes de error que resultan engañosos.

## <a name="upload-policies-and-policy-validation"></a>Validación de directivas y directivas de carga

La validación del archivo de directiva XML se realiza automáticamente durante la carga. La mayoría de los errores hacen que se produzca un error en la carga. En la validación se incluye el archivo de directiva que se está cargando. Además, se incluye la cadena de archivos a la que hace referencia el archivo de carga (el archivo de directiva de usuario de confianza, el archivo de extensiones y el archivo base).

Entre los errores de validación habituales se incluyen los que se indican a continuación:

> Fragmento de código de error: `...makes a reference to ClaimType with id "displayName" but neither the policy nor any of its base policies contain such an element`

* El valor de ClaimType podría estar mal escrito o no existir en el esquema.
* Se deben definir valores de ClaimType en al menos uno de los archivos de la directiva.
    Por ejemplo: `<ClaimType Id="issuerUserId">`
* Si ClaimType se define en el archivo de extensiones, pero también se usa en el valor de TechnichalProfile en el archivo base, al cargar dicho archivo se producirá un error.

> Fragmento de código de error: `...makes a reference to a ClaimsTransformation with id...`

* Las causas del error podrían ser las mismas que las del error ClaimType.

> Fragmento de código de error: `Reason: User is currently logged as a user of 'yourtenant.onmicrosoft.com' tenant. In order to manage 'yourtenant.onmicrosoft.com', please login as a user of 'yourtenant.onmicrosoft.com' tenant`

* Compruebe que el valor de TenantId en los elementos `<TrustFrameworkPolicy\>` y `<BasePolicy\>` coincide con su inquilino de destino de Azure AD B2C.

## <a name="troubleshoot-the-runtime"></a>Solución de problemas del tiempo de ejecución

* Use **Ejecutar ahora** y `https://jwt.ms` para probar sus directivas con independencia de su aplicación web o móvil. Este sitio web actúa como una aplicación de usuario de confianza. Muestra el contenido del JSON Web Token (JWT) que la directiva de Azure AD B2C genera.

    Para crear una aplicación de prueba que pueda redirigir a `https://jwt.ms` para la inspección de tokens:

    [!INCLUDE [active-directory-b2c-appreg-idp](../../includes/active-directory-b2c-appreg-idp.md)]

* Use [Fiddler](https://www.telerik.com/fiddler) para realizar un seguimiento del intercambio de mensajes entre su explorador cliente y Azure AD B2C. Puede ayudarle a detectar un error en el recorrido del usuario en los pasos de la orquestación.

* Use [Application Insights](troubleshoot-with-application-insights.md) en **modo de desarrollo** para realizar un seguimiento de la actividad de su recorrido del usuario en el marco de experiencia de identidad. En el **modo de desarrollo**, es posible observar el intercambio de notificaciones entre Identity Experience Framework y los diversos proveedores de notificaciones definidos por perfiles técnicos, como proveedores de identidades, servicios de basados en API, el directorio de usuario de Azure AD B2C y otros servicios como Multi-Factor Authentication de Azure AD.

## <a name="recommended-practices"></a>Procedimientos recomendados

**Mantenga varias versiones de los escenarios. Agrúpelas en un proyecto con la aplicación.** Los archivos base, de extensiones y de usuario de confianza dependen directamente los unos de los otros. Guárdelos como un grupo. A medida que se agreguen nuevas características a las directivas, mantenga versiones operativas distintas. Organice las versiones operativas en su propio sistema de archivos con el código de aplicación con el que interactúan. Las aplicaciones pueden invocar muchas directivas de usuario de confianza diferentes en un inquilino. Pueden convertirse en dependientes de las notificaciones que esperan de sus directivas de Azure AD B2C.

**Desarrolle y pruebe perfiles técnicos con recorridos de usuario conocidos.** Use directivas probadas del paquete de inicio para configurar los perfiles técnicos. Pruébelas por separado antes de incorporarlas a sus propios recorridos de usuario.

**Desarrolle y pruebe recorridos de usuario con perfiles técnicos conocidos.** Cambie los pasos de orquestación de un recorrido del usuario de forma incremental. Compile progresivamente los escenarios previstos.

## <a name="next-steps"></a>Pasos siguientes

Descargue el archivo .zip [active-directory-b2c-custom-policy-starterpack](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip), disponible en GitHub. También puede clonar el repositorio:

```
git clone https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
```
