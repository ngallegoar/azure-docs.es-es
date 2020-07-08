---
title: 'Publicar aplicaciones integradas en Windows Virtual Desktop: Azure'
description: Publicación de aplicaciones integradas en Windows Virtual Desktop
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 7a453ef44a1ede86290f4130a6147eaaad09fa97
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85214152"
---
# <a name="publish-built-in-apps-in-windows-virtual-desktop"></a>Publicar aplicaciones integradas en Windows Virtual Desktop

>[!IMPORTANT]
>Este contenido se aplica a la versión Fall 2019 que no admite objetos de Windows Virtual Desktop para Azure Resource Manager. Si está tratando de administrar objetos de Windows Virtual Desktop para Azure Resource Manager introducidos en la actualización de Spring 2020, consulte [este artículo](../publish-apps.md).

En este artículo se explica cómo publicar aplicaciones en el entorno de Windows Virtual Desktop.

## <a name="publish-built-in-apps"></a>Publicación de aplicaciones integradas

Para publicar una aplicación integrada:

1. Conéctese a una de las máquinas virtuales del grupo de hosts.
2. Para obtener el elemento **PackageFamilyName** de la aplicación que quiere publicar, siga las instrucciones de [este artículo](/powershell/module/appx/get-appxpackage?view=win10-ps/).
3. Por último, ejecute el siguiente cmdlet con `<PackageFamilyName>` reemplazado por el elemento **PackageFamilyName** que encontró en el paso anterior:

   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -FriendlyName <remoteappname> -FilePath "shell:appsFolder\<PackageFamilyName>!App"
   ```

>[!NOTE]
> Windows Virtual Desktop solo admite la publicación de aplicaciones con ubicaciones de instalación que comienzan por `C:\Program Files\Windows Apps`.

## <a name="update-app-icons"></a>Actualizar los iconos de aplicación

Después de publicar una aplicación, tendrá el icono de aplicación de Windows predeterminado, en lugar de su imagen de icono normal. Para cambiar el icono a su icono normal, coloque la imagen del icono que quiera en un recurso compartido de red. Los formatos de imagen admitidos son PNG, BMP, GIF, JPG, JPEG e ICO.

## <a name="publish-microsoft-edge"></a>Publicar Microsoft Edge

El proceso que se usa para publicar Microsoft Edge es un poco diferente del proceso de publicación de otras aplicaciones. Para publicar Microsoft Edge con la página principal predeterminada, ejecute este cmdlet:

```powershell
New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -FriendlyName <remoteappname> -FilePath "shell:Appsfolder\Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge"
```

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a configurar fuentes para organizar cómo se muestran las aplicaciones para los usuarios en [Personalización de fuente para usuarios de Windows Virtual Desktop](customize-feed-virtual-desktop-users-2019.md).
- Aprenda sobre la característica de asociación de aplicaciones de MSIX en [Configuración de la asociación de aplicaciones en formato .MSIX](../app-attach.md).

