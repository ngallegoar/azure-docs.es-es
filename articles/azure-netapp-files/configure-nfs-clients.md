---
title: Configuración de un cliente NFS para Azure NetApp Files | Microsoft Docs
description: Describe cómo configurar los clientes NFS para usarlos con Azure NetApp Files.
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
ms.topic: how-to
ms.date: 09/28/2020
ms.author: b-juche
ms.openlocfilehash: b2e597ff8fc761b66de6228063c471933a364144
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91449659"
---
# <a name="configure-an-nfs-client-for-azure-netapp-files"></a>Configuración de un cliente NFS para Azure NetApp Files

La configuración de cliente NFS que se describe en este artículo forma parte de la configuración cuando [configura el cifrado Kerberos de NFSv 4.1](configure-kerberos-encryption.md) o [crea un volumen de dos protocolos](create-volumes-dual-protocol.md). Hay una amplia variedad de distribuciones de Linux disponibles para usarse con Azure NetApp Files. En este artículo se describen las configuraciones de dos de los entornos de uso más frecuentes: RHEL 8 y Ubuntu 18.04. 

Independientemente del tipo de Linux que use, se requieren las siguientes configuraciones:
* Configuración de un cliente NTP para evitar problemas con el sesgo horario.
* Configuración de las entradas DNS del cliente Linux para la resolución de nombres.  
    Esta configuración incluye el registro "A" (adelante) y el registro PTR (inverso). 
* Para unirse a un dominio, cree una cuenta de equipo en la instancia de Active Directory de destino (que se crea durante el comando de unión a dominio de Kerberos). 
    > [!NOTE] 
    > La variable `$SERVICEACCOUNT` utilizada en los siguientes comandos debe ser una cuenta de usuario con permisos o delegación para crear una cuenta de equipo en la unidad organizativa de destino.
* Habilite el cliente para montar volúmenes NFS y otras herramientas de supervisión pertinentes.

## <a name="rhel-8-configuration"></a>Configuración de RHEL 8 

1. Instale los paquetes:   
    `sudo yum -y install realmd sssd adcli samba-common krb5-workstation chrony`

2. Configure el servicio de cliente NTP:  
    RHEL 8 usa `chrony` de forma predeterminada.  Siga las instrucciones de configuración de [Uso del conjunto Chrony para configurar NTP](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_basic_system_settings/using-chrony-to-configure-ntp).

3. Una el dominio de Active Directory:  
    `sudo realm join $DOMAIN.NAME -U $SERVICEACCOUNT --computer-ou= OU=$YOUROU,DC=$DOMAIN,DC=TLD`

## <a name="ubuntu-configuration"></a>Configuración de Ubuntu 
En esta sección se describe la configuración de Ubuntu para los clientes NFS.  

### <a name="if-you-are-using-nfsv41-kerberos-encryption"></a>Si utiliza el cifrado Kerberos de NFSv4.1 

1. Instale los paquetes:  
    `sudo yum -y install realmd packagekit sssd adcli samba-common krb5-workstation chrony`

2. Configure el cliente NTP.  
    Ubuntu 18.04 usa `chrony` de forma predeterminada.  Siga las instrucciones de configuración de [Ubuntu Bionic: Uso de chrony para configurar NTP](https://ubuntu.com/blog/ubuntu-bionic-using-chrony-to-configure-ntp).

3. Una el dominio de Active Directory:  
    `sudo realm join $DOMAIN.NAME -U $SERVICEACCOUNT --computer-ou= OU=$YOUROU,DC=$DOMAIN,DC=TLD`

### <a name="if-you-are-using-dual-protocol"></a>Si utiliza un protocolo dual  

1. Ejecute el siguiente comando para actualizar los paquetes instalados:  
    `sudo apt update && sudo apt install libnss-ldap libpam-ldap ldap-utils nscd`

    Ejemplo:   

    `base dc=hariscus,dc=com` `uri ldap://10.20.0.4:389/`
    `ldap_version 3`
    `rootbinddn cn=admin,cn=Users,dc=hariscus,dc=com`
    `pam_password ad`
 
2. Ejecute el siguiente comando para reiniciar y habilitar el servicio:   
    `sudo systemctl restart nscd && sudo systemctl enable nscd`

En el ejemplo siguiente se realiza una consulta al servidor LDAP de AD desde el cliente LDAP de Ubuntu para un usuario de LDAP `ldapu1`:   

`root@cbs-k8s-varun4-04:/home/cbs# getent passwd hari1`   
`hari1:*:1237:1237:hari1:/home/hari1:/bin/bash`   

## <a name="next-steps"></a>Pasos siguientes  

* [Creación de un volumen de NFS para Azure NetApp Files](azure-netapp-files-create-volumes.md)
* [Creación de un volumen de dos protocolos para Azure NetApp Files](create-volumes-dual-protocol.md)

