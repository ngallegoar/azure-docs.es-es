---
title: 'Solución de problemas del SDK de dispositivos de voz: servicio de voz'
titleSuffix: Azure Cognitive Services
description: En este artículo se proporciona información para ayudarlo a resolver los problemas que pueden surgir al usar el SDK de servicios de Voz.
services: cognitive-services
author: mswellsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: wellsi
ms.openlocfilehash: c590e0972de289a36890a75b220eddbded701ea8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "74815568"
---
# <a name="troubleshoot-the-speech-devices-sdk"></a>Solución de problemas de Speech Devices SDK

En este artículo se proporciona información para ayudarlo a resolver los problemas que pueden surgir al usar el SDK de servicios de Voz.

## <a name="certificate-failures"></a>Errores de certificado

Si recibe errores de certificado cuando use el servicio de voz, asegúrese de que el dispositivo tiene la fecha y hora correctas:

1. Vaya a **Configuración**. En **System** (Sistema), seleccione **Date & time** (Fecha y hora).

    ![En Settings (Configuración), seleccione Date & time (Fecha y hora).](media/speech-devices-sdk/qsg-12.png)

1. Mantenga la opción **Automatic date & time** (Fecha y hora automáticas) seleccionada. En **Select time zone** (Seleccionar zona horaria), seleccione la zona horaria actual.

    ![Seleccione las opciones de fecha y zona horaria](media/speech-devices-sdk/qsg-13.png)

    Cuando vea que la hora del kit desarrollo coincide con la hora del equipo, sabrá que el kit de desarrollo está conectado a Internet.

## <a name="next-steps"></a>Pasos siguientes

* [Revise las notas de la versión.](devices-sdk-release-notes.md)
