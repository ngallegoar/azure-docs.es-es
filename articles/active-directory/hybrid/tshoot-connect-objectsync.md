---
title: 'Azure AD Connect: solución de problemas de sincronización de objetos | Microsoft Docs'
description: En este tema se proporcionan los pasos para solucionar problemas relacionados con la sincronización de objetos mediante la tarea de solución de problemas.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 04/29/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 73d4239dd34f2a64aa7b3edbf88bad4348e01291
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85356209"
---
# <a name="troubleshoot-object-synchronization-with-azure-ad-connect-sync"></a>Solución de problemas de sincronización de objetos con Azure AD Connect Sync
En este artículo se proporcionan los pasos para solucionar problemas relacionados con la sincronización de objetos mediante la tarea de solución de problemas. Para ver cómo se solucionan problemas en Azure Active Directory (Azure AD) Connect, vea [este breve vídeo](https://aka.ms/AADCTSVideo).

## <a name="troubleshooting-task"></a>Tarea de solución de problemas
Para la implementación de Azure AD Connect con la versión 1.1.749.0 o superior, use la tarea de solución de problemas del asistente para solucionar problemas de sincronización de objetos. En versiones anteriores, solucione los problemas manualmente como se describe [aquí](tshoot-connect-object-not-syncing.md).

### <a name="run-the-troubleshooting-task-in-the-wizard"></a>Ejecución de la tarea de solución de problemas en el asistente
Para ejecutar la tarea de solución de problemas en el asistente, realice los pasos siguientes:

1.  Abra una nueva sesión de Windows PowerShell en el servidor de Azure AD Connect mediante la opción Ejecutar como administrador.
2.  Ejecute `Set-ExecutionPolicy RemoteSigned` o `Set-ExecutionPolicy Unrestricted`.
3.  Inicie el asistente de Azure AD Connect.
4.  Vaya a la página Additional Tasks (Tareas adicionales), seleccione Troubleshoot (Solucionar problemas) y haga clic en Next (Siguiente).
5.  En la página de solución de problemas, haga clic en Iniciar para iniciar el menú de solución de problemas en PowerShell.
6.  En el menú principal, seleccione Troubleshoot Object Synchronization (Solucionar problemas de sincronización de objetos).
![Solucionar problemas de sincronización de objetos](media/tshoot-connect-objectsync/objsynch11.png)

### <a name="troubleshooting-input-parameters"></a>Solución de problemas de parámetros de entrada
Los siguientes parámetros de entrada son necesarios para la tarea de solución de problemas:
1.  **Nombre distintivo del objeto**: es el nombre distintivo del objeto que necesita solución de problemas
2.  **Nombre del conector de AD**: es el nombre del bosque de AD donde reside el objeto anterior.
3.  Credenciales de administrador global del inquilino de Azure AD ![Credenciales de administrador global](media/tshoot-connect-objectsync/objsynch1.png)

### <a name="understand-the-results-of-the-troubleshooting-task"></a>Descripción de los resultados de la tarea de solución de problemas
La tarea de solución de problemas realiza las siguientes comprobaciones:

1.  Detecta errores de coincidencia de UPN si el objeto se ha sincronizado con Azure Active Directory
2.  Comprueba si el objeto se filtra debido al filtrado de dominio
3.  Comprueba si el objeto se filtra debido al filtrado de unidad organizativa
4.  Compruebe si la sincronización de objetos se bloquea debido a un buzón vinculado
5. Compruebe si el objeto es el grupo de distribución dinámico que no debe estar sincronizado

En el resto de esta sección se describen los resultados concretos que devuelve la tarea. En cada caso, la tarea proporciona un análisis seguido de acciones recomendadas para resolver el problema.

## <a name="detect-upn-mismatch-if-object-is-synced-to-azure-active-directory"></a>Detecta errores de coincidencia de UPN si el objeto se ha sincronizado con Azure Active Directory
### <a name="upn-suffix-is-not-verified-with-azure-ad-tenant"></a>El sufijo de UPN no se comprueba con el inquilino de Azure AD
Cuando el sufijo del identificador de inicio de sesión UserPrincipalName (UPN)/alternativo no se comprueba con el inquilino de Azure AD, Azure Active Directory reemplaza los sufijos de UPN por el nombre de dominio predeterminado "onmicrosoft.com".

![Azure AD reemplaza a UPN](media/tshoot-connect-objectsync/objsynch2.png)

### <a name="azure-ad-tenant-dirsync-feature-synchronizeupnformanagedusers-is-disabled"></a>La característica "SynchronizeUpnForManagedUsers" de DirSync del inquilino de Azure AD está deshabilitada
Cuando la característica "SynchronizeUpnForManagedUsers" de DirSync del inquilino de Azure AD está deshabilitada, Azure Active Directory no permite actualizaciones de sincronización con el identificador de inicio de sesión UserPrincipalName/alternativo en las cuentas de usuario con licencia con autenticación administrada.

![SynchronizeUpnForManagedUsers](media/tshoot-connect-objectsync/objsynch4.png)

## <a name="object-is-filtered-due-to-domain-filtering"></a>El objeto se filtra debido al filtrado de dominio
### <a name="domain-is-not-configured-to-sync"></a>El dominio no está configurado para la sincronización
El objeto está fuera del ámbito porque el dominio no está configurado. En el ejemplo siguiente, el objeto está fuera del ámbito de sincronización dado que el dominio al que pertenece está excluido de la sincronización.

![El dominio no está configurado para la sincronización](media/tshoot-connect-objectsync/objsynch5.png)

### <a name="domain-is-configured-to-sync-but-is-missing-run-profilesrun-steps"></a>El dominio está configurado para sincronizarse, pero le faltan pasos y perfiles de ejecución
El objeto está fuera del ámbito porque faltan pasos y perfiles de ejecución en el dominio. En el ejemplo siguiente, el objeto está fuera del ámbito de sincronización dado que el dominio al que pertenece carece de los pasos de ejecución para el perfil de ejecución de importación completa.
![Perfiles de ejecución faltantes](media/tshoot-connect-objectsync/objsynch6.png)

## <a name="object-is-filtered-due-to-ou-filtering"></a>El objeto se filtra debido al filtrado de unidad organizativa
El objeto está fuera del ámbito de sincronización debido a la configuración del filtrado de unidad organizativa. En el ejemplo siguiente, el objeto pertenece a OU=NoSync,DC=bvtadwbackdc,DC=com.  Esta unidad organizativa no está incluida en el ámbito de sincronización.</br>

![UNIDAD ORGANIZATIVA](./media/tshoot-connect-objectsync/objsynch7.png)

## <a name="linked-mailbox-issue"></a>Problema de buzón vinculado
Un buzón vinculado debe estar asociado con una cuenta maestra externa ubicada en otro bosque de cuenta de confianza. Si hay ninguna cuenta maestra externa, Azure AD Connect no sincronizará la cuenta de usuario que se corresponde con el buzón vinculado del bosque de Exchange con el inquilino de Azure AD.</br>
![Buzón vinculado](./media/tshoot-connect-objectsync/objsynch12.png)

## <a name="dynamic-distribution-group-issue"></a>Problema de grupo de distribución dinámico
Debido a diversas diferencias entre Azure Active Directory y Active Directory local, Azure AD Connect no sincroniza grupos de distribución dinámicos con el inquilino de Azure AD.

![Grupo de distribución dinámico](./media/tshoot-connect-objectsync/objsynch13.png)

## <a name="html-report"></a>Informe de HTML
Además de analizar el objeto, la tarea de solución de problemas también genera un informe de HTML que contiene todo lo que se sabe sobre el objeto. Este informe se puede compartir con el equipo de soporte técnico para profundizar en la solución de problemas, en caso necesario.

![Informe de HTML](media/tshoot-connect-objectsync/objsynch8.png)

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre la [Integración de las identidades locales con Azure Active Directory](whatis-hybrid-identity.md).
