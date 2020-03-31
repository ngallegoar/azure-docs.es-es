---
title: 'Azure AD Connect: Qué es el módulo de PowerShell ADConnectivityTool | Microsoft Docs'
description: En este documento se presenta el nuevo módulo de PowerShell ADConnectivity y cómo usarlo para ayudar a solucionar problemas.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 4/25/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: cd5340cd8c802df4ffbe0207b5401d2fee4e207e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "64571121"
---
# <a name="troubleshoot-azure-ad-connectivity-with-the-adconnectivitytool-powershell-module"></a>Solución de problemas de conectividad de Azure AD con el módulo de PowerShell ADConnectivityTool

La herramienta ADConnectivity es un módulo de PowerShell que se usa en uno de las siguientes situaciones:

- Durante la instalación, cuando un problema de conectividad de red impide la correcta validación de las credenciales de Active Directory que el usuario proporcionó en el asistente.
- Después de la instalación que realizó un usuario que llama a las funciones de una sesión de PowerShell.

La herramienta se encuentra en: **C:\Program Files\Microsoft Azure Active Directory Connect\Tools\ ADConnectivityTool.psm1** 

## <a name="adconnectivitytool-during-installation"></a>ADConnectivityTool durante la instalación

En la página **Connect your directories** (Conecte sus directorios) del asistente de Azure AD Connect, si ocurre un problema de red, ADConnectivityTool usará automáticamente una de sus funciones para determinar qué está sucediendo.  Cualquiera de los siguientes puntos puede considerarse como un problemas de red:

- El nombre del bosque que proporcionó el usuario se escribió incorrectamente, o dicho bosque no existe. 
- El puerto UDP 389 está cerrado en los controladores de dominio asociados con el bosque que proporcionó el usuario.
- Las credenciales que se proporcionaron en la ventana "Cuenta del bosque de AD" no tienen privilegios para recuperar los controladores de dominio asociados con el bosque de destino.
- Cualquiera de los puertos UDP 53, 88 o 389 está cerrado en los controladores de dominio asociados con el bosque que proporcionó el usuario. 
- Tanto el puerto UDP 389 como un puerto o puertos TCP están cerrados.
- El DNS no se pudo resolver para el bosque o sus controladores de dominio asociados.

Cuando se encuentra alguno de estos problemas, se muestra un mensaje de error relacionado en el asistente de AADConnect:


![Error](media/how-to-connect-adconnectivitytools/error1.png)

Por ejemplo, cuando intentamos agregar un directorio en la pantalla **Connect your directories** (Conecte sus directorios), Azure AD Connect necesita comprobar esta opción y espera poder comunicarse con un controlador de dominio a través del puerto 389.  Si no puede comunicarse con él, verá el error que se muestra en la captura de pantalla anterior.  

Lo que realmente sucede en segundo plano es que Azure AD Connect está llamando a la función `Start-NetworkConnectivityDiagnosisTools`.  Se llama a esta función cuando se produce un error en la validación de credenciales debido a un problema de conectividad de red.

Finalmente, se genera un archivo de registro detallado cada vez que se llama a la herramienta desde el asistente. El registro se encuentra en **C:\ProgramData\AADConnect\ADConnectivityTool-\<fecha>-\<hora>.log**

## <a name="adconnectivitytools-post-installation"></a>ADConnectivityTools después de la instalación
Una vez instalado Azure AD Connect, se puede usar cualquiera de las funciones del módulo de PowerShell ADConnectivityTools.  

Puede encontrar información de referencia sobre las funciones en la [referencia de ADConnectivityTools](reference-connect-adconnectivitytools.md).

### <a name="start-connectivityvalidation"></a>Start-ConnectivityValidation

Vamos a llamar a esta función porque **solo** se puede llamar manualmente una vez que se ha importado ADConnectivityTool.psm1 en PowerShell. 

Esta función usa la misma lógica que el asistente de Azure AD Connect cuando la ejecuta para validar las credenciales de AD proporcionadas.  Sin embargo, proporciona una explicación mucho más detallada sobre el problema y una solución sugerida. 

La validación de conectividad consta de los pasos siguientes:
-   Obtener el objeto del nombre de dominio completo (FQDN).
-   Verificar que, si el usuario seleccionó "Crear una cuenta de AD", estas credenciales pertenecen al grupo de administradores de empresas.
-   Obtener el objeto FQDN del bosque.
-   Confirmar que se puede alcanzar al menos un dominio asociado con el objeto FQDN del bosque obtenido anteriormente.
-   Comprobar que el nivel funcional del bosque sea Windows Server 2003 o superior.

El usuario podrá agregar un directorio si todas estas acciones se ejecutaron con éxito.

Si el usuario ejecuta esta función después de resolver un problema (o si no existe ningún problema), el resultado indicará al usuario que debe volver al asistente de Azure AD Connect y reescribir las credenciales.



## <a name="next-steps"></a>Pasos siguientes
- [Azure AD Connect: cuentas y permisos](reference-connect-accounts-permissions.md)
- [Instalación rápida](how-to-connect-install-express.md)
- [Instalación personalizada](how-to-connect-install-custom.md)
- [Referencia de ADConnectivityTools](reference-connect-adconnectivitytools.md)

