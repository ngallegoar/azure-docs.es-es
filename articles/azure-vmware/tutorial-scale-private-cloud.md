---
title: 'Tutorial: Escalado de una nube privada'
description: En este tutorial, usará Azure Portal para escalar una nube privada de la versión preliminar de Azure VMware Solution.
ms.topic: tutorial
ms.date: 09/21/2020
ms.openlocfilehash: d49d973cc6d97280dc0c7ea6681f2602b871e1ba
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2020
ms.locfileid: "92791246"
---
# <a name="tutorial-scale-an-azure-vmware-solution-private-cloud"></a>Tutorial: Escalado de una nube privada de Azure VMware Solution

Para sacar el máximo partido de la experiencia de la nube privada de Azure VMware Solution, escale los clústeres y los hosts para que reflejen lo que necesita para las cargas de trabajo planeadas. Puede escalar los clústeres y los hosts en una nube privada, según sea necesario, para la carga de trabajo de la aplicación. Las limitaciones de rendimiento y disponibilidad de los servicios específicos deberán abordarse cada por caso. Los límites de hosts y clústeres se proporcionan en el artículo de [concepto de nube privada](concepts-private-clouds-clusters.md).

En este tutorial, usará Azure Portal para:

> [!div class="checklist"]
> * Agregar un clúster a una nube privada existente
> * Agregar nodos a un clúster existente

## <a name="prerequisites"></a>Prerrequisitos

Para completar este tutorial, necesita una nube privada. Si no ha creado una nube privada, use el tutorial para la [creación de una nube privada](tutorial-create-private-cloud.md) con el fin de crear una. Configure las redes para que la nube privada de VMware en Azure establezca la red virtual necesaria.

## <a name="add-a-new-cluster"></a>Incorporación de un nuevo clúster

1. En la página de información general de una nube privada existente, en **Administrar** , seleccione **Scale private cloud** (Escalar la nube privada). A continuación, seleccione **+ Agregar un clúster**.

   :::image type="content" source="./media/tutorial-scale-private-cloud/ss2-select-add-cluster.png" alt-text="selección de Agregar un clúster" border="true":::

1. En la página **Add cluster** (Agregar clúster), use el control deslizante para seleccionar el número de hosts. Seleccione **Guardar**.

   :::image type="content" source="./media/tutorial-scale-private-cloud/ss3-configure-new-cluster.png" alt-text="En la página Add cluster (Agregar clúster), use el control deslizante para seleccionar el número de hosts. Seleccione Guardar." border="true":::

   Se iniciará la implementación del nuevo clúster.

## <a name="scale-a-cluster"></a>Escalado de un clúster 

1. En la página de información general de una nube privada existente, seleccione **Scale private cloud** (Escalar la nube privada) y seleccione el icono de lápiz para editar el clúster.

   :::image type="content" source="./media/tutorial-scale-private-cloud/ss4-select-scale-private-cloud-2.png" alt-text="Seleccionar Scale private cloud en información general" border="true":::

1. En la página **Edit Cluster** (Editar clúster), use el control deslizante para seleccionar el número de hosts. Seleccione **Guardar**.

   :::image type="content" source="./media/tutorial-scale-private-cloud/ss5-scale-cluster.png" alt-text="En la página Edit Cluster (Editar clúster), use el control deslizante para seleccionar el número de hosts. Seleccione Guardar." border="true":::

   Empezarán a agregarse hosts al clúster.

## <a name="next-steps"></a>Pasos siguientes

Si necesita otra nube privada de Azure VMware Solution, [créela](tutorial-create-private-cloud.md), con los mismos requisitos previos de red, clúster y límites de host.

<!-- LINKS - external-->

<!-- LINKS - internal -->
