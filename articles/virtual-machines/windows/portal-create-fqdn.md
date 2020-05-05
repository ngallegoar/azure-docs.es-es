---
title: Creación de un nombre de dominio completo para una máquina virtual Windows en Azure Portal
description: Aprenda a crear un nombre de dominio completo, o FQDN, para una máquina virtual basada en Resource Manager en Azure Portal.
author: cynthn
ms.service: virtual-machines-windows
ms.subservice: networking
ms.topic: article
ms.workload: infrastructure-services
ms.date: 08/15/2018
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ece0877c97d3c2cda30aab1b3f7fe56f20080996
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82101661"
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-windows-vm"></a>Creación de un nombre de dominio completo en Azure Portal para una máquina virtual Windows

Cuando crea una máquina virtual (VM) en [Azure Portal](https://portal.azure.com), se crea automáticamente un recurso de IP pública para la máquina virtual. Use esta dirección IP para acceder de forma remota a la máquina virtual. Aunque el portal no crea [un nombre de dominio completo](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), o FQDN, puede crear uno cuando se crea la máquina virtual. Este artículo muestra los pasos para crear un nombre DNS o FQDN.

## <a name="create-a-fqdn"></a>Creación de un FQDN
En este artículo se supone que ya ha creado una máquina virtual. Si es necesario, puede [crear una máquina virtual en el portal](quick-create-portal.md) o [con Azure PowerShell](quick-create-powershell.md). Una vez que la máquina virtual está en funcionamiento, siga estos pasos:

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

Ahora puede conectarse de forma remota a la máquina virtual con este nombre DNS, por ejemplo, para el protocolo de escritorio remoto (RDP).

## <a name="next-steps"></a>Pasos siguientes
Ahora que la máquina virtual tiene un nombre DNS y una IP pública, puede implementar marcos o servicios de aplicaciones comunes, como IIS, SQL o SharePoint.

En [Información general sobre Azure Resource Manager](../../azure-resource-manager/management/overview.md) encontrará sugerencias sobre la creación de implementaciones de Azure.

