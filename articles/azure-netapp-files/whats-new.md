---
title: Novedades de Azure NetApp Files | Microsoft Docs
description: Proporciona un resumen de las nuevas características y mejoras más recientes de Azure NetApp Files.
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
ms.topic: overview
ms.date: 09/30/2020
ms.author: b-juche
ms.openlocfilehash: beb1cc9957f7b43bc6ad9b9b6ee6c7707eb0c2d6
ms.sourcegitcommit: f796e1b7b46eb9a9b5c104348a673ad41422ea97
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2020
ms.locfileid: "91571556"
---
# <a name="whats-new-in-azure-netapp-files"></a>Novedades de Azure NetApp Files

Azure NetApp Files se actualiza periódicamente. En este artículo se proporciona un resumen de las nuevas características y mejoras más recientes. 

## <a name="september-2020"></a>Septiembre de 2020

* [Replicación de Azure NetApp Files entre regiones](cross-region-replication-introduction.md) (versión preliminar pública)

  Azure NetApp Files ahora admite la replicación entre regiones. Con esta nueva funcionalidad de recuperación ante desastres, puede replicar los volúmenes de Azure NetApp Files de una región de Azure a otra de manera rápida y rentable, y así proteger los datos frente a errores regionales imprevistos. La replicación entre regiones de Azure NetApp Files aprovecha la tecnología SnapMirror® de NetApp; solo los bloques modificados se envían a través de la red en un formato comprimido y eficiente. Esta tecnología propietaria reduce la cantidad de datos necesarios para replicar entre regiones, lo que ahorra costos de transferencia de datos. También acorta el tiempo de replicación, por lo que puede lograr un objetivo de punto de restauración (RPO) más pequeño.

