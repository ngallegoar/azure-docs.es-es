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
ms.openlocfilehash: 256da169761da486d8ac064a2f58a59be43bb5df
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92754201"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-workday-mobile-application"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con la aplicación móvil Workday

En este tutorial, aprenderá a integrar Azure Active Directory (Azure AD), el acceso condicional e Intune con la aplicación móvil Workday. Al integrar la aplicación móvil Workday con Microsoft, podrá:

* Asegurarse de que los dispositivos cumplan sus directivas antes de iniciar sesión.
* Agregar controles a la aplicación Workday para asegurarse de que los usuarios accedan de forma segura a los datos corporativos. 
* Controlar en Azure AD quién tiene acceso a Workday.
* Permitir que los usuarios puedan iniciar sesión automáticamente en Workday con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Integración de Workday con Azure AD
* Tutorial: [Integración del inicio de sesión único (SSO) de Azure Active Directory con Workday](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-tutorial)

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, configurará y probará las directivas de acceso condicional de Microsoft e Intune con la aplicación móvil Workday.

* Ahora se puede configurar la aplicación federada de Workday con Azure AD para habilitar el inicio de sesión único. Para obtener más información acerca de la configuración, consulte [este](workday-tutorial.md) vínculo.

> [!NOTE] 
> Workday no es compatible con las directivas de Intune App Protection. Debe usar la administración de dispositivos móviles para utilizar el acceso condicional.


## <a name="ensure-users-have-access-to-the-workday-mobile-app"></a>Asegúrese de que los usuarios tengan acceso a la aplicación móvil Workday:

Configure Workday para permitir el acceso a las ofertas de aplicaciones móviles. Tendrá que configurar las siguientes directivas para móvil:

Puede seguir estas instrucciones para configurarlas:

1. Acceda a las directivas de seguridad del dominio para el informe de área funcional.
2. Seleccione una directiva de seguridad.
    * Uso móvil: Android
    * Uso móvil: iPad
    * Uso móvil: iPhone
3. Haga clic en Editar permisos.
4. Active la casilla View (Ver) o Modify (Modificar) para conceder a los grupos de seguridad acceso a los elementos protegibles de informes o tareas.
5. Active la casilla Get (Obtener) o Put (Poner) para conceder a los grupos de seguridad acceso a las acciones protegibles de informes o tareas y a la integración.

Ejecute la tarea **Activate Pending Security Policy Changes** (Activar cambios en la directiva de seguridad pendientes) para activar los cambios en la directiva de seguridad pendientes.

## <a name="open-workday-login-page-in-mobile-browser"></a>Abra la página de inicio de sesión de Workday en el explorador móvil:

Para aplicar el acceso condicional a la aplicación móvil Workday, es necesario que la aplicación se abra en un explorador externo. Para ello, active la casilla **Enable Mobile Browser SSO for Native Apps** (Habilitar SSO de explorador móvil para aplicaciones nativas) en **Edit Tenant Setup - Security** (Editar configuración de inquilino – Seguridad). Esto requerirá que haya instalado un explorador aprobado por Intune en el dispositivo para iOS y en el perfil de trabajo para Android.

![Inicio de sesión en el explorador móvil](./media/workday-tutorial/mobile-browser.png)

## <a name="setup-conditional-access-policy"></a>Configuración de la directiva de acceso condicional:

Esta directiva solo afectará al inicio de sesión en un dispositivo iOS o Android. Si desea extenderla a todas las plataformas, simplemente seleccione **Cualquier dispositivo**. Esta directiva requerirá que el dispositivo cumpla la directiva y lo comprobará mediante Microsoft Intune. Debido a que Android tiene perfiles de trabajo, esto debe impedir que los usuarios inicien sesión en Workday (web o aplicación) a menos que lo hagan a través de su perfil de trabajo y hayan instalado la aplicación mediante el Portal de empresa de Intune. Para iOS, se necesita un paso adicional para garantizar que ocurra lo mismo. A continuación, se muestran algunas capturas de pantallas de la configuración del acceso condicional.

**Workday admite los siguientes controles de acceso:**
* Requerir autenticación multifactor
* Requerir que el dispositivo esté marcado como compatible

**La aplicación Workday no admite lo siguiente:**
* Requerir aplicación cliente aprobada
* Requerir una directiva de protección de aplicaciones (versión preliminar)

Para configurar **Workday** como **dispositivo administrado** , realice los pasos siguientes:

![Configuración de la directiva de acceso condicional](./media/workday-tutorial/managed-devices-only.png)

1. Haga clic en **Home > Microsoft Intune > conditional Access-Policies > Managed Devices Only** (Inicio > Microsoft Intune > Directivas de acceso condicional > Solo dispositivos administrados). 

1. En la página **Managed Devices Only** (Solo dispositivos administrados), asigne al campo **Nombre** el valor `Managed Devices Only` y haga clic en **Aplicaciones en la nube o acciones**.

1. En **Aplicaciones en la nube o acciones** , siga estos pasos.

    a. Cambie **Seleccione a qué se aplica esta directiva** a **Aplicaciones en la nube**.

    b. En Incluir, haga clic en **Seleccionar aplicaciones**.

    c. Elija **Workday** en la lista de selección.

    d. Haga clic en **Done** (Listo).

1. Active **Habilitar directiva**.

1. Haga clic en **Save** (Guardar).

Para **conceder** acceso, siga estos pasos:

![Configuración de la directiva de acceso condicional para Workday](./media/workday-tutorial/managed-devices-only-2.png)

