---
title: 'Publicar aplicaciones integradas en Windows Virtual Desktop: Azure'
description: Publicación de aplicaciones integradas en Windows Virtual Desktop
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 3a238a5ae282d5059eee23b4bcaa86a3e84b6656
ms.sourcegitcommit: 602e6db62069d568a91981a1117244ffd757f1c2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2020
ms.locfileid: "82863304"
---
# <a name="publish-built-in-apps-in-windows-virtual-desktop"></a>Publicar aplicaciones integradas en Windows Virtual Desktop

>[!IMPORTANT]
>Este contenido se aplica a la actualización de primavera de 2020 con objetos de Windows Virtual Desktop para Azure Resource Manager. Si usa la versión de otoño de 2019 de Windows Virtual Desktop sin objetos de Azure Resource Manager, consulte [este artículo](./virtual-desktop-fall-2019/publish-apps-2019.md).
>
> La actualización de primavera de 2020 de Windows Virtual Desktop se encuentra actualmente en versión preliminar pública. Esta versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. 
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo se explica cómo publicar aplicaciones en el entorno de Windows Virtual Desktop.

## <a name="publish-built-in-apps"></a>Publicación de aplicaciones integradas

Para publicar una aplicación integrada:

1. Conéctese a una de las máquinas virtuales del grupo de hosts.
2. Para obtener el elemento **PackageFamilyName** de la aplicación que quiere publicar, siga las instrucciones de [este artículo](/powershell/module/appx/get-appxpackage?view=win10-ps/).
3. Por último, ejecute el siguiente cmdlet con `<PackageFamilyName>` reemplazado por el elemento **PackageFamilyName** que encontró en el paso anterior:
   
   ```powershell
   New-AzWvdApplication -Name <applicationname> -ResourceGroupName <resourcegroupname> -ApplicationGroupName <appgroupname> -FilePath "shell:appsFolder\<PackageFamilyName>!App" -CommandLineSetting <Allow|Require|DoNotAllow> -IconIndex 0 -IconPath <iconpath> -ShowInPortal:$true
   ```

>[!NOTE]
> Windows Virtual Desktop solo admite la publicación de aplicaciones con ubicaciones de instalación que comienzan por `C:\Program Files\WindowsApps`.

## <a name="update-app-icons"></a>Actualizar los iconos de aplicación

Después de publicar una aplicación, tendrá el icono de aplicación de Windows predeterminado, en lugar de su imagen de icono normal. Para cambiar el icono a su icono normal, coloque la imagen del icono que quiera en un recurso compartido de red. Los formatos de imagen admitidos son PNG, BMP, GIF, JPG, JPEG e ICO.

## <a name="publish-microsoft-edge"></a>Publicar Microsoft Edge

El proceso que se usa para publicar Microsoft Edge es un poco diferente del proceso de publicación de otras aplicaciones. Para publicar Microsoft Edge con la página principal predeterminada, ejecute este cmdlet:

```powershell
New-AzWvdApplication -Name -ResourceGroupName -ApplicationGroupName -FilePath "shell:Appsfolder\Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge" -CommandLineSetting <Allow|Require|DoNotAllow> -iconPath "C:\Windows\SystemApps\Microsoft.MicrosoftEdge_8wekyb3d8bbwe\microsoftedge.exe" -iconIndex 0 -ShowInPortal:$true 
```

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a configurar fuentes para organizar cómo se muestran las aplicaciones para los usuarios en [Personalización de fuente para usuarios de Windows Virtual Desktop](customize-feed-for-virtual-desktop-users.md).
- Aprenda sobre la característica de asociación de aplicaciones de MSIX en [Configuración de la asociación de aplicaciones en formato .MSIX](app-attach.md).

