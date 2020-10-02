---
title: 'Visualización o eliminación de los datos: Custom Vision Service'
titleSuffix: Azure Cognitive Services
description: Mantiene el control total sobre sus datos. En este artículo se explica cómo puede ver, exportar o eliminar sus datos en Custom Vision Service.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: pafarley
ms.openlocfilehash: fe17fa4349085be47201974b418493fe8d912ece
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2020
ms.locfileid: "90527401"
---
# <a name="view-or-delete-user-data-in-custom-vision"></a>Visualización o eliminación de los datos del usuario en Custom Vision

Custom Vision recopila los datos del usuario para prestar el servicio, pero los clientes tienen control total sobre la visualización y la eliminación de sus datos mediante [API Training](https://go.microsoft.com/fwlink/?linkid=865446) de Custom Vision.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Para aprender a ver y eliminar datos del usuario en Custom Vision, consulte la siguiente tabla.

| data | Operación de visualización | Operación de eliminación |
| ---- | ---------------- | ---------------- |
| Información de cuenta (claves de suscripción) | [GetAccountInfo](https://go.microsoft.com/fwlink/?linkid=865446) | Eliminación mediante Azure Portal (suscripciones de Azure). O bien, mediante el botón "Eliminar cuenta" en la página de configuración de CustomVision.ai (suscripciones de la cuenta de Microsoft). | 
| Detalles de la iteración | [GetIteration](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Detalles de rendimiento de la iteración | [GetIterationPerformance](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Lista de iteraciones | [GetIterations](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Proyectos y detalles del proyecto | [GetProject](https://go.microsoft.com/fwlink/?linkid=865446) y [GetProjects](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteProject](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Etiquetas de imagen | [GetTag](https://go.microsoft.com/fwlink/?linkid=865446) y [GetTags](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteTag](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Imágenes | [GetTaggedImages](https://go.microsoft.com/fwlink/?linkid=865446) (proporciona el URI para la descarga de la imagen) y [GetUntaggedImages](https://go.microsoft.com/fwlink/?linkid=865446) (proporciona el URI para la descarga de la imagen) | [DeleteImages](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Iteraciones exportadas | [GetExports](https://go.microsoft.com/fwlink/?linkid=865446) | Eliminación tras la eliminación de la cuenta |
