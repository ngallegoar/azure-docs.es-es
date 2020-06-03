---
title: Tunelización forzada de Azure Firewall
description: Puede configurar la tunelización forzada para enrutar el tráfico vinculado a Internet con una aplicación virtual de red o firewall para su posterior procesamiento.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 05/18/2020
ms.author: victorh
ms.openlocfilehash: cf0af93d95c5af56be6168bc8e4f79d3005e2ec2
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83649602"
---
# <a name="azure-firewall-forced-tunneling"></a>Tunelización forzada de Azure Firewall

Al configurar una nueva instancia de Azure Firewall, puede enrutar todo el tráfico vinculado a Internet a un próximo salto designado, en lugar de ir directamente a Internet. Por ejemplo, puede tener un servidor perimetral local u otra aplicación virtual de red (NVA) para procesar el tráfico de red antes de que pase a Internet. Sin embargo, no se puede configurar un firewall existente para la tunelización forzada.

De forma predeterminada, no se permite la tunelización forzada en Azure Firewall para asegurarse de que se cumplen todas sus dependencias de Azure de salida. Las configuraciones de la ruta definida por el usuario (UDR) en *AzureFirewallSubnet* que tienen una ruta predeterminada que no va directamente a Internet están deshabilitadas.

## <a name="forced-tunneling-configuration"></a>Configuración de la tunelización forzada

Para admitir la tunelización forzada, el tráfico de administración de servicio se separa del tráfico del cliente. Se requiere una subred dedicada adicional denominada *AzureFirewallManagementSubnet* (tamaño de subred mínimo /26) con su propia dirección IP pública asociada. La única ruta permitida en esta subred es una ruta predeterminada a Internet y la propagación de la ruta BGP debe estar deshabilitada.

Si tiene una ruta predeterminada anunciada mediante BGP para forzar el tráfico a un entorno local, debe crear *AzureFirewallSubnet* y *AzureFirewallManagementSubnet* antes de implementar el firewall, tener una UDR con una ruta predeterminada a Internet y deshabilitar **Propagación de rutas de puerta de enlace de red virtual**.

En esta configuración, *AzureFirewallSubnet* puede incluir rutas a cualquier firewall local o NVA para procesar el tráfico antes de pasarlo a Internet. También puede publicar estas rutas mediante BGP en *AzureFirewallSubnet* si está habilitada la opción **Propagación de rutas de puerta de enlace de red virtual** en esta subred.

Por ejemplo, puede crear una ruta predeterminada en *AzureFirewallSubnet* con su puerta de enlace de VPN como el próximo salto para llegar al dispositivo local. O bien, puede habilitar **Propagación de rutas de puerta de enlace de red virtual** para obtener las rutas adecuadas a la red local.

![Propagación de rutas de puerta de enlace de red virtual](media/forced-tunneling/route-propagation.png)

Después de configurar Azure Firewall para admitir la tunelización forzada, no se puede deshacer la configuración. Si quita el resto de configuraciones de IP del firewall, también se quitará la configuración de IP de administración y se desasignará el firewall. No se puede quitar la dirección IP pública asignada a la configuración de IP de administración, pero puede asignar una dirección IP pública diferente.

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: Implementación y configuración de Azure Firewall en una red híbrida con Azure Portal](tutorial-hybrid-portal.md)
