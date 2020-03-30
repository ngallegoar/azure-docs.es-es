---
title: Informe de inicios de sesión peligrosos en el portal | Microsoft Docs
description: Obtenga información acerca del informe de inicios de sesión poco seguros del portal de Azure Active Directory
services: active-directory
author: MarkusVi
manager: daveba
ms.assetid: 7728fcd7-3dd5-4b99-a0e4-949c69788c0f
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 03/04/2020
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: b77486064139895799ac5a48327377154f75da6d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273838"
---
# <a name="risky-sign-ins-report-in-the-azure-active-directory-portal"></a>Informe de inicios de sesión poco seguros del portal de Azure Active Directory

Azure Active Directory (Azure AD) detecta acciones sospechosas relacionadas con las cuentas de usuario. Para cada acción detectada, se crea un registro denominado **detección de riesgos**. Para más información, consulte [Detecciones de riesgos de Azure AD](concept-risk-events.md). 

Puede acceder a los informes de seguridad desde [Azure Portal](https://portal.azure.com); para ello, seleccione la hoja **Azure Active Directory** y, luego, vaya a la sección **Seguridad**. 

Es posible calcular dos informes de seguridad distintos en función de las detecciones de riesgos:

- **Inicios de sesión peligrosos**: un inicio de sesión peligroso es un indicador de un intento de inicio de sesión que puede haber realizado alguien que no es el propietario legítimo de una cuenta de usuario.

- **Usuarios marcados en riesgo**: un usuario en peligro es un indicador de una cuenta de usuario que puede haber estado en peligro. 

![Inicios de sesión no seguros](./media/concept-risky-sign-ins/10.png)

Para aprender a configurar las directivas que desencadenan estas detecciones de riesgos, consulte [Procedimiento de configuración de la directiva de riesgos del usuario](../identity-protection/howto-user-risk-policy.md).  

## <a name="who-can-access-the-risky-sign-ins-report"></a>¿Quién puede acceder al informe de inicios de sesión de riesgo?

Los informes de inicios de sesión de riesgo están disponibles para los usuarios con los siguientes roles:

- Administrador de seguridad
- Administrador global
- Lector de seguridad

Para obtener información sobre cómo asignar roles administrativos a un usuario en Azure Active Directory, consulte [Ver y asignar roles de administrador en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-manage-roles-portal).

## <a name="what-azure-ad-license-do-you-need-to-access-a-security-report"></a>¿Qué licencia de Azure AD se necesita para acceder a un informe de seguridad?  

Todas las ediciones de Azure AD le proporcionan informes sobre inicios de sesión de riesgo. Sin embargo, el nivel de granularidad del informe varía según la edición: 

- En la **edición Azure Active Directory Free**, puede obtener una lista de inicios de sesión de riesgo. 

- Además, la edición **Azure Active Directory Premium 1** le permite examinar algunas de las detecciones de riesgos subyacentes que se han detectado en cada informe. 

- La edición **Azure Active Directory Premium 2** le proporciona la información más detallada acerca de todas las detecciones de riesgos subyacentes y también le permite configurar directivas de seguridad que responden automáticamente a los niveles de riesgos configurados.

## <a name="risky-sign-ins-report-for-azure-ad-free-edition"></a>Informe de inicios de sesión de riesgo de la edición Azure AD Free

La edición Azure AD Free proporciona una lista de inicios de sesión de riesgo que se han detectado para los usuarios. Cada registro contiene los siguientes atributos:

- **Usuario**: el nombre del usuario que se usó durante la operación de inicio de sesión.
- **IP**: la dirección IP del dispositivo que se usó para conectarse a Azure Active Directory.
- **Ubicación**: la ubicación usada para conectarse a Azure Active Directory. Se trata de una mejor aproximación basada en seguimientos, datos del registro, búsquedas inversas y otra información.
- **Hora de inicio de sesión**: la hora en que se realizó el inicio de sesión
- **Estado**: el estado del inicio de sesión

![Inicios de sesión no seguros](./media/concept-risky-sign-ins/01.png)

Con los datos derivados de la investigación del inicio de sesión de riesgo, puede proporcionar información a Azure AD en forma de las siguientes acciones:

- Resolver
- Marcar como falso positivo
- Ignore
- Reactivar

![Inicios de sesión no seguros](./media/concept-risky-sign-ins/21.png)

Este informe también proporciona una opción para:

- Búsqueda de recursos
- Descargar los datos de informe

![Inicios de sesión no seguros](./media/concept-risky-sign-ins/93.png)

## <a name="risky-sign-ins-report-for-azure-ad-premium-editions"></a>Informe de inicios de sesión de riesgo para las ediciones de Azure AD Premium

El informe de inicios de sesión de riesgo de las ediciones Azure AD Premium proporciona lo siguiente:

- Información agregada sobre los [tipos de detecciones de riesgos](concept-risk-events.md) que se han detectado. Con la **edición de Azure AD Premium P1**, las detecciones que no están cubiertas por su licencia aparecen como la detección de riesgos **Inicio de sesión con riesgo adicional detectado**. Con la **edición de Azure AD Premium P2**, obtiene la información más detallada acerca de todas las detecciones subyacentes.

- Una opción para descargar el informe

![Inicios de sesión no seguros](./media/concept-risky-sign-ins/456.png)

Cuando selecciona una detección de riesgos, obtiene una vista detallada del informe para esta detección de riesgos que le permite:

- Una opción para configurar una [directiva de corrección de riesgos de usuario](../identity-protection/howto-user-risk-policy.md)  

- Revisar la escala de tiempo de detección de la detección de riesgos  

- Revisar una lista de usuarios para los que se ha detectado esta detección de riesgos

- Cierre manualmente las detecciones de riesgos. 

![Inicios de sesión no seguros](./media/concept-risky-sign-ins/457.png)

> [!IMPORTANT]
> En ocasiones, es posible que encuentre una detección de riesgos sin una entrada de inicio de sesión correspondiente en el [informe de inicios de sesión](concept-sign-ins.md). Esto se debe a que Identity Protection evalúa el riesgo para inicios de sesión tanto **interactivos** como **no interactivos**, mientras que el informe de inicios de sesión muestra solo los inicios de sesión interactivos.

Cuando selecciona un usuario, obtiene una vista detallada del informe para este usuario que le permite:

- Abrir la vista de todos los inicios de sesión

- Restablezca la contraseña del usuario.

- Descartar todos los eventos

- Investigar las detecciones de riesgos notificados para el usuario. 

![Inicios de sesión no seguros](./media/concept-risky-sign-ins/324.png)

Para investigar una detección de riesgos, seleccione uno de la lista.  
Se abre la hoja **Detalles** de esta detección de riesgos. En la hoja **Detalles**, tiene la opción de cerrar manualmente una detección de riesgos o de volver a activar una detección de riesgos cerrada manualmente. 

![Inicios de sesión no seguros](./media/concept-risky-sign-ins/325.png)

## <a name="next-steps"></a>Pasos siguientes

- [Procedimiento de configuración de la directiva de riesgo del usuario](../identity-protection/howto-user-risk-policy.md)
- [Procedimiento de configuración de la directiva de corrección de riesgo del usuario](../identity-protection/howto-user-risk-policy.md)
- [Tipos de detección de riesgos](concept-risk-events.md)
