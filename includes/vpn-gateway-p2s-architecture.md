---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/30/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e3ad8fcb6cd5cf68b02ac522d0e5ef5a18ba8a3c
ms.sourcegitcommit: 4b76c284eb3d2b81b103430371a10abb912a83f4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2020
ms.locfileid: "93145009"
---
Las conexiones con autenticación mediante certificado de Azure nativas de punto a sitio usan los siguientes elementos, que se configuran en este ejercicio:

* Una puerta de enlace de VPN RouteBased.
* La clave pública (archivo .cer) de un certificado raíz, que se carga en Azure. Una vez cargado el certificado, se considera un certificado de confianza y se usa para la autenticación.
* Un certificado de cliente que se genera a partir del certificado raíz. El certificado de cliente se instalará en todos los equipos cliente que se conecten a la red virtual. Este certificado se usa para la autenticación de cliente.
* Configuración de cliente de VPN. El cliente VPN se configura mediante los archivos de configuración de cliente de VPN. Estos archivos contienen la información necesaria para que el cliente se conecte a la red virtual. Los archivos configuran el cliente VPN existente que es nativo en el sistema operativo. Cada cliente que se conecta debe configurarse con los valores de los archivos de configuración.
