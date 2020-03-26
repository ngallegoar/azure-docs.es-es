---
title: MFA basado en riesgos y SSPR sin Azure Identity Protection
description: En este tutorial, habilitará las integraciones de Azure Identity Protection para Multi-Factor Authenticacion y el autoservicio de restablecimiento de contraseñas a fin de aminorar los comportamientos de riesgo.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: tutorial
ms.date: 01/31/2018
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: e1a6858d5eda8227b3f7c1b90dee86f44273a258
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "74846358"
---
# <a name="tutorial-use-risk-detections-to-trigger-multi-factor-authentication-and-password-changes"></a>Tutorial: Uso de detecciones de riesgo para desencadenar Multi-Factor Authentication y cambios de contraseñas

En este tutorial, habilitará las funciones de Azure Active Directory (Azure AD) Identity Protection, una característica de Azure AD Premium P2 que es más que una herramienta de supervisión e informe. Para proteger las identidades de una organización, puede configurar directivas en función del que respondan automáticamente a comportamientos de riesgo. Estas directivas pueden bloquear o iniciar la corrección de forma automática, por ejemplo, requiriendo cambios en las contraseñas y aplicando Multi-Factor Authentication.

Las directivas de Azure AD Identity Protection pueden usarse además de las directivas de acceso condicional existentes como un nivel adicional de protección. Puede que los usuarios nunca pongan en práctica un comportamiento arriesgado que requiera una de estas directivas, pero como administrador sabe que están protegidos.

Algunos elementos que podrían desencadenar una detección de riesgo son:

* Usuarios con credenciales perdidas
* Inicios de sesión desde direcciones IP anónimas
* Viaje imposible a ubicaciones inusuales
* Inicios de sesión desde dispositivos infectados
* Inicios de sesión desde direcciones IP con actividad sospechosa
* Inicios de sesión desde ubicaciones desconocidas

Puede encontrar más información acerca de Azure AD Identity Protection en el artículo [¿Qué es Azure AD Identity Protection?](../active-directory-identityprotection.md)

> [!div class="checklist"]
> * Habilitación del registro de Azure MFA
> * Habilitación de los cambios de contraseña en función del riesgo
> * Habilitación de la autenticación multifactor en función del riesgo

## <a name="prerequisites"></a>Prerrequisitos

* Acceso a un inquilino de Azure AD activo con al menos una licencia de prueba asignada de Azure AD Premium P2.
* Una cuenta con privilegios de administrador global en su inquilino de Azure AD.
* Completar los tutoriales anteriores sobre el autoservicio de restablecimiento de contraseñas (SSPR) y Multi-Factor Authentication (MFA).

## <a name="enable-risk-based-policies-for-sspr-and-mfa"></a>Habilitación de las directivas basadas en riesgos para SSPR y MFA

Habilitar directivas en función del riesgo es un proceso sencillo. Los pasos siguientes le guiarán por un ejemplo de configuración.

### <a name="enable-users-to-register-for-multi-factor-authentication"></a>Habilitación de los usuarios para registrarse para la autenticación multifactor.

Azure AD Identity Protection incluye una directiva predeterminada que puede ayudarle a que los usuarios registrados usen la autenticación multifactor y a identificar fácilmente el estado de registro actual. Si habilita esta directiva, no se empieza a requerir que los usuarios lleven a cabo la autenticación multifactor, pero se les pedirá que se registren previamente.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Haga clic en **Todos los servicios** y, a continuación, vaya a **Azure AD Identity Protection**.
1. Haga clic en **Registro de MFA**.
1. Establezca Aplicar directiva en **Activado**.
   1. Al establecer esta directiva, requerirá que todos los usuarios registren los métodos para prepararse para usar la autenticación multifactor.
1. Haga clic en **Save**(Guardar).

   ![Requiera a los usuarios que se registren en MFA al iniciar sesión](./media/tutorial-risk-based-sspr-mfa/risk-based-require-mfa-registration.png)

### <a name="enable-risk-based-password-changes"></a>Habilitación de los cambios de contraseña en función del riesgo

Microsoft trabaja con los investigadores, las autoridades judiciales, varios equipos de seguridad de Microsoft y otros orígenes de confianza para buscar pares de nombre de usuario y contraseña. Cuando uno de estos pares encaja con una cuenta de su entorno, se puede desencadenar un cambio de contraseña en función del riesgo mediante la siguiente directiva.

1. Haga clic en Directiva de riesgo de usuario.
1. En **Condiciones**, seleccione **Riesgo de usuario** y, a continuación, elija **Medio y superior**.
1. Haga clic en "Seleccionar" y en "Listo".
1. En **Acceso**, elija **Permitir el acceso** y, a continuación, seleccione **Requerir cambio de contraseña**.
1. Haga clic en "Seleccionar"
1. Establezca Aplicar directiva en **Activado**.
1. Haga clic en **Guardar**

### <a name="enable-risk-based-multi-factor-authentication"></a>Habilitación de la autenticación multifactor en función del riesgo

La mayoría de los usuarios tienen un comportamiento normal que puede seguirse, cuando se salen de esta norma, podría ser peligroso permitirles que simplemente inicien sesión. Es posible que sea conveniente bloquear a ese usuario o quizás simplemente puede pedirle que lleve a cabo la autenticación multifactor para demostrar que realmente es quien dice ser. Para habilitar una directiva que requiera MFA cuando se detecta un inicio de sesión peligroso, habilite la directiva siguiente.

1. Haga clic en Directiva de riesgo de inicio de sesión.
1. En **Condiciones**, seleccione **Riesgo de usuario** y, a continuación, elija **Medio y superior**.
1. Haga clic en "Seleccionar" y en "Listo".
1. En **Acceso**, elija **Permitir el acceso** y, a continuación, seleccione **Requerir autenticación multifactor**.
1. Haga clic en "Seleccionar"
1. Establezca Aplicar directiva en **Activado**.
1. Haga clic en **Guardar**

## <a name="clean-up-resources"></a>Limpieza de recursos

Si ha completado las pruebas y ya no desea tener habilitadas las directivas en función del riesgo, vuelva a cada directiva que desee deshabilitar y establezca **Aplicar directiva** en **Desactivada**.
