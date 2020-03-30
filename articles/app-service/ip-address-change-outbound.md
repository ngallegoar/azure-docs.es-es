---
title: Preparación para el cambio de la dirección IP de salida
description: Si la dirección IP de salida se va a cambiar, conozca qué debe hacer para que la aplicación continúe funcionando después del cambio.
ms.topic: article
ms.date: 06/28/2018
ms.custom: seodec18
ms.openlocfilehash: 2be4bc92dde278b054bd04f412f937440027ece7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74671664"
---
# <a name="how-to-prepare-for-an-outbound-ip-address-change"></a>Preparación de un cambio de dirección IP de salida

Si recibe una notificación indicándole que se van a cambiar las direcciones IP de salida de la aplicación de Azure App Service, siga las instrucciones de este artículo.

## <a name="determine-if-you-have-to-do-anything"></a>Determinar si es necesario realizar alguna acción por su parte

* Opción 1: si la aplicación de App Service no usa el filtrado de IP, una lista de inclusión explícita o un control especial del tráfico de salida como el enrutamiento o un firewall, no se requiere ninguna acción.

* Opción 2: si la aplicación tiene un control especial para las direcciones IP de salida (vea los ejemplos siguientes), agregue las nuevas direcciones IP de salida dondequiera que aparezcan las direcciones ya existentes. No reemplace las direcciones IP ya existentes. Puede encontrar las nuevas direcciones IP de salida mediante las instrucciones que se indican en la sección siguiente.

  Por ejemplo, una dirección IP de salida puede estar incluida explícitamente en un firewall fuera de la aplicación o puede que un servicio de pagos externo tenga una lista de permitidos que contenga la dirección IP de salida de la aplicación. Si su dirección de salida está configurada en una lista fuera de la aplicación, eso debe cambiar.

## <a name="find-the-outbound-ip-addresses-in-the-azure-portal"></a>Búsqueda de las direcciones IP de salida en Azure Portal

Las nuevas direcciones IP de salida se muestran en el portal antes de entrar en vigor. Cuando Azure empieza a usar las nuevas, las antiguas dejan de usarse. Solo se usa un conjunto cada vez, por lo que las entradas de las listas de inclusión deben tener las direcciones IP nuevas y las antiguas para impedir una interrupción cuando se produce el cambio. 

1.  Abra [Azure Portal](https://portal.azure.com).

2.  En el menú de navegación izquierdo, seleccione **App Services**.

3.  Seleccione la aplicación de App Service en la lista.

1.  Si la aplicación es una aplicación de función, consulte la sección [Function app outbound IP address](../azure-functions/ip-addresses.md#find-outbound-ip-addresses) (Dirección IP saliente de la aplicación de función).

4.  En el encabezado **Configuración**, haga clic en **Propiedades** en el menú de navegación izquierdo y busque la sección denominada **Direcciones IP de salida**.

5. Copie las direcciones IP y agréguelas a su control especial de tráfico de salida como un filtro o una lista de permitidos. No elimine las direcciones IP existentes de la lista.

## <a name="next-steps"></a>Pasos siguientes

En este artículo se explica cómo preparar un cambio de dirección IP que Azure inició. Para más información sobre las direcciones IP en Azure App Service, consulte [direcciones IP de entrada y salida en Azure App Service](overview-inbound-outbound-ips.md).
