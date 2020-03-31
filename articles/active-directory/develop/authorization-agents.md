---
title: Agentes de autorización y cómo habilitarlos | Azure
description: Información sobre los diferentes agentes de autorización que la biblioteca de autenticación de Microsoft (MSAL) permite que la aplicación de Android utilice y cómo habilitarlos.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/05/2019
ms.author: marsma
ms.reviewer: shoatman, brianmel, hahamil
ms.custom: aaddev
ms.openlocfilehash: 4f1b3fc5b60069cfa47d437e4341ded141204418
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77085336"
---
# <a name="authorization-agents-android"></a>Agentes de autorización (Android)

En este artículo se describen los diferentes agentes de autorización que la biblioteca de autenticación de Microsoft (MSAL) permite que la aplicación utilice y cómo habilitarlos.

La elección de una estrategia específica para los agentes de autorización es opcional y representa una funcionalidad adicional que las aplicaciones pueden personalizar. La mayoría de las aplicaciones usarán los valores predeterminados de MSAL (consulte [Información acerca del archivo de configuración de MSAL para Android](msal-configuration.md) para ver los distintos valores predeterminados).

MSAL admite la autorización mediante `WebView` o el explorador del sistema.  En la imagen siguiente se muestra cómo se utiliza `WebView` o el explorador del sistema con CustomTabs o sin CustomTabs:

![Ejemplos de inicio de sesión de MSAL](./media/authorization-agents/sign-in-ui.jpg)

## <a name="single-sign-in-implications"></a>Implicaciones del inicio de sesión único

De forma predeterminada, las aplicaciones integradas con MSAL utilizan Custom Tabs del explorador del sistema para la autorización. A diferencia de WebWiews, Custom Tabs comparte un archivo jar de cookies con el explorador del sistema predeterminado, lo que permite menos inicios de sesión con aplicaciones web u otras aplicaciones nativas integradas con Custom Tabs.

Si la aplicación usa una estrategia de `WebView` sin integrar la compatibilidad con Microsoft Authenticator o el Portal de empresa en su aplicación, los usuarios no tendrán una experiencia de inicio de sesión único (SSO) en el dispositivo o entre aplicaciones nativas y aplicaciones web.

Si la aplicación usa MSAL con la compatibilidad de Microsoft Authenticator o del Portal de empresa, los usuarios pueden tener una experiencia de inicio de sesión único entre aplicaciones si el usuario tiene un inicio de sesión activo con una de las aplicaciones.

## <a name="webview"></a>WebView

Para usar WebView de la aplicación, ponga la siguiente línea en el archivo JSON de configuración de la aplicación que se pasa a MSAL:

```json
"authorization_user_agent" : "WEBVIEW"
```

Al utilizar `WebView` de la aplicación, el usuario inicia sesión directamente en la aplicación. Los tokens se mantienen en el espacio aislado de la aplicación y no están disponibles fuera del archivo jar de cookies de la aplicación. En consecuencia, el usuario no puede tener una experiencia de inicio de sesión único entre aplicaciones a menos que las aplicaciones se integren con Authenticator o el Portal de empresa.

Sin embargo, `WebView` proporciona la funcionalidad para personalizar la apariencia de la interfaz de usuario de inicio de sesión. Para más información sobre cómo realizar esta personalización, consulte [Android WebViews](https://developer.android.com/reference/android/webkit/WebView) (WebViews en Android).

## <a name="default-browser-plus-custom-tabs"></a>Explorador predeterminado con Custom Tabs

De forma predeterminada, MSAL usa el explorador y una estrategia de [Custom Tabs](https://developer.chrome.com/multidevice/android/customtabs). Puede indicar explícitamente esta estrategia para evitar cambios en versiones futuras en `DEFAULT` mediante la siguiente configuración de JSON en el archivo de configuración personalizado:

```json
"authorization_user_agent" : "BROWSER"
```

Utilice este enfoque para proporcionar una experiencia de inicio de sesión único mediante el explorador del dispositivo. MSAL usa un archivo jar de cookies compartido, que permite que otras aplicaciones nativas o aplicaciones web ejecuten el inicio de sesión único en el dispositivo mediante la cookie de sesión persistente establecida por MSAL.

## <a name="browser-selection-heuristic"></a>Heurística de selección de explorador

Como es imposible que MSAL especifique el paquete exacto de explorador que se va a usar en cada amplia gama de teléfonos Android, MSAL implementa una heurística de selección de explorador que intenta proporcionar el mejor inicio de sesión único entre dispositivos.

MSAL recupera la lista completa de los exploradores instalados en el dispositivo para seleccionar qué explorador se va a utilizar. La lista se encuentra en el orden devuelto por el administrador de paquetes, que refleja indirectamente las preferencias del usuario. Por ejemplo, el explorador predeterminado, si se establece, es la primera entrada de la lista. Se elegirá el _primer_ explorador de la lista independientemente de si admite Custom Tabs. Si el explorador admite Custom Tabs, MSAL iniciará Custom Tab. Custom Tabs tiene una apariencia similar a la de una aplicación `WebView` y permite una personalización de la interfaz de usuario básica. Consulte [Custom Tabs in Android](https://developer.chrome.com/multidevice/android/customtabs) (Custom Tabs en Android) para más información.

Si no hay paquetes de explorador en el dispositivo, MSAL usa `WebView` de la aplicación.

El sistema operativo determina el orden de los exploradores en la lista de exploradores. Está ordenada del explorador más preferido al menos. Si no se cambia la configuración predeterminada del dispositivo, se debe iniciar el mismo explorador para cada inicio de sesión con el fin de garantizar una experiencia de SSO.

> [!NOTE]
> MSAL ya no siempre prefiere Chrome si hay otro explorador establecido como predeterminado. Por ejemplo, en un dispositivo que tenga Chrome y otro explorador preinstalado, MSAL usará el explorador que el usuario haya establecido como predeterminado.

### <a name="tested-browsers"></a>Exploradores probados

Se han probado los siguientes exploradores para ver si se redirigen correctamente al `"redirect_uri"` especificado en el archivo de configuración:

| | Explorador integrado | Chrome | Opera  | Microsoft Edge | Explorador UC | Firefox |
| -- |:-------------:| -----:|-----:|-----:|-----:|-----:|
| Nexus 4 (API 17) | superada | superada |no aplicable |no aplicable |no aplicable |no aplicable |
| Samsung S7 (API 25) | superada* | superada | superada | superada | no superada |superada |
| Huawei (API 26) |superada** | superada | no superada | superada | superada |superada |
| Vivo (API 26) |superada|superada|superada|superada|superada|no superada|
| Pixel 2 (API 26) |superada | superada | superada | superada | no superada |superada |
| Oppo | superada | no aplicable** |no aplicable  |no aplicable |no aplicable | no aplicable|
| OnePlus (API 25) |superada | superada | superada | superada | no superada |superada |
| Nexus (API 28) |superada | superada | superada | superada | no superada |superada |
|MI | superada | superada | superada | superada | no superada |superada |

*El explorador integrado de Samsung es Samsung Internet.  
**El explorador integrado de Huawei es Huawei Browser.  
***El explorador predeterminado no se puede cambiar en la configuración del dispositivo Oppo.
