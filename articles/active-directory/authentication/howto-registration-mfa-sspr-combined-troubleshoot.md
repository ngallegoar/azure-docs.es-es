---
title: 'Solución de problemas de registro combinado: Azure Active Directory'
description: Solución de problemas del registro combinado de Multi-Factor Authentication y el autoservicio de restablecimiento de contraseña de Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: troubleshooting
ms.date: 04/15/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6ae5995340640e9cda16634cf76b9953d7d663c2
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91966668"
---
# <a name="troubleshooting-combined-security-information-registration"></a>Solución de problemas de registro de información de seguridad combinado

La información de este artículo está pensada como guía para los administradores que solucionan problemas notificados por los usuarios de la experiencia de registro combinado.

## <a name="audit-logs"></a>Registros de auditoría

Los eventos registrados para un registro combinado están en la categoría de métodos de autenticación en los registros de auditoría de Azure AD.

![Interfaz de registros de auditoría de Azure AD con los eventos de registro](media/howto-registration-mfa-sspr-combined-troubleshoot/combined-security-info-audit-log.png)

En la siguiente tabla se muestran todos los eventos de auditoría generados por el registro combinado:

| Actividad | Status | Motivo | Descripción |
| --- | --- | --- | --- |
| El usuario registró toda la información de seguridad necesaria | Correcto | El usuario registró toda la información de seguridad necesaria. | Este evento se produce cuando un usuario ha completado correctamente el registro.|
| El usuario registró toda la información de seguridad necesaria | Error | El usuario canceló el registro de información de seguridad. | Este evento se produce cuando un usuario cancela el registro del modo de interrupción.|
| El usuario registró información de seguridad | Correcto | El usuario registró el *método*. | Este evento se produce cuando un usuario registra un método individual. El *método* puede ser una aplicación de autenticación, un teléfono, una dirección de correo electrónico, preguntas de seguridad, una contraseña de aplicación, un teléfono alternativo, etc.| 
| Información de seguridad revisada por el usuario | Correcto | Información de seguridad revisada correctamente por el usuario. | Este evento se produce cuando un usuario selecciona **Looks good** (Parece correcto) en la página de revisión de información de seguridad.|
| Información de seguridad revisada por el usuario | Error | El usuario no pudo revisar la información de seguridad. | Este evento se produce cuando un usuario selecciona **Looks good** (Parece correcto) en la página de revisión de información de seguridad, pero se ha producido algún problema en el back-end.|
| El usuario eliminó información de seguridad | Correcto | El usuario eliminó el *método*. | Este evento se produce cuando un usuario elimina un método individual. El *método* puede ser una aplicación de autenticación, un teléfono, una dirección de correo electrónico, preguntas de seguridad, una contraseña de aplicación, un teléfono alternativo, etc.|
| El usuario eliminó información de seguridad | Error | El usuario no pudo eliminar el *método*. | Este evento se produce cuando un usuario intenta eliminar un método pero el intento falla por alguna razón. El *método* puede ser una aplicación de autenticación, un teléfono, una dirección de correo electrónico, preguntas de seguridad, una contraseña de aplicación, un teléfono alternativo, etc.|
| El usuario cambió la información de seguridad predeterminada | Correcto | El usuario cambió la información de seguridad predeterminada para el *método*. | Este evento se produce cuando un usuario cambia el método predeterminado. El *método* puede ser una notificación de la aplicación de autenticación, un código de mi aplicación de autenticación o un token, una llamada al número +X XXXXXXXXXX, un envío de mensaje de un mensaje de texto al número +X XXXXXXXXX, etc.|
| El usuario cambió la información de seguridad predeterminada | Error | El usuario no pudo cambiar la información de seguridad predeterminada para el *método*. | Este evento se produce cuando un usuario intenta cambiar el método predeterminado, pero el intento falla por alguna razón. El *método* puede ser una notificación de la aplicación de autenticación, un código de mi aplicación de autenticación o un token, una llamada al número +X XXXXXXXXXX, un envío de mensaje de un mensaje de texto al número +X XXXXXXXXX, etc.|

## <a name="troubleshooting-interrupt-mode"></a>Solución de problemas del modo de interrupción

