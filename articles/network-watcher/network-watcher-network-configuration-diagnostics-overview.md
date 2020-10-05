---
title: Introducción a los diagnósticos de configuración de red en Azure Network Watcher | Microsoft Docs
description: 'En esta página se ofrece información general de Network Watcher: diagnósticos de configuración de red'
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/15/2020
ms.author: damendo
ms.openlocfilehash: 4d308b8a1a589308e2481c8bb9e0dc9ec64be25b
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2020
ms.locfileid: "90993660"
---
# <a name="introduction-to-network-configuration-diagnostics-in-azure-network-watcher"></a>Introducción a diagnósticos de configuración de red en Azure Network Watcher

La herramienta de diagnóstico de configuración de red ayuda a los clientes a comprender qué flujos de tráfico se permitirán o denegarán en Azure Virtual Network junto con información detallada para la depuración. Puede ayudar a comprender si las reglas de NSG se configuran correctamente. 

## <a name="pre-requisites"></a>Requisitos previos
Para usar diagnósticos de configuración de red, Network Watcher debe estar habilitado en la suscripción. Consulte [Creación de una instancia de Azure Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-create) para habilitarlo.

## <a name="background"></a>Fondo

- Los recursos de Azure se conectan a través de redes virtuales (VNET) y subredes. La seguridad de estas redes virtuales y subredes se puede administrar mediante un grupo de seguridad de red (NSG).
- Un NSG contiene una lista de reglas de seguridad que permiten o deniegan el tráfico de red a los recursos a los que se conecta. Los grupos de seguridad de red se pueden asociar a subredes, máquinas virtuales individuales o interfaces de red (NIC) individuales conectadas a máquinas virtuales. 
- Todos los flujos de tráfico de la red se evalúan mediante las reglas de los grupos de seguridad de red correspondientes.
- Las reglas se evalúan en función del número de prioridad, de menor a mayor. 

## <a name="how-does-network-configuration-diagnostic-work"></a>¿Cómo funciona el diagnóstico de configuración de red? 

Para un flujo determinado, la herramienta NCD ejecuta una simulación del flujo y devuelve si el flujo se permitiría (o denegaría) e información detallada sobre las reglas que permiten o deniegan el flujo.  Los clientes deben proporcionar detalles de un flujo como origen, destino, protocolo, etc. La herramienta devuelve si se permitió o denegó el tráfico, las reglas de NSG que se evaluaron para el flujo especificado y los resultados de la evaluación para cada regla.

## <a name="next-steps"></a>Pasos siguientes

Uso del diagnóstico de configuración de red a través de otras interfaces
 - [REST API](https://docs.microsoft.com/rest/api/network-watcher/networkwatchers/getnetworkconfigurationdiagnostic)
 - [PowerShell](https://docs.microsoft.com/powershell/module/az.network/invoke-aznetworkwatchernetworkconfigurationdiagnostic?view=azps-4.6.1)
 - [CLI de Azure](https://docs.microsoft.com/cli/azure/network/watcher?view=azure-cli-latest#az_network_watcher_run_configuration_diagnostic)

