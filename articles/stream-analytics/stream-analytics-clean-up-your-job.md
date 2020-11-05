---
title: Limpiar un trabajo de Azure Stream Analytics
description: En este artículo se muestran distintos métodos para eliminar los trabajos de Azure Stream Analytics.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 06/21/2019
ms.custom: seodec18
ms.openlocfilehash: 2652b97c9392d3016bbc52209d4b2bda81c31706
ms.sourcegitcommit: 99955130348f9d2db7d4fb5032fad89dad3185e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93348974"
---
# <a name="stop-or-delete-your-azure-stream-analytics-job"></a>Detener o eliminar un trabajo de Azure Stream Analytics

Los trabajos de Azure Stream Analytics se pueden detener o eliminar fácilmente desde Azure Portal, Azure PowerShell, el SDK de Azure para .Net o la API de REST. Un trabajo de Stream Analytics no puede recuperarse una vez que se ha eliminado.

>[!NOTE] 
>Cuando termine su trabajo en Stream Analytics, los datos solo se conservarán en almacenaje de entrada y de salida, como Event Hubs o Azure SQL Database. Si se le solicita que elimine datos de Azure, asegúrese de seguir el proceso de supresión de los recursos de entrada y salida de su trabajo en Stream Analytics.

## <a name="stop-a-job-in-azure-portal"></a>Detener un trabajo en Azure Portal

Cuando se detiene un trabajo, los recursos se desaprovisionan y se detiene el procesamiento de eventos. También se detienen los cargos relacionados con este trabajo. Sin embargo, se mantiene toda su configuración y puede reiniciar el trabajo más tarde. 

1. Inicie sesión en [Azure Portal](https://portal.azure.com). 

2. Busque el trabajo de Stream Analytics que se está ejecutando y selecciónelo.

3. En la página del trabajo de Stream Analytics, seleccione **Detener** para detener el trabajo. 

   ![Detención de un trabajo de Azure Stream Analytics](./media/stream-analytics-clean-up-your-job/stop-stream-analytics-job.png)


## <a name="delete-a-job-in-azure-portal"></a>Eliminación un trabajo en Azure Portal

>[!WARNING] 
>Un trabajo de Stream Analytics no puede recuperarse una vez que se ha eliminado.

1. Inicie sesión en Azure Portal. 

2. Busque el trabajo de Stream Analytics existente y selecciónelo.

3. En la página del trabajo de Stream Analytics, seleccione **Eliminar** para eliminar el trabajo. 

   ![Eliminación de un trabajo de Azure Stream Analytics](./media/stream-analytics-clean-up-your-job/delete-stream-analytics-job.png)


## <a name="stop-or-delete-a-job-using-powershell"></a>Detener o eliminar un trabajo mediante PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Para detener un trabajo con PowerShell, use el cmdlet [Stop-AzStreamAnalyticsJob](/powershell/module/az.streamanalytics/stop-azstreamanalyticsjob). Para eliminar un trabajo con PowerShell, use el cmdlet [Remove-AzStreamAnalyticsJob](/powershell/module/az.streamanalytics/Remove-azStreamAnalyticsJob).

## <a name="stop-or-delete-a-job-using-azure-sdk-for-net"></a>Detener o eliminar un trabajo mediante el SDK de Azure para .NET

Para detener un trabajo mediante el SDK de Azure para. NET, use el método [StreamingJobsOperationsExtensions.BeginStop](/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.beginstop). Para eliminar un trabajo mediante el SDK de Azure para. NET, use el método [StreamingJobsOperationsExtensions.BeginDelete](/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.begindelete).

## <a name="stop-or-delete-a-job-using-rest-api"></a>Detener o eliminar un trabajo mediante la API de REST

Para detener un trabajo mediante la API de REST, consulte el método [Stop](/rest/api/streamanalytics/2016-03-01/streamingjobs/stop). Para eliminar un trabajo mediante la API de REST, consulte el método [Delete](/rest/api/streamanalytics/2016-03-01/streamingjobs/delete).