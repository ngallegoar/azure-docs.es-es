---
title: Directiva de emparejamiento de Microsoft
titleSuffix: Azure
description: Directiva de emparejamiento de Microsoft
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: conceptual
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: a683ad71f5e80c91728262dc7bbabf36e9d68deb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "75773951"
---
# <a name="peering-policy"></a>Directiva de emparejamiento
En las secciones siguientes se explican los requisitos generales de Microsoft de la red. Se aplican a las solicitudes de emparejamiento directo y de emparejamiento de Exchange.

## <a name="technical-requirements"></a>Requisitos técnicos

* Una red totalmente redundante con capacidad suficiente para intercambiar tráfico sin congestión.
* El elemento del mismo nivel tendrá un número de sistema autónomo (ASN) enrutable públicamente.
* Tanto IPv4 como IPv6 se admiten y Microsoft espera establecer sesiones de ambos tipos en cada ubicación de emparejamiento.
* No se admite el uso de MD5.
* **Detalles de ASN:**
    * Microsoft administra AS8075 junto con los siguientes ASN: AS8068, AS8069, AS12076. Para obtener una lista completa de ASN con el emparejamiento de AS8075, consulte AS-SET MICROSOFT.
    * Todas las partes emparejadas con Microsoft acuerdan no aceptar rutas de AS12076 (ExpressRoute) bajo ninguna circunstancia y deben filtrar AS12076 en todos los elementos del mismo nivel.
* **Directiva de enrutamiento:**
    * El elemento del mismo nivel tendrá al menos un /24 enrutable públicamente.
    * Microsoft sobrescribirá los discriminadores de múltiples salidas (MED) recibidos.
    * Microsoft prefiere recibir etiquetas de comunidad de BGP de elementos del mismo nivel para indicar el origen de la ruta.
    * Se espera que el elemento del mismo nivel registre sus rutas en una base de datos de registro de enrutamiento de Internet (IRR) público con fines de filtrado, y realizará esfuerzos de buena fe para mantener esta información actualizada.
    * Se recomienda que los elementos del mismo nivel establezcan un prefijo máximo de 1000 (IPv4) y 100 (IPv6) rutas en las sesiones de emparejamiento con Microsoft.
    * A menos que se acuerde expresamente de antemano, se espera que los elementos del mismo nivel anuncien rutas coherentes en todas las ubicaciones en las que se emparejan con Microsoft.
    * En general, las sesiones de emparejamiento con AS8075 anunciarán todas las rutas AS-MICROSOFT. Las interconexiones de AS8075 en África y Asia pueden estar limitadas a las rutas pertinentes a la región correspondiente.
    * Ninguna de las partes establecerá una ruta estática o una ruta de último recurso ni enviará tráfico de ningún otro modo a la otra parte para una ruta no anunciada a través de BGP.
    * Se espera que los elementos del mismo nivel se adhieran a los estándares [MANRS](https://www.manrs.org/) del sector para la seguridad de las rutas.

## <a name="operational-requirements"></a>Requisitos operativos
* Un centro de operaciones de red (NOC) disponible de manera totalmente ininterrumpida, capaz de ayudar con la resolución de todos los problemas técnicos y de rendimiento, las infracciones de seguridad, los ataques por denegación de servicio o cualquier otro abuso que se origine en el elemento del mismo nivel o en sus clientes.
* Se espera que los elementos del mismo nivel tengan un perfil completo y actualizado en [PeeringDB](https://www.peeringdb.com), incluido un correo electrónico de NOC ininterrumpido del dominio corporativo y el número de teléfono. Usamos esta información para validar los detalles del elemento del mismo nivel, como su información de NOC, sus datos de contacto técnico, su presencia en las instalaciones de emparejamiento, etc. No se aceptan cuentas personales de Yahoo, Gmail y Hotmail.

## <a name="physical-connection-requirements"></a>Requisitos de la conexión física
* Las ubicaciones en las que puede conectarse con Microsoft para el emparejamiento directo o el emparejamiento de Exchange se muestran en [PeeringDB](https://www.peeringdb.com/net/694).
* **Emparejamiento de Exchange:**
    * La interconexión debe realizarse a través de fibra de modo único con la óptica de 10 Gbps adecuada.
    * Se espera que los elementos del mismo nivel actualicen sus puertos cuando el uso máximo supere el 50 %.
* **Emparejamiento directo:**
    * La interconexión debe realizarse a través de fibra de modo único con la óptica de 10 Gbps o 100 Gbps adecuada.
    * Microsoft solo establecerá el emparejamiento directo con proveedores ISP o de servicios de red.
    * Se espera que los elementos del mismo nivel actualicen sus puertos cuando el uso máximo supere el 50 % y mantengan una capacidad diversa en cada metro, ya sea en una sola ubicación o en varias ubicaciones de un metro.
    * Cada emparejamiento directo consta de dos conexiones a dos enrutadores perimetrales de Microsoft desde los enrutadores del mismo nivel ubicados en el borde del mismo nivel. Microsoft requiere sesiones de BGP duales en estas conexiones. El elemento del mismo nivel puede optar por no implementar dispositivos redundantes en su extremo.

## <a name="traffic-requirements"></a>Requisitos de tráfico
* Los elementos del mismo nivel a través del emparejamiento de Exchange deben tener como mínimo 200 Mb de tráfico y menos de 2 Gb.  Se debe revisar el tráfico que supere el emparejamiento directo de 2 Gb.
* En el emparejamiento directo, el tráfico de la red a Microsoft debe cumplir los requisitos mínimos siguientes.

    | Geoárea                      | Tráfico mínimo a Microsoft   |
    | :----------------------- |:-------------------------------|
    | África                   | 500 Mbps                       |
    | Asia Pacífico (excepto India)      |   2 Gbps                       |
    | Asia Pacífico (solo India)        | 500 Mbps                       |
    | Europa                   |   2 Gbps                       |
    | Latinoamérica                    |   2 Gbps                       |
    | Oriente Medio              | 500 Mbps                       |
    | N/D                       |   2 Gbps                       |

* **Diversidad:**
    * En Norteamérica, Europa, Asia Pacífico y Latinoamérica, realice la interconexión en un mínimo de tres ubicaciones geográficamente diversas si es factible y mantiene una capacidad diversa para permitir el tráfico a la conmutación por error en cada metro.
    * En África, Oriente Medio e India, realice la interconexión en tantas ubicaciones distintas como sea posible. Debe mantener una capacidad diversa suficiente para garantizar que el tráfico permanece en la región.

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre los pasos para configurar el emparejamiento directo con Microsoft, consulte el [Tutorial del emparejamiento directo](walkthrough-direct-all.md).
* Para más información sobre los pasos para configurar el emparejamiento de Exchange con Microsoft, consulte el [Tutorial del emparejamiento de Exchange](walkthrough-exchange-all.md).