| Síntoma | Pasos para solucionar problemas |
| --- | --- |
| No veo los métodos que esperaba ver. | 1. Compruebe si el usuario tiene un rol de administrador de Azure AD. Si es así, vea las diferencias de la directiva de administración de autoservicio de restablecimiento de contraseña. <br> 2. Determine si se ha interrumpido el acceso al usuario debido a la aplicación del registro de autenticación multifactor o a la aplicación de registro de autoservicio de restablecimiento de contraseña. Consulte el [diagrama de flujo](../../active-directory/authentication/concept-registration-mfa-sspr-combined.md#combined-registration-modes) en "Modos de registro combinado" para determinar qué métodos se deben mostrar. <br> 3. Determine cómo se cambió la directiva de autenticación multifactor o autoservicio de restablecimiento de contraseña. Si el cambio es reciente, puede que la actualización de la directiva tarde en aplicarse.|

## <a name="troubleshooting-manage-mode"></a>Solución de problemas de modo de administración

| Síntoma | Pasos para solucionar problemas |
| --- | --- |
| No tengo la opción de agregar un método determinado. | 1. Determine si el método está habilitado para la autenticación multifactor o para el autoservicio de restablecimiento de contraseña. <br> 2. Si el método está habilitado, vuelva a guardar las directivas y espere entre 1 y 2 horas antes de volver a probar. <br> 3. Si el método está habilitado, asegúrese de que el usuario no lo haya configurado el número máximo de veces que puede hacerlo.|

## <a name="disable-combined-registration"></a>Habilitación del registro combinado

Cuando un usuario registra un número de teléfono o aplicación móvil en la nueva experiencia combinada, nuestro servicio imprime un conjunto de indicadores (StrongAuthenticationMethods) para esos métodos en ese usuario. Esta funcionalidad permite al usuario realizar la autenticación multifactor con estos métodos siempre que se requiera esa autenticación.

Si un administrador habilita la versión preliminar, los usuarios se registran mediante la nueva experiencia y, después, el administrador deshabilita la versión preliminar, los usuarios también pueden registrarse para la autenticación multifactor sin saberlo.

Si un usuario que ha completado el registro combinado llega a la página de registro de restablecimiento de autoservicio de restablecimiento de contraseña (SSPR) actual en [https://aka.ms/ssprsetup](https://aka.ms/ssprsetup), se solicitará al usuario que realice la autenticación multifactor para poder obtener acceso a esa página. Este paso es el esperado desde un punto de vista técnico, pero es nuevo para los usuarios que estaban previamente registrados en SSPR únicamente. Aunque este paso adicional mejora la postura de seguridad del usuario al proporcionar otro nivel de seguridad, los administradores pueden querer revertir a sus usuarios para que ya no puedan realizar la autenticación multifactor.  

### <a name="how-to-roll-back-users"></a>Cómo revertir a los usuarios

Si usted, como un administrador, desea restablecer la configuración de autenticación multifactor de un usuario, puede usar el script de PowerShell proporcionado en la siguiente sección. El script borrará la propiedad StrongAuthenticationMethods de la aplicación móvil o el número de teléfono de un usuario. Si ejecuta este script para los usuarios, se tendrán que volver a registrar para la autenticación multifactor cuando la necesiten. Se recomienda probar la reversión con uno o dos usuarios antes de revertir a todos los usuarios afectados.

Los pasos siguientes le ayudarán a revertir a un usuario o grupo de usuarios.

#### <a name="prerequisites"></a>Prerrequisitos

1. Instale los módulos de Azure AD PowerShell adecuados. En una ventana de PowerShell, ejecute estos comandos para instalar los módulos:

   ```powershell
   Install-Module -Name MSOnline
   Import-Module MSOnline
   ```

1. Guarde la lista de los identificadores de objetos de usuario afectados en el equipo como un archivo de texto con un identificador por línea. Tome nota de la ubicación del archivo.
1. Guarde el siguiente script en el equipo y tome nota de su ubicación:

   ```powershell
   <# 
   //********************************************************
   //*                                                      *
   //*   Copyright (C) Microsoft. All rights reserved.      *
   //*                                                      *
   //********************************************************
   #>

   param($path)

   # Define Remediation Fn
   function RemediateUser {

       param  
       (
           $ObjectId
       )

       $user = Get-MsolUser -ObjectId $ObjectId

       Write-Host "Checking if user is eligible for rollback: UPN: "  $user.UserPrincipalName  " ObjectId: "  $user.ObjectId -ForegroundColor Yellow

       $hasMfaRelyingParty = $false
       foreach($p in $user.StrongAuthenticationRequirements)
       {
           if ($p.RelyingParty -eq "*")
           {
               $hasMfaRelyingParty = $true
               Write-Host "User was enabled for per-user MFA." -ForegroundColor Yellow
           }
       }

       if ($user.StrongAuthenticationMethods.Count -gt 0 -and -not $hasMfaRelyingParty)
       {
           Write-Host $user.UserPrincipalName " is eligible for rollback" -ForegroundColor Yellow
           Write-Host "Rolling back user ..." -ForegroundColor Yellow
           Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName $user.UserPrincipalName
           Write-Host "Successfully rolled back user " $user.UserPrincipalName -ForegroundColor Green
       }
       else
       {
           Write-Host $user.UserPrincipalName " is not eligible for rollback. No action required."
       }

       Write-Host ""
       Start-Sleep -Milliseconds 750
   }

   # Connect
   Import-Module MSOnline
   Connect-MsolService

   foreach($line in Get-Content $path)
   {
       RemediateUser -ObjectId $line
   }
   ```

#### <a name="rollback"></a>Reversión

En una ventana de PowerShell, ejecute el comando siguiente y proporcione el script y las ubicaciones de archivo del usuario. Introduzca las credenciales de administrador global cuando se le solicite. El script generará el resultado de cada operación de actualización de usuarios.

`<script location> -path <user file location>`

### <a name="disable-the-updated-experience"></a>Deshabilitación de la experiencia actualizada

Para deshabilitar la experiencia actualizada para los usuarios, complete estos pasos:

1. Inicie sesión en Azure Portal como administrador de usuarios.
2. Vaya a **Azure Active Directory** > **Configuración de usuario** > **Administrar la configuración de las características en vista previa del panel de acceso**.
3. En **Los usuarios pueden utilizar las características en vista previa para registrar y administrar la información de seguridad**, establezca el selector en **Ninguno** y, a continuación, haga clic en **Guardar**.

Ya no se pedirá a los usuarios que se registren mediante la experiencia actualizada.

## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre el registro combinado para el autoservicio de restablecimiento de contraseña y Azure Multi-Factor Authentication](concept-registration-mfa-sspr-combined.md)
