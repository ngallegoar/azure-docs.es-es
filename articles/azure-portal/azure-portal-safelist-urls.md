---
title: Guardado en la lista de IP seguras de las direcciones URL de Azure Portal en el servidor proxy o firewall
description: Agregar estas direcciones URL a la omisión del servidor proxy para comunicarse con Azure Portal y sus servicios
services: azure-portal
keywords: ''
author: mgblythe
ms.author: mblythe
ms.date: 04/10/2020
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: 16acedc8fad97c1752d71c8643b1655015484e5d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91330866"
---
# <a name="safelist-the-azure-portal-urls-on-your-firewall-or-proxy-server"></a>Guardado en la lista de IP seguras de las direcciones URL de Azure Portal en el servidor proxy o firewall

Puede configurar dispositivos de seguridad locales para omitir las restricciones de seguridad de las direcciones URL de Azure Portal. Esta configuración puede mejorar el rendimiento y la conectividad entre la red de área local o de área extensa y la nube de Azure.

A menudo, los administradores de red implementan servidores proxy, firewalls u otros dispositivos. Estos dispositivos ayudan a proteger y proporcionan control sobre el modo en que los usuarios acceden a Internet. Las reglas diseñadas para proteger a los usuarios a veces pueden bloquear o ralentizar el tráfico de Internet empresarial legítimo. Este tráfico incluye comunicaciones entre el usuario y Azure. Para optimizar la conectividad entre la red y Azure Portal y sus servicios, se recomienda agregar las direcciones URL de Azure Portal a la lista de IP seguras.

## <a name="azure-portal-urls-for-proxy-bypass"></a>Direcciones URL de Azure Portal para la omisión del proxy

Los puntos de conexión de URL a la lista de IP seguras de Azure Portal son específicos de la nube de Azure donde está implementada la organización. Para permitir el tráfico de red a estos puntos de conexión a fin de omitir restricciones, seleccione la nube. Luego agregue la lista de direcciones URL al servidor proxy o firewall.

#### <a name="public-cloud"></a>[Nube pública](#tab/public-cloud)

```
*.aadcdn.microsoftonline-p.com
*.aka.ms
*.applicationinsights.io
*.azure.com
*.azure.net
*.azurefd.net
*.azure-api.net
*.azuredatalakestore.net
*.azureedge.net
*.loganalytics.io
*.microsoft.com
*.microsoftonline.com
*.microsoftonline-p.com
*.msauth.net
*.msftauth.net
*.trafficmanager.net
*.visualstudio.com
*.windows.net
*.windows-int.net
```

#### <a name="us-government-cloud"></a>[Gobierno de gobierno de Estados Unidos](#tab/us-government-cloud)

```
*.applicationinsights.us
*.azure.us
*.loganalytics.us
*.microsoft.us
*.microsoftonline.us
*.msauth.net
*.usgovcloudapi.net
*.usgovtrafficmanager.net
*.windowsazure.us
```

#### <a name="china-government-cloud"></a>[Nube del gobierno de China](#tab/china-government-cloud)

```
*.azure.cn
*.microsoft.cn
*.microsoftonline.cn
*.chinacloudapi.cn
*.trafficmanager.cn
*.chinacloudsites.cn
*.windowsazure.cn
```
---

> [!NOTE]
> El tráfico a estos puntos de conexión usa puertos TCP estándar para HTTP (80) y HTTPS (443).
>
>
