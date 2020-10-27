---
title: 'Configuración de redireccionamientos de dispositivos: Azure'
description: Cómo configurar el redireccionamiento de dispositivos para Windows Virtual Desktop.
author: Heidilohr
ms.topic: how-to
ms.date: 09/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 00a3c1d0a2a905e6435b811d5f2611c16a5de502
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92328886"
---
# <a name="configure-device-redirections"></a>Configuración de redireccionamientos de dispositivos

La configuración de redireccionamientos de dispositivos para el entorno de Windows Virtual Desktop le permite usar impresoras, dispositivos USB, micrófonos y otros dispositivos periféricos en la sesión remota. Algunos redireccionamientos de dispositivos requieren cambios en las propiedades del Protocolo de Escritorio remoto (RDP) y en la configuración de la directiva de grupo.

## <a name="supported-device-redirections"></a>Redireccionamientos de dispositivos admitidos

Cada cliente admite diferentes redireccionamientos de dispositivos. Consulte [Comparar los clientes](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-app-compare) para obtener la lista completa de redireccionamientos de dispositivos compatibles para cada cliente.

## <a name="customizing-rdp-properties-for-a-host-pool"></a>Personalización de las propiedades de RDP para un grupo de hosts

Para obtener más información sobre cómo personalizar las propiedades de RDP para un grupo de hosts mediante PowerShell o Azure Portal, consulte las [Propiedades de RDP](customize-rdp-properties.md). Para obtener la lista completa de propiedades de RDP compatibles, consulte la [Configuración de archivos de RDP compatibles](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/rdp-files?context=/azure/virtual-desktop/context/context).

## <a name="setup-device-redirections"></a>Configuración de redireccionamientos de dispositivos

Puede usar las siguientes propiedades de RDP y la configuración de directiva de grupo para configurar redireccionamientos de dispositivos.

### <a name="audio-input-microphone-redirection"></a>Redireccionamiento de la entrada de audio (micrófono)

Establezca la siguiente propiedad RDP para configurar el redireccionamiento de la entrada de audio:

- `audiocapturemode:i:1` habilita el redireccionamiento de la entrada de audio.
- `audiocapturemode:i:0` deshabilita el redireccionamiento de la entrada de audio.

### <a name="audio-output-speaker-redirection"></a>Redireccionamiento de la salida de audio (altavoz)

Establezca la propiedad RDP siguiente para configurar el redireccionamiento de la salida de audio:

- `audiomode:i:0` habilita el redireccionamiento de la salida de audio.
- `audiomode:i:1` o `audiomode:i:2` deshabilita el redireccionamiento de la salida de audio.

### <a name="camera-redirection"></a>Redireccionamiento de la cámara

Establezca la siguiente propiedad RDP para configurar el redireccionamiento de la cámara:

- `camerastoredirect:s:*` redirige todas las cámaras.
- `camerastoredirect:s:` deshabilita el redireccionamiento de la cámara.

>[!NOTE]
>Aunque la propiedad `camerastoredirect:s:` esté deshabilitada, las cámaras locales se pueden redirigir a través de la propiedad `devicestoredirect:s:`. Para deshabilitar completamente la redirección de la cámara, configure `camerastoredirect:s:` y configure `devicestoredirect:s:` o defina algún subconjunto de dispositivos Plug and play que no incluya ninguna cámara.

También puede redirigir cámaras específicas mediante una lista delimitada por signos de punto y coma de interfaces KSCATEGORY_VIDEO_CAMERA, como `camerastoredirect:s:\?\usb#vid_0bda&pid_58b0&mi`. 

### <a name="clipboard-redirection"></a>Redireccionamiento del portapapeles

Establezca la siguiente propiedad RDP para configurar el redireccionamiento del portapapeles:

- `redirectclipboard:i:1` habilita el redireccionamiento del portapapeles.
- `redirectclipboard:i:0` deshabilita el redireccionamiento del portapapeles.

### <a name="com-port-redirections"></a>Redireccionamientos de puertos COM

Establezca la siguiente propiedad RDP para configurar el redireccionamiento del puerto COM:

- `redirectcomports:i:1` habilita el redireccionamiento de puertos COM.
- `redirectcomports:i:0` deshabilita el redireccionamiento de puertos COM.

### <a name="usb-redirection"></a>Redireccionamiento de USB

En primer lugar, establezca la siguiente propiedad RDP para habilitar el redireccionamiento de los dispositivos USB:

- `usbdevicestoredirect:s:*` habilita el redireccionamiento de los dispositivos USB.
- `usbdevicestoredirect:s:` deshabilita el redireccionamiento de los dispositivos USB.

En segundo lugar, establezca la siguiente directiva de grupo en el dispositivo local del usuario:

- Vaya a **Configuración del equipo** > **Directivas**> **Plantillas administrativas** > **Componentes de Windows** > **Servicios de Escritorio remoto** > **Cliente de conexión a Escritorio remoto** > **Redireccionamiento del dispositivo USB RemoteFX** .
- Seleccione **Permitir el redireccionamiento RDP de otros dispositivos USB RemoteFX compatibles desde este equipo** .
- Seleccione la opción **Habilitado** y, a continuación, seleccione el cuadro **Administrators and Users in RemoteFX USB Redirection Access Rights** (Administradores y usuarios en los derechos de acceso del redireccionamiento de dispositivos USB RemoteFX).
- Seleccione **Aceptar** .

### <a name="plug-and-play-device-redirection"></a>Redireccionamiento del dispositivo Plug and Play

Establezca la siguiente propiedad RDP para configurar el redireccionamiento del dispositivo Plug and Play:

- `devicestoredirect:s:*` habilita el redireccionamiento de todos los dispositivos Plug and Play.
- `devicestoredirect:s:` deshabilita el redireccionamiento de los dispositivos Plug and Play.

También puede seleccionar dispositivos Plug and Play específicos mediante una lista delimitada por puntos y comas, como `devicestoredirect:s:root\*PNP0F08`.

### <a name="local-drive-redirection"></a>Redireccionamiento de unidades locales

Establezca la siguiente propiedad RDP para configurar el redireccionamiento de la unidad local:

- `drivestoredirect:s:*` habilita el redireccionamiento de todas las unidades de disco.
- `drivestoredirect:s:` deshabilita el redireccionamiento de la unidad local.

También puede seleccionar unidades específicas mediante una lista delimitada por puntos y comas, como `drivestoredirect:s:C:;E:;`.

### <a name="printer-redirection"></a>Redirección de impresoras

Establezca la siguiente propiedad RDP para configurar el redireccionamiento de la impresora:

- `redirectprinters:i:1` habilita el redireccionamiento de la impresora.
- `redirectprinters:i:0` deshabilita el redireccionamiento de la impresora.

### <a name="smart-card-redirection"></a>Redireccionamiento de tarjetas inteligentes

Establezca la siguiente propiedad RDP para configurar el redireccionamiento de la tarjeta inteligente:

- `redirectsmartcards:i:1` habilita el redireccionamiento de tarjetas inteligentes.
- `redirectsmartcards:i:0` deshabilita el redireccionamiento de tarjetas inteligentes.
