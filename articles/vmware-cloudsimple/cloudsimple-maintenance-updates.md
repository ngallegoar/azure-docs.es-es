---
title: Mantenimiento y actualizaciones de CloudSimple
titleSuffix: Azure VMware Solution by CloudSimple
description: Describe el proceso de servicio de CloudSimple para las operaciones de mantenimiento y las actualizaciones programadas
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 826fae1123b355a4143118b53ba649f0939acaf7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77025034"
---
# <a name="cloudsimple-maintenance-and-updates"></a>Mantenimiento y actualizaciones de CloudSimple

El entorno de nube privada está diseñado para no tener un único punto de error.

* Los clústeres de ESXi están configurados con la alta disponibilidad de vSphere. Los clústeres tienen un tamaño para tener al menos un nodo de repuesto para proporcionar resistencia.
* vSAN, que requiere al menos tres nodos para proteger frente a un único error, proporciona almacenamiento principal redundante. vSAN puede configurarse para proporcionar mayor resistencia para clústeres más grandes.
* Las VM de vCenter, PSC y NSX Manager se configuran con la directiva de almacenamiento de RAID-10 para protegerse frente a errores de almacenamiento. La alta disponibilidad de vSphere protege las máquinas virtuales frente a errores de nodo o red.
* Los hosts de ESXi tienen ventiladores y NIC redundantes.
* Los conmutadores TOR y de tallo se configuran en pares de alta disponibilidad para proporcionar resistencia.

CloudSimple supervisa continuamente la actividad y la disponibilidad de las máquinas virtuales siguientes y proporciona los SLA de disponibilidad:

* Hosts de ESXi
* vCenter
* PSC
* NSX Manager

CloudSimple también supervisa continuamente si hay errores en los elementos siguientes:

* Discos duros
* Puertos de NIC física
* Servidores
* Ventiladores
* Power
* Conmutadores
* Puertos de conmutador

Si se produce un error en un disco o nodo, se agrega automáticamente un nodo nuevo al clúster de VMware afectado para que recupere su estado inmediatamente.

CloudSimple crea copias de seguridad de estos elementos de VMware en las nubes privadas, los mantiene y los actualiza:

* ESXi
* Servicios de plataforma de vCenter
* Controller
* vSAN
* NSX

## <a name="back-up-and-restore"></a>Copia de seguridad y restauración

La copia de seguridad de CloudSimple incluye:

* Copias de seguridad incrementales nocturnas de las reglas de vCenter, PSC y DVS.
* API nativas de vCenter para crear una copia de seguridad de los componentes en el nivel de aplicación.
* Copia de seguridad automática antes de cualquier actualización del software de administración de VMware.
* Cifrado de datos de vCenter en el origen antes de transferir datos a través de un canal cifrado TLS1.2 a Azure. Los datos se almacenan en un blob de Azure en que se replican entre regiones.

Puede abrir una [solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) para solicitar una restauración.

## <a name="maintenance"></a>Mantenimiento

CloudSimple realiza varios tipos de mantenimiento planeado.

### <a name="backendinternal-maintenance"></a>Mantenimiento de back-end o interno

Normalmente, este tipo de mantenimiento implica volver a configurar los recursos físicos o instalar revisiones de software. No afecta al consumo normal de los recursos sujetos a mantenimiento. Con NIC redundantes en cada bastidor físico, el tráfico de red normal y las operaciones de nube privada no se ven afectados. Es posible que observe un impacto en el rendimiento solo si su organización prevé utilizar el ancho de banda redundante completo durante el intervalo de mantenimiento.

### <a name="cloudsimple-portal-maintenance"></a>Mantenimiento del portal de CloudSimple

Al actualizar el plano de control o la infraestructura de CloudSimple, se requiere cierto tiempo de inactividad limitado. Actualmente, la frecuencia de los intervalos de mantenimiento puede ser mensual. Se espera que la frecuencia se reduzca con el tiempo. CloudSimple proporciona una notificación de mantenimiento del portal y acorta el intervalo tanto como sea posible. Durante un intervalo de mantenimiento del portal, los servicios siguientes seguirán funcionando sin ningún impacto:

* Las aplicaciones y el plano de administración de VMware
* El acceso a vCenter
* Todas las redes y el almacenamiento
* Todo el tráfico de Azure

### <a name="vmware-infrastructure-maintenance"></a>Mantenimiento de la infraestructura de VMware

En ocasiones, es necesario realizar cambios en la configuración de la infraestructura de VMware.  En la actualidad, estos intervalos se pueden producir cada 1 o 2 meses, pero se espera que la frecuencia se reduzca con el tiempo. Normalmente, este tipo de mantenimiento se puede realizar sin interrumpir el consumo normal de los servicios de CloudSimple. Durante un intervalo de mantenimiento de VMware, los servicios siguientes seguirán funcionando sin ningún impacto:

* Las aplicaciones y el plano de administración de VMware
* El acceso a vCenter
* Todas las redes y el almacenamiento
* Todo el tráfico de Azure

## <a name="updates-and-upgrades"></a>Actualizaciones

CloudSimple es responsable de la administración del ciclo de vida del software de VMware (ESXi, vCenter, PSC y NSX) en la nube privada.

Entre las actualizaciones de software se incluyen las siguientes:

* **Revisiones**. Revisiones de seguridad o correcciones de errores publicadas por VMware.
* **Actualizaciones**. Cambio de versión secundaria de un componente de la pila de VMware.
* **Actualizaciones**. Cambio de versión principal de un componente de la pila de VMware.

CloudSimple prueba una revisión de seguridad crítica en cuanto esté disponible desde VMware. Según el SLA, CloudSimple implementa la revisión de seguridad en los entornos de nube privada en el plazo de una semana.

CloudSimple proporciona actualizaciones de mantenimiento trimestrales para los componentes de software de VMware. Cuando hay una nueva versión principal disponible del software de VMware, CloudSimple trabaja con los clientes para coordinar una ventana de mantenimiento adecuada para la actualización.

## <a name="next-steps"></a>Pasos siguientes

[Copia de seguridad de VM de carga de trabajo con Veeam](backup-workloads-veeam.md).
