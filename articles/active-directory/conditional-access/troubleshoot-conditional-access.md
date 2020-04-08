---
title: 'Resolución de problemas de inicio de sesión con el acceso condicional: Azure Active Directory'
description: En este artículo se describe qué hacer cuando las directivas de acceso condicional producen resultados inesperados
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: troubleshooting
ms.date: 03/13/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, martinco
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdf1daca79f3ed20d9b7a89af20d74ff5f3148b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80337294"
---
# <a name="troubleshooting-sign-in-problems-with-conditional-access"></a>Resolución de problemas de inicio de sesión con el acceso condicional

La información de este artículo se puede usar para solucionar problemas de resultados inesperados de inicio de sesión relacionados con el acceso condicional mediante mensajes de error y el registro de inicios de sesión de Azure AD.

## <a name="conditional-access-sign-in-interrupt"></a>Interrupción de inicio de sesión del acceso condicional

El primer método consiste en revisar el mensaje de error que aparece. En el caso de los problemas de inicio de sesión al usar un explorador web, la página de error en sí contiene información detallada. Esta información por sí sola puede describir cuál es el problema y sugerir una solución.

![Error de inicio de sesión: se requiere un dispositivo compatible](./media/troubleshoot-conditional-access/image1.png)

En el error anterior, el mensaje indica que solo se puede obtener acceso a la aplicación desde dispositivos o aplicaciones cliente que cumplan la directiva de administración de dispositivos móviles de la empresa. En este caso, la aplicación y el dispositivo no cumplen la directiva.

## <a name="azure-ad-sign-in-events"></a>Eventos de inicio de sesión de Azure AD

El segundo método para obtener información detallada sobre la interrupción del inicio de sesión consiste en revisar los eventos de inicio de sesión de Azure AD para ver qué directivas de acceso condicional se han aplicado y por qué.

Para obtener más información sobre el problema, haga clic en **Más detalles** en la página inicial del error. Al hacer clic en **Más detalles** se mostrará información de resolución de problemas que resulta útil al buscar en los eventos de inicio de sesión de Azure AD el evento de error concreto que ha visto el usuario o al abrir un incidente de soporte técnico con Microsoft.

![Más detalles de un inicio de sesión de explorador web interrumpido de acceso condicional.](./media/troubleshoot-conditional-access/image2.png)

Para averiguar qué directiva o directivas de acceso condicional se han aplicado y por qué, haga lo siguiente.

1. Inicie sesión en **Azure Portal** como administrador global, de seguridad o lector global.
1. Vaya a **Azure Active Directory** > **Inicios de sesión**.
1. Busque el evento del inicio de sesión que se va a revisar. Agregue o quite filtros y columnas para filtrar la información innecesaria.
   1. Agregue filtros para limitar el ámbito:
      1. **Id. de correlación** si tiene un evento específico para investigar.
      1. **Acceso condicional** para ver el mensaje de error y de confirmación de la directiva. Defina el ámbito del filtro para mostrar solo los errores a fin de limitar los resultados.
      1. **Nombre de usuario** para ver información relacionada con usuarios específicos.
      1. **Fecha** con el ámbito definido para el período de tiempo en cuestión.

   ![Selección del filtro de acceso condicional en el registro de inicios de sesión](./media/troubleshoot-conditional-access/image3.png)

1. Una vez que se haya encontrado el evento de inicio de sesión que corresponde al error de inicio de sesión del usuario, seleccione la pestaña **Acceso condicional**. En la pestaña Acceso condicional se mostrará la directiva o directivas específicas que han provocado la interrupción del inicio de sesión.
   1. La información de la pestaña **Solución de problemas y soporte técnico** puede proporcionar un motivo claro de la causa del error de inicio de sesión, como, por ejemplo, un dispositivo que no cumple los requisitos de cumplimiento.
   1. Para seguir investigando, explore en profundidad la configuración de las directivas haciendo clic en el **nombre de la directiva**. Al hacer clic en el **nombre de la directiva** se mostrará la interfaz de usuario de configuración de la directiva seleccionada para revisarla y editarla.
   1. El **usuario de cliente** y los **detalles del dispositivo** usados para la evaluación de la directiva de acceso condicional también están disponibles en las pestañas **Información básica**, **Ubicación**, **Información del dispositivo**, **Detalles de autenticación** y **Detalles adicionales** del evento de inicio de sesión.

   ![Pestaña Acceso condicional de un evento de inicio de sesión](./media/troubleshoot-conditional-access/image5.png)

Si la información del evento no es suficiente para comprender los resultados de inicio de sesión o para ajustar la directiva a fin de obtener los resultados deseados, se puede abrir un incidente de soporte técnico. Vaya a la pestaña **Solución de problemas y soporte técnico** de ese evento de inicio de sesión y seleccione **Crear una solicitud de soporte técnico**.

![Pestaña Solución de problemas y soporte técnico del evento de inicio de sesión](./media/troubleshoot-conditional-access/image6.png)

Al enviar el incidente, proporcione el id. de solicitud y la fecha y hora del evento de inicio de sesión en los detalles del envío del incidente. Esta información permitirá al servicio de soporte técnico de Microsoft encontrar el evento que le preocupa.

### <a name="conditional-access-error-codes"></a>Códigos de error del acceso condicional

| Código de error de inicio de sesión | Cadena de error |
| --- | --- |
| 53000 | DeviceNotCompliant |
| 53001 | DeviceNotDomainJoined |
| 53002 | ApplicationUsedIsNotAnApprovedApp |
| 53003 | BlockedByConditionalAccess |
| 53004 | ProofUpBlockedDueToRisk |

## <a name="next-steps"></a>Pasos siguientes

- [Informes de actividad de inicio de sesión en el portal de Azure Active Directory](../reports-monitoring/concept-sign-ins.md)
- [Solución de problemas de acceso condicional con la herramienta What If](troubleshoot-conditional-access-what-if.md)
- Procedimientos recomendados para el [acceso condicional en Azure Active Directory](best-practices.md)
