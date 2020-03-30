---
title: Limitaciones de Azure Cloud Shell | Microsoft Docs
description: Introducción a las limitaciones de Azure Cloud Shell
services: azure
documentationcenter: ''
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/15/2018
ms.author: damaerte
ms.openlocfilehash: 092dccab82326bb9983f11ff64fe50aee7b1084d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74951486"
---
# <a name="limitations-of-azure-cloud-shell"></a>Limitaciones de Azure Cloud Shell

Azure Cloud Shell tiene las limitaciones conocidas siguientes:

## <a name="general-limitations"></a>Limitaciones generales

### <a name="system-state-and-persistence"></a>Persistencia y estado del sistema

La máquina que proporciona la sesión de Cloud Shell es temporal y se recicla después de que la sesión esté inactiva durante 20 minutos. Cloud Shell requiere montar un recurso compartido de archivos de Azure. Como resultado, la suscripción debe poder configurar los recursos de almacenamiento para tener acceso a Cloud Shell. Otras consideraciones:

* Con el almacenamiento montado, solo se conservan las modificaciones dentro del directorio `$Home`.
* Solo se pueden montar recursos compartidos de archivos de Azure desde la [región asignada](persisting-shell-storage.md#mount-a-new-clouddrive).
  * En Bash, ejecute `env` para buscar la región establecida como `ACC_LOCATION`.

### <a name="browser-support"></a>Compatibilidad con exploradores

Cloud Shell es compatible con las versiones más recientes de Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox y Safari de Apple. Safari en modo privado no es compatible.

### <a name="copy-and-paste"></a>Copiar y pegar

[!INCLUDE [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="for-a-given-user-only-one-shell-can-be-active"></a>Solo puede haber un shell activo para un usuario determinado

Los usuarios solo pueden iniciar un tipo de shell a la vez, ya sea **Bash** o **PowerShell**. Pero puede haber varias instancias de Bash o PowerShell en ejecución al mismo tiempo. Los cambios entre Bash y PowerShell mediante el menú provocan el reinicio de Cloud Shell, lo que finaliza las sesiones existentes. Como alternativa, puede escribir `bash` para ejecutar Bash en PowerShell o escribir `pwsh` para ejecutar PowerShell en Bash.

### <a name="usage-limits"></a>Límites de uso

Cloud Shell está pensado para casos de uso interactivos. Por tanto, todas las sesiones que no sean de este tipo y que se prolonguen durante mucho tiempo se finalizarán sin previo aviso.

## <a name="bash-limitations"></a>Limitaciones de Bash

### <a name="user-permissions"></a>Permisos de usuario

Los permisos se establecen como usuarios normales sin acceso a sudo. No se conserva cualquier instalación fuera del directorio `$Home`.

### <a name="editing-bashrc-or-profile"></a>Edición de .bashrc o $PROFILE

Tenga cuidado al editar el archivo .bashrc o el archivo $PROFILE de PowerShell, ya que puede provocar errores inesperados en Cloud Shell.

## <a name="powershell-limitations"></a>Limitaciones de PowerShell

### <a name="azuread-module-name"></a>Nombre del módulo `AzureAD`

El nombre del módulo `AzureAD` es actualmente `AzureAD.Standard.Preview`y proporciona la misma funcionalidad.

### <a name="sqlserver-module-functionality"></a>Funcionalidad del módulo `SqlServer`

El módulo `SqlServer` incluido en Cloud Shell solo es compatible con versiones preliminares de PowerShell Core. En concreto, `Invoke-SqlCmd` aún no está disponible.

### <a name="default-file-location-when-created-from-azure-drive"></a>Ubicación del archivo predeterminada cuando se crea a partir de la unidad de Azure:

Con los cmdlets de PowerShell, los usuarios no pueden crear archivos en la unidad de Azure (Azure:). Cuando los usuarios crean nuevos archivos con otras herramientas, como vim o nano, los archivos se guardan de forma predeterminada en `$HOME`. 

### <a name="gui-applications-are-not-supported"></a>No se admiten las aplicaciones con GUI

Si el usuario ejecuta un comando que crearía un cuadro de diálogo de Windows, se ve un mensaje de error como el siguiente: `Unable to load DLL 'IEFRAME.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)`.

### <a name="large-gap-after-displaying-progress-bar"></a>Espacio grande después de mostrar la barra de progreso

Si el usuario realiza una acción que muestra una barra de progreso, como una finalización con tabulación mientras está en la unidad `Azure:`, puede que el cursor no se establezca correctamente y que aparezca un espacio donde antes aparecía la barra de progreso.

## <a name="next-steps"></a>Pasos siguientes

[Solución de problemas de Cloud Shell](troubleshooting.md) <br>
[Inicio rápido de Bash](quickstart.md) <br>
[Guía de inicio rápido de PowerShell](quickstart-powershell.md)