1. Haga clic en **Home > Microsoft Intune > conditional Access-Policies > Managed Devices Only** (Inicio > Microsoft Intune > Directivas de acceso condicional > Solo dispositivos administrados). 

1. En la página **Managed Devices Only** (Solo dispositivos administrados), asigne al campo **Nombre** el valor `Managed Devices Only` y haga clic en **Controles de acceso > Conceder**.

1. En la página **Conceder** , siga estos pasos.

    a. Seleccione los controles que se aplicarán como **Conceder acceso**.

    b. Active la casilla **Requerir que el dispositivo esté marcado como compatible**.

    c. Seleccione **Requerir uno de los controles seleccionados**.

    d. Haga clic en **Seleccionar**.

1. Active **Habilitar directiva**.

1. Haga clic en **Guardar**

## <a name="set-up-device-compliance-policy"></a>Configuración de la directiva de cumplimiento de dispositivos:

Para asegurarse de que los dispositivos iOS solo pueden iniciar sesión mediante una aplicación Workday administrada de MDM, debe bloquear la aplicación App Store agregando **com.workday.workdayapp** a la lista de aplicaciones restringidas. Esto garantizará que solo los dispositivos que tengan la aplicación Workday instalada mediante el portal de empresa puedan acceder a Workday. Si se utiliza un explorador, los dispositivos solo podrán acceder a Workday si el dispositivo está administrado por Intune y se usa un explorador administrado.

![Configuración de la directiva de cumplimiento de dispositivos para Workday](./media/workday-tutorial/ios-policy.png)

## <a name="set-up-microsoft-intune-app-configuration-policies"></a>Configuración de las directivas de configuración de aplicaciones de Microsoft Intune:

| Escenario | Pares clave-valor |
|----------------------------------------------------------------------------------------   |-----------|
| Rellene automáticamente los campos Inquilino y Dirección web para:<br>• Workday en Android al habilitar perfiles de trabajo en Android.<br>• Workday en iPad y iPhone.     | Use estos valores para configurar el inquilino: <br>● Clave de configuración = UserGroupCode<br>●   Tipo de valor = String <br>●   Valor de configuración = Nombre del inquilino Ejemplo: gms<br>Use estos valores para configurar la dirección web:<br>●    Clave de configuración = AppServiceHost<br>● Tipo de valor = String<br>●    Valor de configuración = Dirección URL base para el inquilino Ejemplo: https://www.myworkday.com                              |   |
| Deshabilite estas acciones para Workday en iPad y iPhone:<br>●    Cortar, copiar y pegar<br>●   Imprimir                       | Establezca el valor (booleano) en false en estas claves para deshabilitar la funcionalidad:<br>● AllowCutCopyPaste<br>●  AllowPrint  |
| Deshabilite las capturas de pantalla para Workday en Android. |Establezca el valor (booleano) en false en la clave AllowScreenshots para deshabilitar la funcionalidad.|
| Deshabilite las actualizaciones sugeridas para los usuarios.|Establezca el valor (booleano) en false en la clave AllowSuggestedUpdates para deshabilitar la funcionalidad.|
|Personalice la dirección URL de la tienda de aplicaciones para dirigir a los usuarios móviles a la tienda de aplicaciones de su elección.|Use estos valores para cambiar la dirección URL de la tienda de aplicaciones:<br>● Clave de configuración = AppUpdateURL<br>●   Tipo de valor = String<br> ●   Valor de configuración = Dirección URL de la tienda de aplicaciones|
|       |


## <a name="ios-configuration-policies"></a>Directivas de configuración de iOS:

1. Vaya a https://portal.azure.com/ e inicie sesión.
2. Busque **Intune** o haga clic en el widget de la lista.
3. Vaya a **Aplicaciones clientes -> Aplicaciones -> Directivas de configuración de la aplicación -> + Agregar -> Dispositivos administrados**.
4. Escriba un nombre.
5. En **Plataforma** , seleccione **iOS/iPadOS**.
6. En **Associated App** (Aplicación asociada), elija la aplicación Workday para iOS que ha agregado.
7. Haga clic en **Configuration Settings** (Opciones de configuración) en **Configuration settings format** (Formato de opciones de configuración) y seleccione **Enter XML Data** (Escribir datos XML).
8. A continuación, se incluye un archivo XML de ejemplo. Agregue las configuraciones que desee aplicar. Reemplace **STRING_VALUE** por la cadena que desee usar. Reemplace `<true />` o `<false />` por `<true />` o `<false />`. Si no agrega una configuración, funcionará como si se hubiera establecido en true.

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
9. Haga clic en Agregar
10. Actualice la página y haga clic en la directiva recién creada.
11. Haga clic en Tareas y elija a quién desea que se aplique la aplicación.
12. Haga clic en Guardar.

## <a name="android-configuration-policies"></a>Directiva de configuración de Android:

1. Vaya a `https://portal.azure.com/` e inicie sesión.
2. Busque **Intune** o haga clic en el widget de la lista.
3. Vaya a **Aplicaciones clientes -> Aplicaciones -> Directivas de configuración de la aplicación -> + Agregar -> Dispositivos administrados**.
5. Escriba un nombre. 
6. En **Plataforma** , seleccione **Android**.
7. En **Associated App** (Aplicación asociada), elija la aplicación Workday para Android que ha agregado.
8. Haga clic en **Configuration Settings** (Opciones de configuración) en **Configuration settings format** (Formato de opciones de configuración) y seleccione **Enter JSON Data** (Escribir datos JSON).

