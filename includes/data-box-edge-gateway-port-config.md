---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/25/2019
ms.author: alkohli
ms.openlocfilehash: 71460af42b4da97a578ae5a3e23a714577e71867
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86218121"
---
| N.º de puerto| Dentro o fuera | Ámbito de puerto| Obligatorio|   Notas |
|--------|-----|-----|-----------|----------|
| TCP 80 (HTTP)|Fuera|WAN |No|El puerto de salida se usa para obtener acceso a Internet para así recuperar las actualizaciones. <br>El usuario puede configurar el proxy web de salida. |
| TCP 443 (HTTPS)|Fuera|WAN|Sí|El puerto de salida se usa para tener acceso a los datos en la nube.<br>El usuario puede configurar el proxy web de salida.|
| UDP 123 (NTP)|Fuera|WAN|En algunos casos.<br>Vea las notas|Este puerto es necesario solo si está utilizando un servidor NTP basado en Internet.  |   
| UDP 53 (DNS)|Fuera|WAN|En algunos casos.<br>Vea las notas|Este puerto es necesario solo si está utilizando un servidor DNS basado en Internet.<br>Se recomienda usar un servidor DNS local. |
| TCP 5985 (WinRM)|Salida/Entrada|LAN|En algunos casos.<br>Vea las notas|Este puerto es necesario para conectarse al dispositivo a través de la instancia remota de PowerShell a través de HTTP.  |
| UDP 67 (DHCP)|Fuera|LAN|En algunos casos.<br>Vea las notas|Este puerto solo es necesario si se usa un servidor DHCP local.  |
| TCP 80 (HTTP)|Salida/Entrada|LAN|Sí|Este es el puerto de entrada de la interfaz de usuario local del dispositivo para la administración local. <br>Si obtiene acceso a la interfaz de usuario local mediante HTTP, se le redireccionará automáticamente a HTTPS.  |
| TCP 443 (HTTPS)|Salida/Entrada|LAN|Sí|Este es el puerto de entrada de la interfaz de usuario local del dispositivo para la administración local. |
| TCP 445 (SMB)|En|LAN|En algunos casos.<br>Vea las notas|Este puerto solo es necesario si se va a conectar mediante SMB. |
| TCP 2049 (NFS)|En|LAN|En algunos casos.<br>Vea las notas|Este puerto solo es necesario si se va a conectar mediante NFS. |
