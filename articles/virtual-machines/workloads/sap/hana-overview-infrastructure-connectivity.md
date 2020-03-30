---
title: Infraestructura y conectividad con SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Configure la infraestructura de conectividad necesaria para usar SAP HANA en Azure (instancias grandes).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/12/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cea89087742f1987f693b8bfb627bd71038a0c14
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77616918"
---
# <a name="sap-hana-large-instances-deployment"></a>Implementación de SAP HANA (instancias grandes) 

En este artículo se supone que ha completado la compra de SAP HANA en Azure (instancias grandes) de Microsoft. Antes de leer este artículo y para obtener información general, consulte los [términos comunes de instancias grandes de HANA](hana-know-terms.md) y las [SKU de instancias grandes de HANA](hana-available-skus.md).


Microsoft requiere la siguiente información para implementar unidades de instancia grande de HANA:

- Nombre del cliente.
- Información del contacto de la empresa (incluida la dirección de correo electrónico y el número de teléfono).
- Información del contacto técnico (incluida la dirección de correo electrónico y el número de teléfono).
- Información del contacto técnico de red (incluida la dirección de correo electrónico y el número de teléfono).
- Región de implementación de Azure (por ejemplo, Oeste de EE. UU., Este de Australia o Norte de Europa).
- SKU de SAP HANA en Azure (instancias grandes) (configuración).
- Para cada región de implementación de Azure:
    - Un intervalo de direcciones IP /29 para conexiones ER-P2P para conectar redes virtuales de Azure con HANA (instancias grandes).
    - Un bloque CIDR /24 usado para el grupo de direcciones IP de servidor de HANA (instancias grandes).
    - Opcionalmente, cuando se usa [ExpressRoute Global Reach](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach) para habilitar el enrutamiento directo desde un entorno local a las unidades de HANA (instancias grandes), o bien el enrutamiento entre unidades de HANA (instancias grandes) en diferentes regiones de Azure, es preciso reservar otro intervalo de direcciones IP /29. Este intervalo concreto no se puede superponer con ninguno de los otros intervalos de direcciones IP definidos antes.
- Los valores del intervalo de direcciones IP usados en el atributo de espacio de direcciones de red virtual de cada red virtual de Azure que se conecta a HANA (instancias grandes).
- Datos para cada sistema de HANA (instancias grandes):
  - Nombre de host deseado, preferiblemente el nombre de dominio completo.
  - Dirección IP deseada para la unidad de instancia grande de HANA fuera del intervalo de direcciones del grupo de IP del servidor. (Las primeras 30 direcciones IP del intervalo de direcciones del grupo de IP del servidor están reservadas para el uso interno de HANA [instancias grandes]).
  - Nombre del SID de SAP HANA para la instancia de SAP HANA (necesario para crear los volúmenes de disco necesarios relacionados con SAP HANA). Microsoft necesita el SID de HANA para crear los permisos para sidadm en los volúmenes NFS. Estos volúmenes se conectan a la unidad de instancia grande de HANA. El SID de HANA también se usa como uno de los componentes del nombre de los volúmenes de disco que se montan. Si quiere ejecutar más de una instancia de HANA en la unidad, debería enumerar varios SID de HANA. Cada uno obtiene un conjunto independiente de volúmenes asignados.
  - En el sistema operativo Linux, el usuario sidadm tiene un identificador de grupo. Este identificador es necesario para crear los volúmenes de disco necesarios relacionados con SAP HANA. La instalación de SAP HANA normalmente crea el grupo sapsys con un identificador de grupo de 1001. El usuario sidadm forma parte de ese grupo.
  - En el sistema operativo Linux, el usuario sidadm tiene un identificador de usuario. Este identificador es necesario para crear los volúmenes de disco necesarios relacionados con SAP HANA. Si está ejecutando varias instancias HANA en la unidad, se enumeran todos los usuarios sidadm. 
- Identificador de la suscripción a Azure a la que SAP HANA en Azure (instancias grandes) estará conectado directamente. Este identificador de suscripción hace referencia a la suscripción a Azure, que se va a cargar con la unidad o unidades de HANA (instancias grandes).

Después de proporcionar la información anterior, Microsoft aprovisiona SAP HANA en Azure (instancias grandes). Microsoft le envía información para vincular sus redes virtuales de Azure a HANA (instancias grandes). También puede acceder a las unidades de HANA (instancias grandes).

Use la secuencia siguiente para conectarse a HANA (instancias grandes) una vez que Microsoft lo haya implementado:

1. [Conexión de las máquinas virtuales de Azure a HANA (instancias grandes)](hana-connect-azure-vm-large-instances.md)
2. [Conexión de una red virtual a ExpressRoute de HANA (instancias grandes)](hana-connect-vnet-express-route.md)
3. [Requisitos de red adicionales (opcional)](hana-additional-network-requirements.md)

