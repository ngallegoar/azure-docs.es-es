---
title: 'Tutorial: Incorporación de un segmento de red de NSX-T en Azure VMware Solution'
description: Aprenda a crear un segmento de red de NSX-T para usarlo en máquinas virtuales en vCenter.
ms.topic: tutorial
ms.date: 11/09/2020
ms.openlocfilehash: 8ecb37a42e2986bd1c6261b8fe6c23382323b31d
ms.sourcegitcommit: 2a8a53e5438596f99537f7279619258e9ecb357a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "94335055"
---
# <a name="tutorial-add-a-network-segment-in-azure-vmware-solution"></a>Tutorial: Incorporación de un segmento de red en Azure VMware Solution 

Las máquinas virtuales creadas en vCenter se colocan en los segmentos de red creados en NSX-T y son visibles en vCenter.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Desplazarse en NSX-T Manager para agregar segmentos de red
> * Agregar un nuevo segmento de red
> * Observar el nuevo segmento de red en vCenter

## <a name="prerequisites"></a>Requisitos previos

Una nube privada de Azure VMware Solution con acceso a las interfaces de vCenter y NSX-T Manager. Para obtener más información, consulte el tutorial de [configuración de redes](tutorial-configure-networking.md).

## <a name="add-a-network-segment"></a>Incorporación de un segmento de red

[!INCLUDE [add-network-segment-steps](includes/add-network-segment-steps.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado un segmento de red de NSX-T que se usa para las máquinas virtuales en vCenter. 

Ahora puede: 

- [Crear y administrar DHCP para Azure VMware Solution](manage-dhcp.md)
- [Crear una biblioteca de contenido para implementar máquinas virtuales en Azure VMware Solution](deploy-vm-content-library.md) 
- [Emparejamiento de entornos locales con una nube privada](tutorial-expressroute-global-reach-private-cloud.md)


<!-- LINKS - external-->

<!-- LINKS - internal -->
