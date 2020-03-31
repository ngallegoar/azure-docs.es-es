---
title: Cree un grupo de selección de ubicación de proximidad con el portal
description: Aprenda cómo crear un grupo de selección de ubicación de proximidad con el Azure Portal.
services: virtual-machines
author: cynthn
manager: gwallace
ms.service: virtual-machines
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 10/30/2019
ms.author: cynthn
ms.openlocfilehash: 8512d9b701242dc686d49bbe56e8a2059f14ee3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73180024"
---
# <a name="create-a-proximity-placement-group-using-the-portal"></a>Cree un grupo de selección de ubicación de proximidad con el portal

Para acercar las máquinas virtuales lo máximo posible con la menor latencia, debe implementarlas dentro de un [grupo de selección de ubicación de proximidad](co-location.md#proximity-placement-groups).

Un grupo de selección de ubicación de proximidad es una agrupación lógica que se usa para asegurarse de que los recursos de proceso de Azure se encuentran físicamente cercanos entre sí. Los grupos de selección de ubicación de proximidad son útiles para las cargas de trabajo en las que la latencia baja es un requisito.


## <a name="create-the-proximity-placement-group"></a>Creación de un grupo de selección de ubicación de proximidad

1. Escriba **proximity placement group** 'grupo de selección de ubicación de proximidad' en el cuadro de búsqueda.
1. En **Servicios** en los resultados de la búsqueda, seleccione **Proximity placement groups (Grupos de selección de ubicación de proximidad)** .
1. En la página **Grupos de selección de ubicación de proximidad**, seleccione **Agregar**.
1. En la pestaña **Datos básicos**, en **Detalles del proyecto**, asegúrese de que está seleccionada la suscripción correcta.
1. En **Grupo de recursos** seleccione **Crear nuevo** para crear un nuevo grupo o seleccione un grupo de recursos existente de la lista desplegable.
1. En **Región** seleccione la ubicación en la que desea crear el grupo de selección de ubicación de proximidad.
1. En **Nombre de grupo de ubicación de proximidad** escriba un nombre y luego seleccione **Revisar + crear**.
1. Una vez que se haya superado la validación, seleccione **Crear** para crear el grupo de selección de ubicación de proximidad.

    ![Captura de pantalla de creación de un grupo de selección de ubicación de proximidad](./media/ppg/ppg.png)


## <a name="create-a-vm"></a>Crear una VM

1. Al crear una máquina virtual en el portal, vaya a la pestaña **Advanced**. 
1. En la selección **Grupo de selección de ubicación de proximidad**, elija el grupo de ubicación correcto. 

    ![Captura de pantalla de la sección del grupo de selección de ubicación de proximidad al crear una nueva máquina virtual en el portal](./media/ppg/vm-ppg.png)

1. Cuando haya terminado de realizar todas las demás selecciones necesarias, seleccione **Revisar + crear**.
1. Después de pasar la validación, seleccione **Crear** para implementar la máquina virtual en el grupo de ubicación.




## <a name="next-steps"></a>Pasos siguientes

También puede usar [Azure PowerShell](proximity-placement-groups.md) para crear grupos de selección de ubicación de proximidad.

