---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 10/06/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 317d75dae83e8529bda25897b77824d8cd36e72e
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "95554260"
---
1. En la página de su red WAN virtual, seleccione **Configuraciones de VPN de usuario**.
1. En la parte superior de la página, seleccione **Download user VPN config** (Descargar configuración de VPN de usuario). Al descargar la configuración del nivel de WAN, se obtiene un perfil de VPN de usuario basado en Traffic Manager. Para más información sobre los perfiles globales o sobre cualquier perfil basado en un centro de conectividad, consulte [Perfiles de centro de conectividad](../articles/virtual-wan/global-hub-profile.md). Los escenarios de conmutación por error se simplifican con los perfiles globales.

   Si, por alguna razón, alguno de los centros de conectividad no está disponible, la administración de tráfico integrada que proporciona el servicio garantiza la conectividad (a través de otro centro) a los recursos de Azure para los usuarios de punto a sitio. Siempre puede descargar una configuración de VPN específica del centro de conectividad. Para ello, vaya al centro de conectividad. En **VPN de usuario (punto a sitio)** , descargue el perfil de **VPN de usuario** del centro de conectividad virtual.
1. Una vez que el archivo haya terminado de crearse, puede seleccionar el vínculo para descargarlo.
1. Use el archivo de perfil para configurar los clientes de VPN.