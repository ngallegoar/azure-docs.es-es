---
title: 'Configuración de Azure Multi-Factor Authentication para Windows Virtual Desktop: Azure'
description: Cómo configurar Azure Multi-Factor Authentication para mejorar la seguridad en Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: b470f9278bdca94d1fe98c64b11b070fb36cb075
ms.sourcegitcommit: 25490467e43cbc3139a0df60125687e2b1c73c09
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2020
ms.locfileid: "80998473"
---
# <a name="set-up-azure-multi-factor-authentication"></a>Configuración de Azure Multi-Factor Authentication

El cliente de Windows para Windows Virtual Desktop es una opción excelente para integrar Windows Virtual Desktop con el equipo local. Sin embargo, al configurar la cuenta de Windows Virtual Desktop en el cliente de Windows, hay ciertas medidas que debe seguir para mantener a todos los usuarios protegidos.

Al iniciar sesión por primera vez, el cliente solicita el nombre de usuario, la contraseña y Azure MFA. Posteriormente, la próxima vez que inicie sesión, el cliente recordará el token de la aplicación empresarial de Azure Active Directory (AD). Al seleccionar **Recordarme**, los usuarios pueden iniciar sesión después de reiniciar el cliente sin necesidad de volver a escribir sus credenciales.

Aunque recordar credenciales resulta práctico, también puede hacer que las implementaciones en escenarios empresariales o dispositivos personales sean menos seguras. Para proteger a los usuarios, debe asegurarse de que el cliente sigue solicitando las credenciales de Azure Multi-Factor Authentication (MFA). En este artículo se muestra cómo configurar la directiva de acceso condicional de Windows Virtual Desktop para habilitar esta opción de configuración.

## <a name="prerequisites"></a>Prerrequisitos

Requisitos para poder comenzar:

- Asigne a todos los usuarios una de las licencias siguientes:
  - Microsoft 365 E3 o E5
  - Azure Active Directory Premium P1 o P2
  - Enterprise Mobility + Security E3 o E5
- Un grupo de Azure Active Directory con los usuarios asignados como miembros del grupo.
- Habilite Azure MFA para todos los usuarios. Para obtener más información sobre cómo realizar ese proceso, consulte [Exigencia de verificación en dos pasos para un usuario](../active-directory/authentication/howto-mfa-userstates.md#view-the-status-for-a-user).

>[!NOTE]
>La configuración siguiente también se aplica al [cliente web de Windows Virtual Desktop](https://rdweb.wvd.microsoft.com/webclient/index.html).

## <a name="opt-in-to-the-conditional-access-policy"></a>Aceptación de la directiva de acceso condicional

1. Abra **Azure Active Directory**.

2. Vaya a la pestaña **Todas las aplicaciones**. En el menú desplegable "Tipo de aplicación", seleccione **Aplicaciones empresariales** y, a continuación, busque **Cliente de Windows Virtual Desktop**.

    ![Una captura de pantalla de la pestaña Todas las aplicaciones. El usuario escribió "cliente de Windows Virtual Desktop" en la barra de búsqueda y la aplicación aparece en los resultados de la búsqueda.](media/all-applications-search.png)

3. Seleccione **Acceso condicional**.

    ![Captura de pantalla que muestra que el usuario mantiene el cursor del mouse sobre la pestaña Acceso condicional.](media/conditional-access-location.png)

4. Seleccione **+ Nueva directiva**.

   ![Captura de pantalla de la página Acceso condicional. El usuario mantiene el cursor del mouse sobre el botón Nueva directiva.](media/new-policy-button.png)

5. Escriba un **nombre** para la **regla** y después **seleccione** el *nombre del **grupo** que creó en los requisitos previos.

6. Seleccione **Seleccionar** y luego **Listo**.

7. Después, abra **Aplicaciones en la nube o acciones**.

8. En el panel **Seleccionar**, seleccione la aplicación empresarial **Windows Virtual Desktop**.

    ![Captura de pantalla de la página Aplicaciones en la nube o acciones. El usuario ha seleccionado la aplicación Windows Virtual Desktop al activar la marca de verificación situada junto a ella. La aplicación seleccionada está resaltada con rojo.](media/cloud-apps-select.png)
    
    >[!NOTE]
    >La aplicación cliente de Windows Virtual Desktop también debería mostrarse seleccionada en el lado izquierdo de la pantalla, tal como se muestra en la siguiente imagen. Necesita la aplicación empresarial de Windows Virtual Desktop y del cliente de Windows Virtual Desktop para que la directiva funcione.
    >
    > ![Captura de pantalla de la página Aplicaciones en la nube o acciones. Las aplicaciones de Windows Virtual Desktop y del cliente de Windows Virtual Desktop están resaltadas con rojo.](media/cloud-apps-enterprise-selected.png)

9. Seleccione la opción **Seleccionar**.

10. Después, abra **Conceder**. 

11. Seleccione **Requerir autenticación multifactor** y, a continuación, **Requerir uno de los controles seleccionados**.
   
    ![Captura de pantalla de la página Conceder. Está seleccionada la opción "Requerir autenticación multifactor".](media/grant-page.png)

    >[!NOTE]
    >Si tiene dispositivos inscritos en MDM en su organización y no quiere que muestren la solicitud de MFA, también puede seleccionar **Requerir que el dispositivo esté marcado como compatible**.

12. Seleccione **Sesión**.

13. Establezca la **Frecuencia de inicio de sesión** en **Activa** y, a continuación, cambie su valor a **1 hora**.

    ![Captura de pantalla de la página Sesión. En el menú Sesión se muestra el menú desplegable Frecuencia de inicio de sesión que se ha cambiado a los valores "1" y "horas".](media/sign-in-frequency.png)
   
    >[!NOTE]
    >Las sesiones activas en el entorno de Windows Virtual Desktop seguirán funcionando cuando cambie la directiva. Sin embargo, si se desconecta o se cierra la sesión, deberá volver a especificar sus credenciales después de 60 minutos. A medida que cambia la configuración, puede ampliar el período de tiempo de espera tanto como quiera (siempre que coincida con la directiva de seguridad de su organización).
    >
    >La configuración predeterminada es un plazo acumulable de 90 días; es decir, el cliente solicitará a los usuarios que inicien sesión de nuevo cuando intenten acceder a un recurso después de estar inactivos en su equipo durante 90 días o más.

14. Habilite la directiva.

15. Seleccione **Crear** para confirmar la directiva.

¡Ya ha terminado! No dude en probar la directiva para asegurarse de que la lista de permitidos funciona según lo previsto.
