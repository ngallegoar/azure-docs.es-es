---
title: Reactivación de los espacios de nombres deshabilitados de Azure Access Control Service (ACS)
description: Busque y habilite los espacios de nombres de Azure Access Control Service (ACS) y solicite una extensión para mantenerlos habilitados hasta el 4 de febrero de 2019.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.workload: identity
ms.topic: how-to
ms.date: 01/21/2019
ms.author: ryanwi
ms.reviewer: jlu
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: 7bb572e9e22519491290e54417f5ca350d6c0b5c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85383724"
---
# <a name="how-to-reactivate-disabled-access-control-service-namespaces"></a>Procedimientos: Reactivación de los espacios de nombres deshabilitados de Access Control Service

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

En noviembre de 2017, anunciamos que Microsoft Azure Access Control Service (ACS), un servicio de Azure Active Directory (Azure AD), se va a retirar el 7 de noviembre de 2018.

Desde entonces, hemos enviado varios mensajes al correo electrónico de administración de las suscripciones de ACS sobre la retirada de ACS 12 meses, 9 meses, 6 meses, 3 meses, 1 mes, 2 semanas, 1 semana y 1 día antes de la fecha de retirada del 7 de noviembre de 2018.

El 3 de octubre de 2018 anunciamos (mediante un correo electrónico y [una entrada de blog](https://azure.microsoft.com/blog/one-month-retirement-notice-access-control-service/)) una oferta de extensión para los clientes que no puedan terminar su migración antes del 7 de noviembre de 2018. El anuncio también contenía instrucciones para solicitar una extensión.

## <a name="why-your-namespace-is-disabled"></a>¿Por qué se deshabilita el espacio de nombres?

Si no ha optado por recibir una extensión, empezaremos a deshabilitar los espacios de nombres de ACS a partir del 7 de noviembre de 2018. Ya debería haber solicitado la extensión al 4 de febrero de 2019. En caso contrario, no podrá habilitar los espacios de nombres mediante PowerShell.

> [!NOTE]
> Debe ser un administrador de servicio o coadministrador de la suscripción para poder ejecutar los comandos de PowerShell y solicitar una extensión.

## <a name="find-and-enable-your-acs-namespaces"></a>Búsqueda y habilitación de los espacios de nombres de ACS

Puede utilizar PowerShell de ACS para enumerar todos los espacios de nombre de ACS y reactivar los que se han desactivado.

1. Descargue e instale PowerShell de ACS:
    1. Vaya a la Galería de PowerShell y descargue [Acs.Namespaces](https://www.powershellgallery.com/packages/Acs.Namespaces/1.0.2).
    1. Instale el módulo:

        ```powershell
        Install-Module -Name Acs.Namespaces
        ```

    1. Obtenga una lista de todos los comandos posibles:

        ```powershell
        Get-Command -Module Acs.Namespaces
        ```

        Para obtener ayuda sobre un comando específico, ejecute:

        ```powershell
        Get-Help [Command-Name] -Full
        ```
    
        donde `[Command-Name]` es el nombre del comando de ACS.
1. Conéctese a ACS con el cmdlet **Connect AcsAccount**. 

    Es posible que necesite cambiar la directiva de ejecución con **Set-ExecutionPolicy** para que pueda ejecutar el comando.
1. Enumere las suscripciones de Azure disponibles con el cmdlet **Get AcsSubscription**.
1. Enumere los espacios de nombres de ACS con el cmdlet **Get AcsNamespace**.
1. Confirme que los espacios de nombres están deshabilitados mediante la confirmación de que `State` es `Disabled`.

    [![Confirmación de que los espacios de nombres están deshabilitados](./media/howto-reactivate-disabled-acs-namespaces/confirm-disabled-namespace.png)](./media/howto-reactivate-disabled-acs-namespaces/confirm-disabled-namespace.png#lightbox)

    También puede usar `nslookup {your-namespace}.accesscontrol.windows.net` para confirmar si el dominio está todavía activo.

1. Habilite los espacios de nombres de ACS con el cmdlet **Enable-AcsNamespace**.

    Una vez que haya habilitado los espacios de nombres, puede solicitar una extensión para que los espacios de nombres no vuelvan a deshabilitarse antes del 4 de febrero de 2019. Después de esa fecha, se producirá un error en todas las solicitudes a ACS.

## <a name="request-an-extension"></a>Solicitud de una extensión

Vamos a recibir nuevas solicitudes de extensión a partir del 21 de enero de 2019.

Se iniciará la deshabilitación de los espacios de nombres para los clientes que hayan solicitado extensiones al 4 de febrero de 2019. Todavía puede volver a habilitar los espacios de nombres mediante PowerShell, pero, de nuevo, se deshabilitarán los espacios de nombres después de 48 horas.

Después del 4 de marzo de 2019, los clientes ya no podrán volver a habilitar los espacios de nombres mediante PowerShell.

Ya no se aprobarán automáticamente más extensiones. Si necesita tiempo adicional para migrar, póngase en contacto con [Soporte técnico de Azure](https://portal.azure.com/#create/Microsoft.Support) para proporcionar una escala de tiempo detallada de la migración.

### <a name="to-request-an-extension"></a>Solicitud de una extensión

1. Inicie sesión en Azure Portal y cree una [nueva solicitud de soporte técnico](https://portal.azure.com/#create/Microsoft.Support).
1. Rellene el formulario de solicitud de soporte técnico tal como se muestra en el ejemplo siguiente.

    | Campo de solicitud de soporte técnico | Value |
    |-----------------------|--------------------|
    | **Tipo de problema** | `Technical` |
    | **Suscripción** | Establecimiento de la suscripción |
    | **Servicio** | `All services` |
    | **Recurso** | `General question/Resource not available` |
    | **Tipo de problema** | `ACS to SAS Migration` |
    | **Subject** | Descripción del problema |

   ![Muestra un ejemplo de una nueva solicitud de soporte técnico](./media/howto-reactivate-disabled-acs-namespaces/new-technical-support-request.png)

<!--

1. Navigate to your ACS namespace's management portal by going to `https://{your-namespace}.accesscontrol.windows.net`.
1. Select the **Read Terms** button to read the [updated Terms of Use](https://azure.microsoft.com/support/legal/access-control/), which will direct you to a page with the updated Terms of Use.

    [![Select the Read Terms button](./media/howto-reactivate-disabled-acs-namespaces/read-terms-button-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/read-terms-button-expanded.png#lightbox)

1. Select **Request Extension** on the banner at the top of the page. The button will only be enabled after you read the [updated Terms of Use](https://azure.microsoft.com/support/legal/access-control/).

    [![Select the Request Extension button](./media/howto-reactivate-disabled-acs-namespaces/request-extension-button-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/request-extension-button-expanded.png#lightbox)

1. After the extension request is registered, the page will refresh with a new banner at the top of the page.

    [![Updated page with refreshed banner](./media/howto-reactivate-disabled-acs-namespaces/updated-banner-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/updated-banner-expanded.png#lightbox)
-->

## <a name="help-and-support"></a>Ayuda y soporte técnico

- Si se encuentra con algún problema después de seguir este procedimiento, póngase en contacto con el [soporte técnico de Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).
- Si tiene preguntas o comentarios sobre la retirada de ACS, póngase en contacto con nosotros en acsfeedback@microsoft.com.

## <a name="next-steps"></a>Pasos siguientes

- Revise la información sobre la retirada de ACS en [ Migración desde Azure Access Control Service](active-directory-acs-migration.md).