* [Grupo de capacidad de QoS manual](manage-manual-qos-capacity-pool.md) (versión preliminar)  

    En un grupo de capacidad de QoS manual, puede asignar la capacidad y el rendimiento de un volumen de forma independiente. El rendimiento total de todos los volúmenes creados con un grupo de capacidad de QoS manual está limitado por el rendimiento total del grupo. Viene determinado por la combinación del tamaño del grupo y el rendimiento del nivel de servicio. También, el [tipo de QoS](azure-netapp-files-understand-storage-hierarchy.md#qos_types) de un grupo de capacidad puede ser automático (auto), que es el valor predeterminado. En un grupo de capacidad de QoS automático, el rendimiento se asigna automáticamente a los volúmenes del grupo, de forma proporcional a la cuota de tamaño asignada a los volúmenes.

* [Firma LDAP](azure-netapp-files-create-volumes-smb.md) (versión preliminar)   

    Azure NetApp Files admite ahora la firma LDAP para proteger las búsquedas de LDAP entre el servicio Azure NetApp Files y los controladores de dominio de Active Directory Domain Services especificados por el usuario. Esta funcionalidad actualmente está en su versión preliminar.

* [Cifrado AES para la autenticación de AD](azure-netapp-files-create-volumes-smb.md) (versión preliminar)

    Azure NetApp Files admite ahora el cifrado AES en la conexión LDAP con el controlador de dominio para permitir este tipo de cifrado en un volumen SMB. Esta funcionalidad actualmente está en su versión preliminar. 

* Nuevas [métricas](azure-netapp-files-metrics.md):   

    * Nuevas métricas de volumen: 
        * *Tamaño asignado del volumen*: Tamaño aprovisionado de un volumen
    * Nuevas métricas de grupo: 
        * *Tamaño asignado del grupo*: el tamaño aprovisionado del grupo. 
        * *Tamaño de instantánea total del grupo*: la suma del tamaño de la instantánea de todos los volúmenes del grupo.

## <a name="july-2020"></a>Julio de 2020

* [Volumen de protocolo doble (NFSv3 y SMB)](create-volumes-dual-protocol.md)

    Ahora puede crear un volumen de Azure NetApp Files que permite el acceso simultáneo con protocolo doble (NFS V3 y SMB) con compatibilidad con la asignación de usuarios LDAP. Esta característica permite casos de uso en los que puede tener una carga de trabajo basada en Linux que genera y almacena los datos en un volumen de Azure NetApp Files. Al mismo tiempo, el personal debe usar clientes y software basados en Windows para analizar los datos recién generados desde el mismo volumen de Azure NetApp Files. La característica de acceso simultáneo con protocolo doble elimina la necesidad de copiar los datos generados por la carga de trabajo en un volumen independiente con un protocolo diferente para el análisis posterior, lo que supone un ahorro en los costes y en el tiempo de funcionamiento. Esta característica es gratuita (se sigue aplicando el [costo de almacenamiento de Azure NetApp Files](https://azure.microsoft.com/pricing/details/netapp/) normal) y está disponible con carácter general. Más información en la [documentación de acceso simultáneo con protocolo doble](create-volumes-dual-protocol.MD).

* [Cifrado Kerberos de NFS v 4.1 en tránsito](configure-kerberos-encryption.MD)

    Azure NetApp Files admite ahora el cifrado de cliente NFS en los modos de Kerberos (krb5, krb5i y krb5p) con el cifrado AES-256, lo que le proporciona seguridad adicional para los datos. Esta característica es gratuita (se sigue aplicando el [costo de almacenamiento de Azure NetApp Files](https://azure.microsoft.com/pricing/details/netapp/) normal) y está disponible con carácter general. Más información en la [documentación de cifrado Kerberos de NFS v4.1](configure-kerberos-encryption.MD).

* [Cambio de nivel de servicio de volumen dinámico](dynamic-change-volume-service-level.MD)

    Flexibilidad de promesas de la nube en los gastos de TI. Ahora puede cambiar el nivel de servicio de un volumen existente de Azure NetApp Files si lo mueve a otro grupo de capacidad que use el nivel de servicio que quiera para el volumen. Este cambio de nivel de servicio local para el volumen no requiere que se migren los datos. Tampoco afecta al acceso del plano de datos al volumen. Puede cambiar un volumen existente para que use un nivel de servicio superior para mejorar el rendimiento, o para que use un nivel de servicio inferior para la optimización de costos. Esta característica es gratuita (se sigue aplicando el [costo de almacenamiento de Azure NetApp Files](https://azure.microsoft.com/pricing/details/netapp/) normal) y se encuentra actualmente en versión preliminar pública. Para registrarse en la versión preliminar de la característica, siga la [documentación de cambio de nivel de servicio de volúmenes dinámicos](dynamic-change-volume-service-level.md).

* [Directiva de instantáneas de volumen](azure-netapp-files-manage-snapshots.md#manage-snapshot-policies) (versión preliminar) 

    Azure NetApp Files le permite crear instantáneas de un momento dado de los volúmenes. Ahora, puede crear una directiva de instantáneas para que Azure NetApp Files cree automáticamente instantáneas de volumen con una frecuencia de su elección. Puede programar que se tomen instantáneas en ciclos por hora, día, semana o mes. También puede especificar el número máximo de instantáneas que se deben conservar como parte de la directiva de instantáneas. Esta característica es gratuita (se sigue aplicando el [costo de almacenamiento de Azure NetApp Files](https://azure.microsoft.com/pricing/details/netapp/) normal) y se encuentra actualmente en versión preliminar. Para registrarse en la versión preliminar de la característica, siga la [documentación de la directiva de instantáneas de volumen](azure-netapp-files-manage-snapshots.md#manage-snapshot-policies).

* [Directiva de exportación de acceso raíz de NFS](azure-netapp-files-configure-export-policy.md)

    Azure NetApp Files ahora le permite especificar si la cuenta raíz puede acceder al volumen. 

* [Ocultar la ruta de acceso de la instantánea](azure-netapp-files-manage-snapshots.md#restore-a-file-from-a-snapshot-using-a-client)

    Azure NetApp Files le permite ahora especificar si un usuario puede ver el directorio `.snapshot` (clientes NFS) o la carpeta `~snapshot` (clientes SMB) en un volumen montado, y acceder a estos.

## <a name="may-2020"></a>Mayo de 2020

* [Usuarios de la directiva de copia de seguridad](azure-netapp-files-create-volumes-smb.md#create-an-active-directory-connection) (versión preliminar)

    Azure NetApp Files le permite incluir cuentas adicionales que requieran privilegios elevados para la cuenta de equipo creada para Azure NetApp Files. Se permitirá a las cuentas especificadas cambiar los permisos de NTFS en el nivel de archivo o carpeta. Por ejemplo, puede especificar una cuenta de servicio sin privilegios que se usa para migrar los datos a un recurso compartido de archivos de SMB en Azure NetApp Files. La característica Usuarios de la directiva de copia de seguridad se encuentra actualmente en la versión preliminar.

## <a name="next-steps"></a>Pasos siguientes
* [¿Qué es Azure NetApp Files?](azure-netapp-files-introduction.md)
* [Información sobre la jerarquía del almacenamiento de Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md) 
