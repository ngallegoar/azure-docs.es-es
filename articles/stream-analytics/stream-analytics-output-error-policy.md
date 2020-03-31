---
title: Directivas de errores de salida en Azure Stream Analytics
description: Conozca más información sobre las directivas de control de errores de salida en Azure Stream Analytics.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 22112272bb302769e5969cf6995d486438deb41f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75431623"
---
# <a name="azure-stream-analytics-output-error-policy"></a>Directivas de errores de salida en Azure Stream Analytics
En este artículo se describen las directivas de control de errores de datos de salida que se pueden configurar en Azure Stream Analytics.

Las directivas de control de errores de datos de salida se aplican solo a los errores de conversión de datos que suceden cuando el evento de salida que produce un trabajo de Stream Analytics no cumple con el esquema del receptor de destino. Puede configurar esta directiva seleccionando **Reintentar** o **Anular**. En Azure Portal, cuando esté en un trabajo de Stream Analytics, en **Configurar**, seleccione **Directiva de error**.

![Ubicación de las directivas de errores de salida en Azure Stream Analytics](./media/stream-analytics-output-error-policy/stream-analytics-error-policy-locate.png)


## <a name="retry"></a>Reintento
Si se produce un error, Azure Stream Analytics volverá a intentar la escritura del evento indefinidamente hasta que esta se realice correctamente. No hay ningún tiempo de expiración para los reintentos. En última instancia, el evento que se está reintentando bloqueará todos los eventos posteriores del procesamiento. Esta opción es la directiva de control de errores de salida predeterminada.

## <a name="drop"></a>Anular
Azure Stream Analytics anulará todos los eventos de salida que produzcan un error de conversión de datos. No se pueden recuperar los eventos anulados para volver a procesarlos más adelante.


Todos los errores transitorios (por ejemplo, errores de red) se reintentan independientemente de la configuración de la directiva de control de errores de salida.


## <a name="next-steps"></a>Pasos siguientes
[Guía de solución de problemas de Azure Stream Analytics](stream-analytics-troubleshooting-guide.md)
