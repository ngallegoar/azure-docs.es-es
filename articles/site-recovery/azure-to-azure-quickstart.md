---
title: Configuración de la recuperación ante desastres de máquinas virtuales de Azure en una región secundaria con Azure Site Recovery
description: Configure rápidamente la recuperación ante desastres en otra región de Azure para una máquina virtual de Azure mediante el servicio de Azure Site Recovery.
ms.topic: quickstart
ms.date: 03/27/2020
ms.custom: mvc
ms.openlocfilehash: 8681ccaac30402927005e5e8802d7da2bddc2c5c
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86135709"
---
# <a name="quickstart-set-up-disaster-recovery-to-a-secondary-azure-region-for-an-azure-vm"></a>Inicio rápido: Configuración de la recuperación ante desastres en una región secundaria de Azure de una máquina virtual de Azure

El servicio [Azure Site Recovery](site-recovery-overview.md) contribuye a la estrategia de recuperación ante desastres y continuidad empresarial (BCDR) al mantener las aplicaciones empresariales en línea durante las interrupciones planeadas y no planeadas. Site Recovery administra y coordina la recuperación ante desastres de máquinas locales y máquinas virtuales de Azure (VM), lo que incluye la replicación, la conmutación por error y la recuperación.

En este inicio rápido se describe cómo configurar la recuperación ante desastres en una máquina virtual de Azure mediante la replicación en una región secundaria de Azure. En general, se utiliza la configuración predeterminada para habilitar la replicación.

## <a name="prerequisites"></a>Prerrequisitos

Para completar este tutorial, necesitará una suscripción de Azure y una máquina virtual.

- Si no tiene una cuenta de Azure con una suscripción activa, puede [crear una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Se recomienda una máquina virtual con un mínimo de 1 GB de RAM. [Más información](../virtual-machines/windows/quick-create-portal.md) sobre cómo crear una máquina virtual.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en [Azure Portal](https://portal.azure.com).

## <a name="enable-replication-for-the-azure-vm"></a>Habilitación de la replicación para la máquina virtual de Azure

Los pasos siguientes habilitan la replicación de la máquina virtual en una ubicación secundaria.

1. En Azure Portal, en el menú **Página principal** > **Máquinas virtuales**, seleccione una máquina virtual para replicarla.
1. En **Operaciones**, seleccione **Recuperación ante desastres**.
1. En **Básico** > **Región de destino**, seleccione la región de destino.
1. Para ver la configuración de replicación, seleccione **Revisar e iniciar replicación**. Si necesita cambiar los valores predeterminados, seleccione **Configuración avanzada**.
1. Para iniciar el trabajo que habilita la replicación de la máquina virtual, seleccione **Iniciar replicación**.

   :::image type="content" source="media/azure-to-azure-quickstart/enable-replication1.png" alt-text="Habilitación de la replicación.":::

## <a name="verify-settings"></a>Comprobación de la configuración

Cuando haya finalizado el trabajo de replicación, puede comprobar el estado de la replicación, modificar la configuración de la replicación y probar la implementación.

1. En el menú de Azure Portal, seleccione **Máquinas virtuales** y seleccione la máquina virtual que ha replicado.
1. En **Operaciones**, seleccione **Recuperación ante desastres**.
1. Para ver los detalles de la replicación, en **Información general**, seleccione **Essentials**. Se muestran más detalles en **Mantenimiento y estado**, **Preparación de la conmutación por error** y el mapa **Vista de la infraestructura**.

   :::image type="content" source="media/azure-to-azure-quickstart/replication-status.png" alt-text="Estado de replicación.":::

## <a name="clean-up-resources"></a>Limpieza de recursos

Para detener la replicación de la máquina virtual de la región primaria, debe deshabilitar la replicación:

- La configuración de replicación de origen se limpia automáticamente.
- La extensión de Site Recovery instalada en la máquina virtual durante la replicación no se elimina.
- La facturación de Site Recovery para la máquina virtual se detiene.

Para deshabilitar la replicación, siga estos pasos:

1. En el menú de Azure Portal, seleccione **Máquinas virtuales** y seleccione la máquina virtual que ha replicado.
1. En **Operaciones**, seleccione **Recuperación ante desastres**.
1. En **Información general**, seleccione **Deshabilitar replicación**.
1. Para desinstalar la extensión de Site Recovery, vaya a la opción **Configuración** > **Extensiones** de la máquina virtual.

   :::image type="content" source="media/azure-to-azure-quickstart/disable2-replication.png" alt-text="Deshabilitar replicación.":::

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, se replica una única máquina virtual en una región secundaria. A continuación, configure la replicación para varias máquinas virtuales de Azure.

> [!div class="nextstepaction"]
> [Configuración de la recuperación ante desastres de máquinas virtuales de Azure](azure-to-azure-tutorial-enable-replication.md)
