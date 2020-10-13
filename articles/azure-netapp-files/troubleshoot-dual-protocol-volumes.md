---
title: Solución de problemas de volúmenes de protocolo dual para Azure NetApp Files | Microsoft Docs
description: Aquí se describen los mensajes de error y propuestas que pueden ayudar a solucionar los problemas de protocolo dual de Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/01/2020
ms.author: b-juche
ms.openlocfilehash: cc046a27fec1b9e361ff840c7ae0f077e2987b67
ms.sourcegitcommit: b4f303f59bb04e3bae0739761a0eb7e974745bb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2020
ms.locfileid: "91654087"
---
# <a name="troubleshoot-dual-protocol-volumes"></a>Solución de problemas de volúmenes de protocolo doble

En este artículo se describen las soluciones a las condiciones de error que pueden darse al crear o administrar volúmenes de protocolo dual.

## <a name="error-conditions-and-resolutions"></a>Condiciones de error y soluciones

|     Condiciones del error    |     Solución    |
|-|-|
| Se produce un error al crear el volumen de protocolo dual: `This Active Directory has no Server root CA Certificate`.    |     Si este error se produce al crear un volumen de protocolo dual, compruebe que el certificado de la entidad de certificación raíz se haya cargado en su cuenta de NetApp.    |
| Se produce un error al crear el volumen de protocolo dual: `Failed to validate LDAP configuration, try again after correcting LDAP configuration`.    |  Considere las siguientes soluciones:   <ul><li>Asegúrese de que se haya agregado el certificado raíz necesario al unir Active Directory (AD) a la cuenta de NetApp. Consulte [Carga del certificado raíz público de la entidad de certificación de Active Directory](create-volumes-dual-protocol.md#upload-active-directory-certificate-authority-public-root-certificate).   </li><li>Es posible que falte el registro de puntero (PTR) del equipo host de AD en el servidor DNS. Tiene que crear una zona de búsqueda inversa en el servidor DNS y luego agregar un registro de puntero (PTR) del equipo host de AD a esa zona de búsqueda inversa. <br> Por ejemplo, supongamos que la dirección IP de la máquina de AD es `1.1.1.1`, que el nombre de host de la máquina de AD (que se encuentra mediante el comando `hostname`) es `AD1` y el nombre de dominio es `myDomain.com`.  El registro PTR agregado a la zona de búsqueda inversa debe ser `1.1.1.1` -> `AD1.myDomain.com`. </li></ul>  |
| Se produce un error al crear el volumen de protocolo dual: `Failed to create the Active Directory machine account \\\"TESTAD-C8DD\\\". Reason: Kerberos Error: Pre-authentication information was invalid Details: Error: Machine account creation procedure failed\\n [ 434] Loaded the preliminary configuration.\\n [ 537] Successfully connected to ip 1.1.1.1, port 88 using TCP\\n**[ 950] FAILURE`. |  Este error indica que la contraseña de AD es incorrecta cuando Active Directory está unido a la cuenta de NetApp. Actualice la conexión de AD con la contraseña correcta e inténtelo de nuevo. |
| Se produce un error al crear el volumen de protocolo dual: `Could not query DNS server. Verify that the network configuration is correct and that DNS servers are available`. |   Este error indica que no se puede obtener acceso al servidor DNS. La razón puede ser que la dirección IP del servidor DNS sea incorrecta o que haya un problema de red. Compruebe la dirección IP de DNS especificada en la conexión de AD y asegúrese de que sea correcta. <br> Asegúrese también de que AD y el volumen estén en la misma región y en la misma red virtual. Si están en redes virtuales diferentes, asegúrese de establecer un emparejamiento entre las dos redes virtuales.|
| Error de denegación de permiso al montar un volumen de protocolo dual. | Un volumen de protocolo dual admite los protocolos NFS y SMB.  Al intentar obtener acceso al volumen montado en el sistema UNIX, el sistema intenta asignar el usuario de UNIX que utiliza a un usuario de Windows. Si no se encuentra ninguna asignación, se produce el error "Permiso denegado". <br> Esta situación se aplica también cuando se usa el usuario "raíz" para obtener acceso. <br> Para evitar el problema de tipo "Permiso denegado", asegúrese de que Active Directory para Windows incluya `pcuser` antes de obtener acceso al punto de montaje. Si agrega `pcuser` después de encontrar el problema "Permiso denegado", espere 24 horas para que la entrada de caché se borre antes de volver a intentar el acceso. |

## <a name="next-steps"></a>Pasos siguientes  

* [Creación de un volumen de protocolo dual](create-volumes-dual-protocol.md)
* [Configuración de un cliente NFS para Azure NetApp Files](configure-nfs-clients.md)
