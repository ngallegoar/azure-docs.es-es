---
title: 'Inicio rápido: Vinculación de un área de trabajo de Power BI a un área de trabajo de Synapse'
description: Vincule un área de trabajo de Power BI a un área de trabajo de Azure Synapse Analytics siguiendo los pasos de esta guía.
services: synapse-analytics
author: jocaplan
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/29/2020
ms.author: jocaplan
ms.reviewer: jrasnick
ms.openlocfilehash: 9b4b9da37b753418773141f832f253003ad40014
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91260467"
---
# <a name="quickstart-linking-a-power-bi-workspace-to-a-synapse-workspace"></a>Inicio rápido: Vinculación de un área de trabajo de Power BI a un área de trabajo de Synapse

En este inicio rápido, aprenderá a conectar un área de trabajo de Power BI a un área de trabajo de Synapse Analytics para crear informes de Power BI y conjuntos de valores de Synapse Studio (versión preliminar).

Si no tiene una suscripción a Azure, [cree una cuenta gratuita antes de empezar](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Requisitos previos

- [Creación de un área de trabajo de Azure Synapse y una cuenta de almacenamiento asociada](quickstart-create-workspace.md).
- [Un área de trabajo de Power BI Professional o Premium](https://docs.microsoft.com/power-bi/service-create-the-new-workspaces).

## <a name="link-power-bi-workspace-to-your-synapse-workspace"></a>Vinculación de un área de trabajo de Power BI a su área de trabajo de Synapse

1. Inicie Synapse Studio y haga clic en **Manage** (Administrar).

    ![Synapse Studio, haga clic en Manage (Administrar).](media/quickstart-link-powerbi/synapse-studio-click-manage.png)

2. En **External Connections** (Conexiones externas), haga clic en **Linked services** (Servicios vinculados).

    ![Servicios vinculados resaltados.](media/quickstart-link-powerbi/manage-click-linked-services.png)

3. Haga clic en **+ Nuevo**.

    ![+New (+Nuevo) está resaltado en Linked services (Servicios vinculados)](media/quickstart-link-powerbi/new-highlighted.png)

4. Haga clic en **Power BI** y, después en **Continue** (Continuar).

    ![Mostrar servicio vinculado de Power BI.](media/quickstart-link-powerbi/powerbi-linked-service.png)

5. Escriba un nombre para el servicio vinculado y seleccione un área de trabajo en la lista desplegable.

    ![Mostrar la instalación del servicio vinculado de Power BI.](media/quickstart-link-powerbi/workspace-link-dialog.png)

6. Haga clic en **Crear**.

## <a name="view-power-bi-workspace-in-synapse-studio"></a>Visualización del área de trabajo de Power BI en Synapse Studio

Una vez que las áreas de trabajo están vinculadas, desde Synapse Studio puede examinar los conjuntos de valores de Power BI, editar o crear nuevos informes de Power BI.

1. Haga clic en **Develop** (Desarrollar).

    ![Synapse Studio, haga clic en Develop (Desarrollar).](media/quickstart-link-powerbi/synapse-studio-click-develop.png)

2. Expanda Power BI y el área de trabajo que desea utilizar.

    ![Expanda Power BI y el área de trabajo.](media/quickstart-link-powerbi/develop-expand-powerbi.png)

Para crear informes, haga clic en **+** en la parte superior de la pestaña **Develop** (Desarrollar). Para editar los informes existentes, haga clic en su nombre. Los cambios guardados se volverán a escribir en el área de trabajo de Power BI.

![Ver y editar un informe de Power BI.](media/quickstart-link-powerbi/powerbi-report.png)


## <a name="next-steps"></a>Pasos siguientes

Más información acerca de la [creación de un informe de Power BI en archivos almacenados en Azure Storage](sql/tutorial-connect-power-bi-desktop.md).
