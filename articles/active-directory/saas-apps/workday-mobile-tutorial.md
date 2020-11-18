---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con la aplicación móvil Workday | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y la aplicación móvil Workday.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/31/2020
ms.author: jeedes
ms.openlocfilehash: e706649957bf427cd577d7995fb9ce104c687f4b
ms.sourcegitcommit: 0d171fe7fc0893dcc5f6202e73038a91be58da03
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2020
ms.locfileid: "93378999"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-workday-mobile-application"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con la aplicación móvil Workday

En este tutorial aprenderá a integrar Azure Active Directory (Azure AD), el acceso condicional e Intune con la aplicación móvil Workday. Al integrar la aplicación móvil Workday con Microsoft, podrá:

* Asegurarse de que los dispositivos cumplan sus directivas antes de iniciar sesión.
* Agregar controles a la aplicación móvil Workday para asegurarse de que los usuarios accedan de forma segura a los datos corporativos. 
* Controlar en Azure AD quién tiene acceso a Workday.
* Permitir que los usuarios inicien sesión automáticamente en Workday con sus cuentas de Azure AD.
* Administrar sus cuentas en una ubicación central: Azure Portal.

## <a name="prerequisites"></a>Requisitos previos

Primeros pasos:

* Integre Workday con Azure AD.
* Lea [Integración del inicio de sesión único (SSO) de Azure Active Directory con Workday](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-tutorial).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial configurará y probará las directivas de acceso condicional de Azure AD e Intune con la aplicación móvil Workday.

Para habilitar el inicio de sesión único (SSO), puede configurar la aplicación federada Workday con Azure AD. Para más información, consulte [Integración del inicio de sesión único (SSO) de Azure Active Directory con Workday](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-tutorial).

> [!NOTE] 
> Workday no es compatible con las directivas de protección de la aplicación de Intune. Debe usar la administración de dispositivos móviles para utilizar el acceso condicional.


## <a name="ensure-users-have-access-to-workday-mobile-application"></a>Garantía de que los usuarios tienen acceso a la aplicación móvil Workday

Configure Workday para permitir el acceso a las aplicaciones móviles. Tendrá que configurar las siguientes directivas para la aplicación móvil Workday:

1. Acceda a las directivas de seguridad del dominio para el informe de área funcional.
1. Seleccione la directiva de seguridad adecuada:
    * Uso móvil: Android
    * Uso móvil: iPad
    * Uso móvil: iPhone
1. Seleccione **Editar permisos**.
1. Active la casilla **View or Modify** (Ver o Modificar) para conceder a los grupos de seguridad acceso a los elementos protegibles de informes o tareas.
1. Active la casilla **Get or Put** (Obtener o Poner) para conceder a los grupos de seguridad acceso a las acciones protegibles de informes o tareas y a la integración.

Ejecute **Activate Pending Security Policy Changes** (Activar cambios en la directiva de seguridad pendientes) para activar los cambios en la directiva de seguridad pendientes.

## <a name="open-workday-sign-in-page-in-workday-mobile-browser"></a>Apertura de la página de inicio de sesión de Workday en el explorador móvil de Workday

Para aplicar el acceso condicional a la aplicación móvil Workday, debe abrir la aplicación en un explorador externo. En **Edit Tenant Setup - Security** (Editar configuración de inquilino – Seguridad), seleccione **Enable Mobile Browser SSO for Native Apps** (Habilitar SSO de explorador móvil para aplicaciones nativas). Esto requiere que haya instalado un explorador aprobado por Intune en el dispositivo para iOS y en el perfil de trabajo para Android.

![Captura de pantalla del inicio de sesión del explorador móvil de Workday.](./media/workday-tutorial/mobile-browser.png)

## <a name="set-up-conditional-access-policy"></a>Configuración de la directiva de acceso condicional

