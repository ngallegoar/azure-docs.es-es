---
title: Creación de un nombre de dominio completo para una VM en Azure Portal
description: Aprenda a crear un nombre de dominio completo para una máquina virtual en Azure Portal.
author: cynthn
ms.service: virtual-machines
ms.subservice: networking
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 11/03/2020
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 08d5e20bf0755a71e70a0e446cf96d33bb42ad59
ms.sourcegitcommit: 99955130348f9d2db7d4fb5032fad89dad3185e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93351853"
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-linux-vm"></a>Creación de un nombre de dominio completo en Azure Portal para una máquina virtual Linux

Cuando crea una máquina virtual (VM) en [Azure Portal](https://portal.azure.com), se crea automáticamente un recurso de IP pública para la máquina virtual. Use esta dirección IP para acceder de forma remota a la máquina virtual. Aunque el portal no crea [un nombre de dominio completo](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), o FQDN, puede agregar uno cuando se crea la máquina virtual. Este artículo muestra los pasos para crear un nombre DNS o FQDN. 

## <a name="create-a-fqdn"></a>Creación de un FQDN
En este artículo se supone que ya ha creado una máquina virtual. Si es necesario, puede crear una máquina virtual [Linux](./linux/quick-create-portal.md) o [Windows](./windows/quick-create-portal.md) en el portal. Una vez que la máquina virtual está en funcionamiento, siga estos pasos:


1. Seleccione la máquina virtual en el portal. En **Nombre DNS** , seleccione **Configurar**.
2. Escriba el nombre DNS y seleccione **Guardar** en la parte superior de la página.
3. Para volver a la hoja de información general de la máquina virtual, cierre la hoja **Configuración** , para lo que debe seleccionar la **X** en la esquina superior derecha. 
4. Compruebe que el *nombre DNS* se muestra ahora correctamente.
   



## <a name="next-steps"></a>Pasos siguientes
Ahora que la máquina virtual tiene un nombre DNS y una IP pública, puede implementar marcos y servicios de aplicaciones comunes, como nginx, MongoDB y Docker.

En [Información general sobre Azure Resource Manager](../azure-resource-manager/management/overview.md) encontrará sugerencias sobre la creación de implementaciones de Azure.

