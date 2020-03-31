---
title: Guardado en la lista de IP seguras de las direcciones URL de Azure Portal en el servidor proxy o firewall
description: Agregar estas direcciones URL a la omisión del servidor proxy para comunicarse con Azure Portal y sus servicios
services: azure-portal
keywords: ''
author: mgblythe
ms.author: mblythe
ms.date: 01/29/2020
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: c5bba7296a05cfbb72698a991ece1ef298689bd1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76900666"
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
*.azureafd.net
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
## <a name="next-steps"></a>Pasos siguientes

¿Necesita guardar direcciones IP en la lista de IP seguras? Descargue la lista de intervalos IP del centro de datos de Microsoft Azure para su nube:

* [Internacional](https://www.microsoft.com/download/details.aspx?id=56519)
* [Gobierno de Estados Unidos](https://www.microsoft.com/download/details.aspx?id=57063)
* [Alemania](https://www.microsoft.com/download/details.aspx?id=57064)
* [China](https://www.microsoft.com/download/details.aspx?id=57062)

Otro servicios de Microsoft usan direcciones URL y direcciones IP adicionales para la conectividad. Para optimizar la conectividad de red de los servicios de Microsoft 365, consulte [Set up your network for Office 365](/office365/enterprise/set-up-network-for-office-365) (Configuración de la red para Office 365).