Esta directiva solo afecta al inicio de sesión en un dispositivo iOS o Android. Si desea extenderla a todas las plataformas, seleccione **Cualquier dispositivo**. Esta directiva requiere que el dispositivo cumpla la directiva y lo comprueba mediante Intune. Como Android tiene perfiles de trabajo, impide que los usuarios inicien sesión en WorkDay, a menos que lo hagan mediante su perfil de trabajo y hayan instalado la aplicación mediante el portal de empresa de Intune. Para iOS, se necesita un paso adicional para garantizar que ocurra lo mismo.

Workday admite los siguientes controles de acceso:
* Requiere autenticación multifactor
* Requerir que el dispositivo esté marcado como compatible

La aplicación Workday no admite lo siguiente:
* Requerir aplicación cliente aprobada
* Requerir una directiva de protección de aplicaciones (versión preliminar)

Para configurar Workday como dispositivo administrado, realice los pasos siguientes:

![Captura de pantalla de Managed Devices Only (Solo dispositivos administrados) y Cloud apps or actions (Aplicaciones o acciones en la nube).](./media/workday-tutorial/managed-devices-only.png)

1. Seleccione **Inicio** > **Microsoft Intune** > **Conditional Access-Policies** (Directiva de acceso condicional). A continuación, seleccione **Managed Devices Only** (Solo dispositivos administrados). 

1. En **Managed Devices Only** (Solo dispositivos administrados), en **Name** (Nombre), seleccione **Managed Devices Only** (Solo dispositivos administrados) y **Cloud apps or actions** (Aplicaciones o acciones en la nube).

1. En **Cloud apps or actions** (Aplicaciones o acciones en la nube):

    a. Cambie **Seleccione a qué se aplica esta directiva** a **Aplicaciones en la nube**.

    b. En **Incluir**, elija **Seleccionar aplicaciones**.

    c. En la lista **Seleccionar**, elija **Workday**.

    d. Seleccione **Listo**.

1. Establezca **Habilitar directiva** en **Activado**.

1. Seleccione **Guardar**.

Para **conceder** acceso, siga estos pasos:

![Captura de pantalla de Managed Devices Only (Solo dispositivos administrados) y Grant (Concesión).](./media/workday-tutorial/managed-devices-only-2.png)

1. Seleccione **Inicio** > **Microsoft Intune** > **Conditional Access-Policies** (Directiva de acceso condicional). A continuación, seleccione **Managed Devices Only** (Solo dispositivos administrados). 

1. En **Managed Devices Only** (Solo dispositivos administrados), en **Name** (Nombre), selecciones **Managed Devices Only** (Solo dispositivos administrados). En **Controles de acceso**, seleccione **Conceder**.

1. En **Grant** (Concesión):

    a. Seleccione los controles que se aplicarán como **Conceder acceso**.

    b. Seleccione **Requerir que el dispositivo esté marcado como compatible**.

    c. Seleccione **Requerir uno de los controles seleccionados**.

    d. Elija **Seleccionar**.

1. Establezca **Habilitar directiva** en **Activado**.

1. Seleccione **Guardar**.

## <a name="set-up-device-compliance-policy"></a>Configuración de la directiva de cumplimiento de dispositivos

Para asegurarse de que los dispositivos iOS solo pueden iniciar sesión mediante una aplicación Workday administrada de la administración de dispositivos móviles, agregue **com.workday.workdayapp** a la lista de aplicaciones restringidas para bloquear la aplicación App Store. Esto garantiza que solo los dispositivos que tengan la aplicación Workday instalada mediante el portal de empresa puedan acceder a ella. Si se utiliza un explorador, los dispositivos solo podrán acceder a Workday si están administrados por Intune y se usa un explorador administrado.

![Captura de pantalla de la directiva de cumplimiento para dispositivos iOS.](./media/workday-tutorial/ios-policy.png)

## <a name="set-up-intune-app-configuration-policies"></a>Configuración de las directivas de configuración de aplicaciones de Intune

