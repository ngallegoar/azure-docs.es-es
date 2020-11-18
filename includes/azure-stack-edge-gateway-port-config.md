---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 12/16/2019
ms.author: alkohli
ms.openlocfilehash: 417fbdea3f46dfb3e90ab4890cec5e88c5aa4e07
ms.sourcegitcommit: 4bee52a3601b226cfc4e6eac71c1cb3b4b0eafe2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94523819"
---
| N.º de puerto| Dentro o fuera | Ámbito de puerto| Obligatorio | Notas |
|---------|-----------|-----------|----------|-------|
| TCP 80 (HTTP)|Fuera|WAN |No|El puerto de salida se usa para obtener acceso a Internet para así recuperar las actualizaciones. <br>El usuario puede configurar el proxy web de salida. |
| TCP 443 (HTTPS)|Fuera|WAN|Sí|El puerto de salida se usa para tener acceso a los datos en la nube.<br>El usuario puede configurar el proxy web de salida.|
| UDP 123 (NTP)|Fuera|WAN|En algunos casos.<br>Vea las notas|Este puerto es necesario solo si está utilizando un servidor NTP basado en Internet.  |   
| UDP 53 (DNS)|Fuera|WAN|En algunos casos.<br>Vea las notas|Este puerto es necesario solo si está utilizando un servidor DNS basado en Internet.<br>Se recomienda usar un servidor DNS local. |
| TCP 5985 (WinRM)|Salida/Entrada|LAN|En algunos casos.<br>Vea las notas|Este puerto es necesario para conectarse al dispositivo a través de la instancia remota de PowerShell a través de HTTP.  |
| UDP 67 (DHCP)|Fuera|LAN|En algunos casos.<br>Vea las notas|Este puerto solo es necesario si se usa un servidor DHCP local.  |
| TCP 80 (HTTP)|Salida/Entrada|LAN|Sí|Este es el puerto de entrada de la interfaz de usuario local del dispositivo para la administración local. <br>Si obtiene acceso a la interfaz de usuario local mediante HTTP, se le redireccionará automáticamente a HTTPS.  |
| TCP 443 (HTTPS)|Salida/Entrada|LAN|Sí|Este es el puerto de entrada de la interfaz de usuario local del dispositivo para la administración local. Este puerto también se usa para conectar Azure Resource Manager a las API locales del dispositivo, para conectar el almacenamiento de blobs a través de las API REST y al servicio de token de seguridad (STS) para autenticar a través de tokens de acceso y actualización.|
| TCP 445 (SMB)|En|LAN|En algunos casos.<br>Vea las notas|Este puerto solo es necesario si se va a conectar mediante SMB. |
| TCP 2049 (NFS)|En|LAN|En algunos casos.<br>Vea las notas|Este puerto solo es necesario si se va a conectar mediante NFS. |


