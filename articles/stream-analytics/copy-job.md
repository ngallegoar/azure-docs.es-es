---
title: Copia o copia de seguridad de trabajos de Azure Stream Analytics
description: En este artículo se describe como copiar o hacer una copia de seguridad de un trabajo de Azure Stream Analytics.
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 09/11/2019
ms.openlocfilehash: ba63358347cf9722d2cafa35598b9b3b37f49dc3
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93129463"
---
# <a name="copy-or-back-up-azure-stream-analytics-jobs"></a>Copia o copia de seguridad de trabajos de Azure Stream Analytics

Puede copiar o hacer una copia de seguridad de los trabajos de Azure Stream Analytics implementados mediante Visual Studio Code o Visual Studio. Al copiar un trabajo en otra región, no se copia la hora de la última salida. Por lo tanto, no se puede usar la opción [**Última detención**](./start-job.md#start-options) al iniciar el trabajo copiado.

## <a name="before-you-begin"></a>Antes de empezar
* Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/).

* Inicie sesión en [Azure Portal](https://portal.azure.com/).

* Instale la [extensión Azure Stream Analytics para Visual Studio Code](quick-create-visual-studio-code.md#install-the-azure-stream-analytics-tools-extension) o las [herramientas de Azure Stream Analytics para Visual Studio](quick-create-visual-studio-code.md#install-the-azure-stream-analytics-tools-extension).  

## <a name="visual-studio-code"></a>Visual Studio Code

1. Haga clic en el icono de **Azure** en la barra de actividades de código de Visual Studio Code y, después, expanda el nodo **Stream Analytics**. Los trabajos deben aparecer en las suscripciones.

   ![Apertura del explorador de Stream Analytics](./media/vscode-explore-jobs/open-explorer.png)

2. Para exportar un trabajo a un proyecto local, busque el trabajo que desea exportar en el **explorador de Stream Analytics** en Visual Studio Code. Después seleccione una carpeta para el proyecto.

    ![Búsqueda de un trabajo de ASA en Visual Studio Code](./media/vscode-explore-jobs/export-job.png)

    El proyecto se exporta a la carpeta que seleccione y se agrega al área de trabajo actual.

    ![Exportación de un trabajo de ASA en Visual Studio Code](./media/stream-analytics-manage-job/copy-backup-stream-analytics-jobs.png)

3. Para publicar el trabajo en otra región o copia de seguridad con otro nombre, seleccione **Seleccione entre las suscripciones para publicar** en el editor de consultas (\*.asaql) y siga las instrucciones.

    ![Publicación en Azure en Visual Studio Code](./media/quick-create-visual-studio-code/submit-job.png)

## <a name="visual-studio"></a>Visual Studio

1. Siga las [instrucciones para exportar a un proyecto un trabajo de Azure Stream Analytics implementado](./stream-analytics-vs-tools.md#export-jobs-to-a-project).

2. Abra el archivo \*.asaql en el editor de consultas, seleccione **Enviar a Azure** en el editor de scripts y siga las instrucciones para publicar el trabajo en otra región o copia de seguridad con un nombre nuevo.

## <a name="next-steps"></a>Pasos siguientes

* [Inicio rápido: Creación de un trabajo de Stream Analytics mediante Visual Studio Code](quick-create-visual-studio-code.md)
* [Inicio rápido: Creación de un trabajo de Stream Analytics mediante Visual Studio](stream-analytics-quick-create-vs.md)
* [Implementación de un trabajo de Azure Stream Analytics con CI/CD mediante Azure Pipelines](stream-analytics-tools-visual-studio-cicd-vsts.md)