| Escenario | Pares clave-valor |
|----------------------------------------------------------------------------------------   |-----------|
| Rellene automáticamente los campos Inquilino y Dirección web para:<br>• Workday en Android al habilitar perfiles de Android for work.<br>• Workday en iPad y iPhone.     | Use estos valores para configurar el inquilino: <br>● Clave de configuración = `UserGroupCode`<br>● Tipo de valor = String <br>●   Valor de configuración = Nombre del inquilino Ejemplo: `gms`<br>Use estos valores para configurar la dirección web:<br>●  Clave de configuración = `AppServiceHost`<br>●   Tipo de valor = String<br>●    Valor de configuración = Dirección URL base para el inquilino Ejemplo: `https://www.myworkday.com`                                |   |
| Deshabilite estas acciones para Workday en iPad y iPhone:<br>●    Cortar, copiar y pegar<br>●   Imprimir                       | Establezca el valor (booleano) en `False` en estas claves para deshabilitar la funcionalidad:<br>●   `AllowCutCopyPaste`<br>●    `AllowPrint`    |
| Deshabilite las capturas de pantalla para Workday en Android. |Establezca el valor (booleano) en `False` en la clave `AllowScreenshots` para deshabilitar la funcionalidad.|
| Deshabilite las actualizaciones sugeridas para los usuarios.|Establezca el valor (booleano) en `False` en la clave `AllowSuggestedUpdates` para deshabilitar la funcionalidad.|
|Personalice la dirección URL de la tienda de aplicaciones para dirigir a los usuarios móviles a la tienda de aplicaciones de su elección.|Use estos valores para cambiar la dirección URL de la tienda de aplicaciones:<br>● Clave de configuración = `AppUpdateURL`<br>● Tipo de valor = String<br> ●   Valor de configuración = Dirección URL de la tienda de aplicaciones|
|       |


## <a name="ios-configuration-policies"></a>Directivas de configuración de iOS

1. Vaya a [Azure Portal](https://portal.azure.com/) e inicie sesión.
1. Busque **Intune** o seleccione el widget de la lista.
1. Vaya a **Aplicaciones cliente** > **Aplicaciones** > **Directivas de configuración de la aplicación**. A continuación, seleccione **+ Agregar** > **Managed Devices** (Dispositivos administrados).
1. Escriba un nombre.
1. En **Plataforma**, elija **iOS/iPadOS**.
1. En **Associated App** (Aplicación asociada), elija la aplicación Workday para iOS que ha agregado.
1. Seleccione **Opciones de configuración**. En **Configuration settings format** (Formato de opciones de configuración), seleccione **Enter XML Data** (Escribir datos XML).
1. A continuación se incluye un archivo XML de ejemplo. Agregue las configuraciones que desee aplicar. Reemplace `STRING_VALUE` por el nombre que desea usar. Reemplace `<true /> or <false />` por `<true />` o `<false />`. Si no agrega una configuración, este ejemplo funciona como si estuviera establecida en `True`.

    ```
    <dict>
    <key>UserGroupCode</key>
    <string>STRING_VALUE</string>
    <key>AppServiceHost</key>
    <string>STRING_VALUE</string>
    <key>AllowCutCopyPaste</key>
    <true /> or <false />
    <key>AllowPrint</key>
    <true /> or <false />
    <key>AllowSuggestedUpdates</key>
    <true /> or <false />
    <key>AppUpdateURL</key>
    <string>STRING_VALUE</string>
    </dict>

    ```
1. Seleccione **Agregar**.
1. Actualice la página y seleccione la directiva recién creada.
1. Seleccione **Tareas** y elija a quién desea que se aplique la aplicación.
1. Seleccione **Guardar**.

## <a name="android-configuration-policies"></a>Directivas de configuración de Android

1. Vaya a [Azure Portal](https://portal.azure.com/) e inicie sesión.
2. Busque **Intune** o seleccione el widget de la lista.
3. Vaya a **Aplicaciones cliente** > **Aplicaciones** > **Directivas de configuración de la aplicación**. A continuación, seleccione **+ Agregar** > **Managed Devices** (Dispositivos administrados).
5. Escriba un nombre. 
6. En **Plataforma**, seleccione **Android**.
7. En **Associated App** (Aplicación asociada), elija la aplicación Workday para Android que ha agregado.
8. Seleccione **Opciones de configuración**. En **Formato de opciones de configuración**, seleccione **Enter JSON Data** (Escribir datos JSON).

