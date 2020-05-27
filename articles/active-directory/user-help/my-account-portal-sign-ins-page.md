---
title: Visualización y búsqueda de la actividad de inicio de sesión reciente desde la página Mis inicios de sesión (versión preliminar) en Azure Active Directory | Microsoft Docs
description: Detalles acerca de cómo ver y buscar la actividad de inicio de sesión reciente en la página Mis inicios de sesión del portal Mi cuenta.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: rhicock
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: end-user-help
ms.date: 10/28/2019
ms.author: curtand
ms.openlocfilehash: 0b3b0d686ae10f9b376c977bf165eccddda32239
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83744527"
---
# <a name="view-and-search-your-recent-sign-in-activity-from-the-my-sign-ins-preview-page"></a>Visualización y búsqueda de la actividad de inicio de sesión reciente desde la página Mis inicios de sesión (versión preliminar)

Puede ver toda la actividad de inicio de sesión de la cuenta profesional o educativa en la página **Mis inicios de sesión** del portal **Mi cuenta**. La revisión del historial de inicios de sesión lo ayuda a comprobar la actividad inusual al permitirlo ver:

- Si alguien intenta adivinar su contraseña.

- Si un atacante inició sesión correctamente en su cuenta y desde qué ubicación.

- Las aplicaciones a las que el atacante ha intentado acceder.

## <a name="view-your-recent-sign-in-activity"></a>Visualización de las actividades de inicio de sesión recientes

1. Inicie sesión en su cuenta profesional o educativa y vaya a la página https://myaccount.microsoft.com/.

2. Seleccione **Mis inicios de sesión (versión preliminar)** en el panel de navegación izquierdo o seleccione el vínculo **Revisar la actividad reciente** en el bloque **Mis inicios de sesión (versión preliminar)** .

    ![Página Mi cuenta, que muestra vínculos resaltados a Actividad reciente](media/my-account-portal/my-account-portal-sign-ins.png)

3. Expanda y revise cada uno de los elementos de inicio de sesión, asegurándose de que reconoce cada uno de ellos. Si encuentra un elemento de inicio de sesión que no le resulta familiar, le recomendamos encarecidamente que cambie la contraseña para ayudar a proteger la cuenta en caso de que se viera comprometida.

    ![Página Actividad reciente con los detalles de inicio de sesión expandidos](media/my-account-portal/my-account-portal-sign-ins-page.png)

### <a name="if-you-see-a-successful-sign-in"></a>Si ve un inicio de sesión correcto

Debe reconocer que su propia actividad es normal. Sin embargo, si observa un inicio de sesión correcto desde una ubicación, un explorador o un sistema operativo extraño, podría significar que un atacante ha accedido a su cuenta. En esta situación, se recomienda que cambie inmediatamente la contraseña y luego vaya a la página [Información de seguridad](https://mysignins.microsoft.com/security-info) para actualizar la configuración de seguridad.

Antes de determinar si algo es incorrecto, asegúrese de que no está viendo un falso positivo (donde el elemento es cuestionable, pero es correcto). Por ejemplo, determinamos la ubicación aproximada y el mapa en función de su dirección IP. Las redes móviles son especialmente difíciles de identificar, ya que a veces enrutan el tráfico a través de ubicaciones lejanas. Por lo tanto, si ha iniciado sesión con el dispositivo móvil en el estado de Washington, la ubicación podría mostrar que el inicio de sesión procede de California. Por esta razón, se recomienda encarecidamente que consulte más detalles, además de la ubicación. También debe asegurarse de que el sistema operativo, el explorador y la aplicación tienen sentido.

### <a name="if-you-see-an-unsuccessful-sign-in"></a>Si ve un inicio de sesión incorrecto

Un inicio de sesión incorrecto, sin actividad de sesión, significa que se produjo un error en el método de verificación principal (nombre de usuario/contraseña). Esto podría significar que ha escrito mal el nombre de usuario o la contraseña, pero también podría significar que un atacante estaba intentando adivinar la contraseña. Si cree que fue un atacante que intentó adivinar su contraseña sin éxito, no tiene que cambiarla, pero le recomendamos encarecidamente que se registre en Azure Multi-Factor Authentication (MFA). Con MFA, incluso si el hacker finalmente adivina su contraseña, no será suficiente para acceder a su cuenta.

Si ve un inicio de sesión incorrecto, con una nota en la actividad de la sesión que indica, **Error de comprobación adicional, código no válido**, significa que la autenticación principal (nombre de usuario y contraseña) se realizó correctamente, pero hubo un error en MFA. Si era un atacante, adivinó correctamente la contraseña, pero todavía no pudo superar el desafío de MFA. En este caso, le recomendamos que cambie su contraseña, ya que el atacante acertó esa parte, y luego vaya a la página [Información de seguridad ](https://mysignins.microsoft.com/security-info) para actualizar la configuración de seguridad.

## <a name="search-for-specific-sign-in-activity"></a>Búsqueda de actividad de inicio de sesión específica

Puede buscar en la actividad de inicio de sesión reciente cualquier información disponible. Por ejemplo, puede buscar la actividad de inicio de sesión reciente por sistema operativo, ubicación, aplicación, etc.

1. En la página **Revisar la actividad reciente**, escriba la información que desea buscar en la barra **Búsqueda**. Por ejemplo, escriba `My Account` para buscar toda la actividad recopilada por la aplicación Mi cuenta.

2. Seleccione el botón **Buscar** para empezar a buscar.

    ![Página Actividad reciente, donde se muestra la barra de búsqueda, el botón de búsqueda y los resultados resaltados](media/my-account-portal/my-account-portal-sign-ins-page-search.png)

## <a name="next-steps"></a>Pasos siguientes

Después de ver la actividad de inicio de sesión reciente, puede:

- Ver o administrar la [información de seguridad](user-help-security-info-overview.md).

- Ver o administrar los dispositivos [conectados](my-account-portal-devices-page.md).

- Ver o administrar las [organizaciones](my-account-portal-organizations-page.md).

- Ver cómo se [usan los datos relacionados con la privacidad](my-account-portal-privacy-page.md) en la organización.
