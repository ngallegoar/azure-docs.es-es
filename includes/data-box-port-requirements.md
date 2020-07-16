---
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: include
ms.date: 07/11/2019
ms.author: alkohli
ms.openlocfilehash: 948f5399ceb931d701341c2cf378482ede94979d
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86200297"
---
| N.º de puerto| Dentro o fuera | Ámbito de puerto| Obligatorio| Notas |
|--------|-----|-----|-----------|----------|
| TCP 80 (HTTP)|En|LAN|Sí|Este puerto se usa para conectarse a las API REST de Blog Storage de Data Box a través de HTTP. Si no se conecta a las API REST, se redirige automáticamente a la interfaz de usuario web local a través de 8443. |
| TCP 443 (HTTPS)|En|LAN|Sí|Este puerto se usa para conectarse a las API REST de Blog Storage de Data Box a través de HTTPS. Si no se conecta a las API REST, se redirige automáticamente a la interfaz de usuario web local a través de 8443. |
| TCP 8443 (HTTPS-Alt)|En|LAN|Sí|Se trata de un puerto alternativo para HTTPS y se usa al conectarse a la interfaz de usuario web local para la administración de dispositivos. |
| TCP 445 (SMB)|Salida/Entrada|LAN|En algunos casos.<br>Vea las notas|Este puerto solo es necesario si se va a conectar mediante SMB. |
| TCP 2049 (NFS)|Salida/Entrada|LAN|En algunos casos.<br>Vea las notas|Este puerto solo es necesario si se va a conectar mediante NFS. |
| TCP 111 (NFS)|Salida/Entrada|LAN|En algunos casos.<br>Vea las notas|Este puerto se usa para la asignación de rpcbind/puerto y solo es necesario si se está conectando a través de NFS.  |
