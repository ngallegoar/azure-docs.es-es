---
title: 'DNS de Avere vFXT: Azure'
description: Configuración de un servidor DNS para el equilibrio de carga round robin con Avere vFXT for Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 12/19/2019
ms.author: rohogue
ms.openlocfilehash: 81b53904f85e2ac936195b1e39d7586fd1d47524
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96009031"
---
# <a name="avere-cluster-dns-configuration"></a>Configuración de DNS del clúster de Avere

En esta sección se explican los conceptos básicos de la configuración de un sistema DNS para el equilibrio de carga de su clúster de Avere vFXT.

Este documento *no incluye* instrucciones para configurar y administrar un servidor DNS en el entorno de Azure.

En lugar de usar DNS round robin para el equilibrio de carga de un clúster de vFXT en Azure, considere la posibilidad de usar métodos manuales para asignar direcciones IP de manera uniforme entre los clientes cuando se monten. Se describen varios métodos en el tema sobre el [montaje del clúster de Avere](avere-vfxt-mount-clients.md).

Tenga esto en mente la hora de decidir si quiere usar un servidor DNS:

* Si solo acceden a su sistema los clientes de NFS, no es necesario utilizar DNS (es posible especificar todas las direcciones de red mediante direcciones IP numéricas).

* Si el sistema admite el acceso de SMB (CIFS), DNS es necesario, ya que debe especificar un dominio DNS para el servidor de Active Directory.

* DNS es necesario si quiere utilizar la autenticación Kerberos.

## <a name="load-balancing"></a>Equilibrio de carga

Para distribuir la carga global, configure el dominio de DNS para usar la distribución de carga round robin para las direcciones IP orientadas al cliente.

## <a name="configuration-details"></a>Detalles de configuración

Cuando los clientes acceden al clúster, RRDNS equilibra automáticamente sus solicitudes entre todas las interfaces disponibles.

Para obtener un rendimiento óptimo, configure el servidor DNS para controlar las direcciones de clúster orientadas al cliente, como se muestra en el diagrama siguiente.

Un vserver de clúster se muestra a la izquierda y las direcciones IP aparecen en el centro y a la derecha. Configure todos los puntos de acceso de cliente con registros A y punteros como se muestra en la ilustración.

![Diagrama de DNS round robin del clúster de Avere](media/avere-vfxt-rrdns-diagram.png)
<!--- separate text description file provided  [diagram text description](avere-vfxt-rrdns-alt-text.md) -->

Cada dirección IP orientada al cliente debe tener un nombre único para que el clúster la use internamente. (En este diagrama, las direcciones IP del cliente se denominan vs1-client-IP-* por motivos de claridad, pero en producción debería usar probablemente algo más conciso, como client*.)

Los clientes montan el clúster con el nombre de vserver como argumento del servidor.

Modifique el archivo ``named.conf`` del servidor DNS para establecer un orden cíclico para las consultas en su vserver. Esta opción garantiza que todos los valores disponibles se recorran de forma cíclica. Agregue una instrucción como la siguiente:

```
options {
    rrset-order {
        class IN A name "vserver1.example.com" order cyclic;
    };
};
```

Los siguientes comandos ``nsupdate`` proporcionan un ejemplo de configuración correcta de DNS:

```
update add vserver1.example.com. 86400 A 10.0.0.10
update add vserver1.example.com. 86400 A 10.0.0.11
update add vserver1.example.com. 86400 A 10.0.0.12
update add vs1-client-IP-10.example.com. 86400 A 10.0.0.10
update add vs1-client-IP-11.example.com. 86400 A 10.0.0.11
update add vs1-client-IP-12.example.com. 86400 A 10.0.0.12
update add 10.0.0.10.in-addr.arpa. 86400 PTR vs1-client-IP-10.example.com
update add 11.0.0.10.in-addr.arpa. 86400 PTR vs1-client-IP-11.example.com
update add 12.0.0.10.in-addr.arpa. 86400 PTR vs1-client-IP-12.example.com
```

## <a name="cluster-dns-settings"></a>Configuración de DNS del clúster

Especifique el servidor DNS que usa el clúster de vFXT en la página de configuración **Clúster** > **Red administrativa**. La configuración de esta página incluye:

* Dirección del servidor DNS
* Nombre de dominio DNS
* Dominios de búsqueda DNS

Lea [DNS Settings](<https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_admin_network.html#gui-dns>) (Configuración de DNS) en la guía de configuración del clúster de Avere para obtener más detalles sobre el uso de esta página.
