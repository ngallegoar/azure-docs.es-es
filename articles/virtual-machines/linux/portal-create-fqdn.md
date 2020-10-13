---
title: Creación de un nombre de dominio completo para una VM en Azure Portal
description: Aprenda a crear un nombre de dominio completo, o FQDN, para una máquina virtual Linux en Azure Portal.
author: cynthn
ms.service: virtual-machines
ms.subservice: networking
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 08/15/2018
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: deae22ad0c763e48df053d19beefba6054ed2767
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91331478"
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-linux-vm"></a>Creación de un nombre de dominio completo en Azure Portal para una máquina virtual Linux

Cuando crea una máquina virtual (VM) en [Azure Portal](https://portal.azure.com), se crea automáticamente un recurso de IP pública para la máquina virtual. Use esta dirección IP para acceder de forma remota a la máquina virtual. Aunque el portal no crea [un nombre de dominio completo](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), o FQDN, puede agregar uno cuando se crea la máquina virtual. Este artículo muestra los pasos para crear un nombre DNS o FQDN.

## <a name="create-a-fqdn"></a>Creación de un FQDN
En este artículo se supone que ya ha creado una máquina virtual. Si es necesario, puede [crear una máquina virtual en el portal](quick-create-portal.md) o [con la CLI de Azure](quick-create-cli.md). Una vez que la máquina virtual está en funcionamiento, siga estos pasos:

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

Ahora puede conectarse de manera remota a la máquina virtual con este nombre DNS, por ejemplo, con `ssh azureuser@mydns.westus.cloudapp.azure.com`.

## <a name="next-steps"></a>Pasos siguientes
Ahora que la máquina virtual tiene un nombre DNS y una IP pública, puede implementar marcos y servicios de aplicaciones comunes, como nginx, MongoDB, Docker, etc.

En [Información general sobre Azure Resource Manager](../../azure-resource-manager/management/overview.md) encontrará sugerencias sobre la creación de implementaciones de Azure.

