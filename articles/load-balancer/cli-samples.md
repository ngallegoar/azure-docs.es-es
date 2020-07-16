---
title: Ejemplos de la CLI de Azure para Load Balancer
titleSuffix: Azure Load Balancer
description: Ejemplos de la CLI de Azure
services: load-balancer
documentationcenter: load-balancer
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 06/14/2018
ms.author: allensu
ms.openlocfilehash: 3198160fe443f4650d8c5a880c3538efeb9346a1
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86205263"
---
# <a name="azure-cli-samples-for-load-balancer"></a>Ejemplos de la CLI de Azure para Load Balancer

En la tabla siguiente se incluyen vínculos a scripts de Bash creados con la CLI de Azure.

* [Equilibrio de carga del tráfico a las máquinas virtuales para conseguir alta disponibilidad](./scripts/load-balancer-linux-cli-sample-nlb.md)

  Crea varias máquinas virtuales de alta disponibilidad y una configuración de equilibrio de carga.

* [Equilibrio de carga de máquinas virtuales en distintas zonas de disponibilidad](./scripts/load-balancer-linux-cli-sample-zone-redundant-frontend.md)

  Crea tres máquinas virtuales en zonas de disponibilidad diferentes dentro de una región y una instancia de Standard Load Balancer con una dirección IP de front-end con redundancia de zona. Esta configuración del equilibrador de carga le ayudará a proteger sus aplicaciones y sus datos en el caso improbable de que se produzca una pérdida o un error en todo el centro de datos.

* [Equilibrio de carga de máquinas virtuales en una zona de disponibilidad específica](./scripts/load-balancer-linux-cli-sample-zonal-frontend.md)

  Crea tres máquinas virtuales, una instancia de Standard Load Balancer con la dirección IP de front-end de zona que ayuda a alinear la ruta de acceso a los datos y los recursos en una sola zona de una región específica.

* [Equilibrio de carga entre varios sitios web en máquinas virtuales](./scripts/load-balancer-linux-cli-load-balance-multiple-websites-vm.md)

  Creación de dos máquinas virtuales con varias configuraciones de IP, unidas a un conjunto de disponibilidad de Azure, accesibles a través de una instancia de Azure Load Balancer.
