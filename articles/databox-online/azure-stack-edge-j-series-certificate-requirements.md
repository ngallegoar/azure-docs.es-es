---
title: Requisitos y solución de problemas de certificados en Azure Stack Edge | Microsoft Docs
description: Se describen los requisitos de los certificados y la solución de los errores de los certificados en el dispositivo de Azure Stack Edge.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 08/12/2020
ms.author: alkohli
ms.openlocfilehash: b24b745a53b632ce32cda37058363bf974d400b3
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/01/2020
ms.locfileid: "89268269"
---
# <a name="certificate-requirements"></a>Requisitos de certificados

En este artículo se describen los requisitos de certificados que se deben cumplirse para que los certificados se puedan instalar en el dispositivo de Azure Stack Edge. Los requisitos están relacionados con los certificados PFX, la entidad emisora, el nombre del firmante y el nombre alternativo del firmante del certificado, y los algoritmos de certificado admitidos.

## <a name="certificate-issuing-authority"></a>Entidad emisora de certificados

Los requisitos de emisión de certificados son los siguientes:

* Los certificados deben emitirse desde una entidad de certificación interna o pública.

* No se admite el uso de certificados autofirmados.

* El campo *Emitido para:* del certificado no debe ser el mismo que su campo *Emitido por:* , excepto para los certificados de CA raíz.



## <a name="certificate-algorithms"></a>Algoritmos de certificados

Los algoritmos de certificados deben tener los siguientes requisitos:

* Los certificados no deben usar el algoritmo de claves RSA.

* El algoritmo de firma de certificados no puede ser SHA1.

* El tamaño de clave mínimo es de 4096.

## <a name="certificate-subject-name-and-subject-alternative-name"></a>Nombre del firmante y nombre alternativo del firmante del certificado

Los certificados deben tener los siguientes requisitos de nombre del firmante y nombre alternativo del firmante:

* Puede usar un único certificado que abarque todos los espacios de nombres en los campos de nombre alternativo del firmante (SAN) del certificado. O bien, como alternativa, puede usar certificados individuales para cada uno de los espacios de nombres. Para ambos enfoques hay que usar caracteres comodín para los puntos de conexión donde sean necesarios, como un objeto binario grande (blob).

* Asegúrese de que los nombres de los firmantes (el nombre común en el nombre del firmante) formen parte de los nombres alternativos de los firmantes en la extensión de nombre alternativo del firmante.

* Puede usar un único certificado comodín que abarque todos los espacios de nombres en el campo de SAN del certificado.

* Al crear un certificado de punto de conexión, use la tabla siguiente:

    |Tipo |Nombre del firmante (SN)  |Nombre alternativo del firmante (SAN)  |Ejemplo de nombre del firmante |
    |---------|---------|---------|---------|
    |Azure Resource Manager|`management.<Device name>.<Dns Domain>`|`login.<Device name>.<Dns Domain>`<br>`management.<Device name>.<Dns Domain>`|`management.mydevice1.microsoftdatabox.com` |
    |Blob Storage|`*.blob.<Device name>.<Dns Domain>`|`*.blob.< Device name>.<Dns Domain>`|`*.blob.mydevice1.microsoftdatabox.com` |
    |Interfaz de usuario local| `<Device name>.<DnsDomain>`|`<Device name>.<DnsDomain>`| `mydevice1.microsoftdatabox.com` |
    |Certificado único de varios SAN para ambos puntos de conexión|`<Device name>.<dnsdomain>`|`<Device name>.<dnsdomain>`<br>`login.<Device name>.<Dns Domain>`<br>`management.<Device name>.<Dns Domain>`<br>`*.blob.<Device name>.<Dns Domain>`|`mydevice1.microsoftdatabox.com` |
    |Nodo|`<NodeSerialNo>.<DnsDomain>`|`*.<DnsDomain>`<br><br>`<NodeSerialNo>.<DnsDomain>`|`mydevice1.microsoftdatabox.com` |
    |VPN|`AzureStackEdgeVPNCertificate.<DnsDomain>`<br><br> * AzureStackEdgeVPNCertificate está codificado de forma rígida.  | `*.<DnsDomain>`<br><br>`<AzureStackVPN>.<DnsDomain>` | `edgevpncertificate.microsoftdatabox.com`|
    
## <a name="pfx-certificate"></a>Certificado PFX

Los certificados PFX instalados en el dispositivo de Azure Stack Edge deben cumplir los siguientes requisitos:

* Cuando obtenga los certificados de la entidad de certificación SSL, asegúrese de obtener la cadena de firma completa para los certificados.

* Cuando exporte un certificado PFX, asegúrese de haber seleccionado la opción **Include all certificates in the chain, if possible** (Incluir todos los certificados en la cadena, si es posible).

* Use un certificado PFX para el punto de conexión, la interfaz de usuario local, el nodo, la VPN y Wi-Fi, ya que se necesitan claves tanto públicas como privadas para Azure Stack Edge. La clave privada debe tener establecido el atributo de clave de la máquina local.

* El cifrado PFX del certificado debe ser 3DES. Este es el cifrado predeterminado que se usa al exportar desde un cliente de Windows 10 o desde un almacén de certificados de Windows Server 2016. Para obtener más información relacionada con 3DES, consulte [Triple DES](https://en.wikipedia.org/wiki/Triple_DES).

* Los archivos PFX de certificado deben tener valores válidos de *DigitalSignature* y *KeyEncipherment* en el campo *Uso de claves*.

* Los archivos PFX de certificado deben tener los valores *Autenticación de servidor (1.3.6.1.5.5.7.3.1)* y *Autenticación de cliente (1.3.6.1.5.5.7.3.2)* en el campo *Uso mejorado de claves*.

* Las contraseñas para todos los archivos PFX de certificado deben ser las mismas en el momento de la implementación si usa la herramienta Azure Stack Readiness Checker. Para obtener más información, consulte [Creación de certificados para Azure Stack Edge con la herramienta Azure Stack Hub Readiness Checker](azure-stack-edge-j-series-create-certificates-tool.md).

* La contraseña para el archivo PFX de certificado tiene que ser una contraseña compleja. Tome nota de esta contraseña, ya que se usa como parámetro de implementación.

Para obtener más información, consulte [Exportación de certificados PFX con una clave privada](azure-stack-edge-j-series-manage-certificates.md#export-certificates-as-pfx-format-with-private-key).

## <a name="next-steps"></a>Pasos siguientes

[Uso de certificados con Azure Stack Edge](azure-stack-edge-j-series-manage-certificates.md)

[Creación de certificados para Azure Stack Edge con la herramienta Azure Stack Hub Readiness Checker](azure-stack-edge-j-series-create-certificates-tool.md)

[Exportación de certificados PFX con una clave privada](azure-stack-edge-j-series-manage-certificates.md#export-certificates-as-pfx-format-with-private-key)

[Solución de problemas de certificados](azure-stack-edge-j-series-certificate-troubleshooting.md)