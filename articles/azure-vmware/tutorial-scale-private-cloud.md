---
title: 'Tutorial: Escalado de una nube privada'
description: En este tutorial, usará Azure Portal para escalar una nube privada de la versión preliminar de Azure VMware Solution (AVS).
ms.topic: tutorial
ms.date: 05/04/2020
ms.openlocfilehash: 70d8c1c555badd6102f4b2547492bdea54e55783
ms.sourcegitcommit: d9cd51c3a7ac46f256db575c1dfe1303b6460d04
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/04/2020
ms.locfileid: "82739730"
---
# <a name="tutorial-scale-an-azure-vmware-solution-avs-preview-private-cloud"></a>Tutorial: Escalado de una nube privada en la versión preliminar de Azure VMware Solution (AVS)

Para sacar el máximo partido de la experiencia de la nube privada de la versión preliminar de AVS, escale los clústeres y los hosts para que reflejen lo que necesite para las cargas de trabajo planeadas. Como AVS no admitirá el vCenter local durante la versión preliminar, deberá usar lo que ya ha creado mediante Azure Portal.

Puede escalar el número de clústeres y el número de hosts en una nube privada según sea necesario para la carga de trabajo de la aplicación. Las limitaciones de rendimiento y disponibilidad para servicios específicos deben abordarse en función de cada caso en el entorno en la nube de la versión preliminar de AVS. Los límites de hosts y clústeres en una nube privada se proporcionan en [el artículo de conceptos de nube privada](concepts-private-clouds-clusters.md).

En este tutorial se usa Azure Portal para:

> [!div class="checklist"]
> * Agregar un clúster a una nube privada existente
> * Agregar nodos a un clúster existente

## <a name="prerequisites"></a>Prerrequisitos

Necesita una nube privada para completar este tutorial. Si aún no ha creado una nube privada, use el tutorial para [crear una nube privada](tutorial-create-private-cloud.md) a fin de crear una nube privada, configurar redes para la nube privada de VMware en Azure y configurar la red virtual requerida.

## <a name="add-a-new-cluster"></a>Incorporación de un nuevo clúster

En la página de información general de una nube privada existente, en **Administrar**, seleccione **Scale private cloud** (Escalar la nube privada). A continuación, seleccione **+ Agregar un clúster**.

:::image type="content" source="./media/tutorial-scale-private-cloud/ss2-select-add-cluster.png" alt-text="selección de Agregar un clúster" border="true":::

En la página **Add cluster** (Agregar clúster), use el control deslizante para seleccionar el número de hosts. Seleccione **Guardar**.

:::image type="content" source="./media/tutorial-scale-private-cloud/ss3-configure-new-cluster.png" alt-text="Configuración de un nuevo clúster de nube privada" border="true":::

Se iniciará la implementación del nuevo clúster.

## <a name="scale-a-cluster"></a>Escalado de un clúster 

En la página de información general de una nube privada existente, seleccione **Scale private cloud** (Escalar la nube privada) y seleccione el icono de lápiz para editar el clúster.

:::image type="content" source="./media/tutorial-scale-private-cloud/ss4-select-scale-private-cloud-2.png" alt-text="Seleccionar Scale private cloud en información general" border="true":::

En la página **Edit Cluster** (Editar clúster), use el control deslizante para seleccionar el número de hosts. Seleccione **Guardar**.

:::image type="content" source="./media/tutorial-scale-private-cloud/ss5-scale-cluster.png" alt-text="Configuración de un nuevo clúster de nube privada" border="true":::

Empezarán a agregarse hosts al clúster.

## <a name="next-steps"></a>Pasos siguientes

Si necesita otra nube privada de AVS, [créela](tutorial-create-private-cloud.md); para ello, siga los mismos requisitos previos de red, clústeres y límites de host.

<!-- LINKS - external-->

<!-- LINKS - internal -->
