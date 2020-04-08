---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/12/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4ea97e2dbee87f7ab129c4295276c9024c0212c7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80117139"
---
Always On es una nueva característica del cliente VPN de Windows 10 que ofrece la posibilidad de mantener una conexión VPN. Con Always On, el perfil activo de VPN se puede conectar automáticamente y permanecer conectado según los desencadenadores como, por ejemplo, en los inicios de sesión de los usuarios, los cambios en el estado de la red o la activación de la pantalla del dispositivo.

Las puertas de enlace pueden utilizarse con Windows 10 Always On para establecer túneles de usuario persistentes, así como túneles de dispositivo dirigidos a Azure. Este artículo le ayudará a configurar un túnel de usuario de VPN de Always On.

Las conexiones VPN de Always On incluyen uno de estos dos tipos de túneles:

* **Túnel de dispositivo**: se conecta a los servidores VPN especificados antes de que los usuarios inicien sesión en el dispositivo. Los túneles de dispositivo se utilizan en escenarios de conectividad previos al inicio de sesión y para administrar dispositivos.

* **Túnel de usuario**: solo se conecta después de que los usuarios inicien sesión en el dispositivo. Mediante los túneles de usuario puede acceder a los recursos de la organización utilizando servidores VPN.

Los túneles de dispositivo y los de usuario funcionan de forma independiente a los perfiles VPN. Pueden conectarse al mismo tiempo y utilizar diferentes métodos de autenticación u otras opciones de configuración de VPN en función de las necesidades.
