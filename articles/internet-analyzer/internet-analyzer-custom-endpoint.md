---
title: Creación de un punto de conexión personalizado | Microsoft Docs
description: En este artículo, aprenderá a configurar un punto de conexión personalizado que se mida con un recurso de Internet Analyzer.
services: internet-analyzer
author: mattcalder
ms.service: internet-analyzer
ms.topic: quickstart
ms.date: 10/16/2019
ms.author: mebeatty
ms.openlocfilehash: 0b5647dd2e43c8a73bb999af559a579c14bb86a1
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83683980"
---
# <a name="measure-custom-endpoints-to-evaluate-in-your-internet-analyzer-tests"></a>Los puntos de conexión personalizados se miden para realizar la evaluación en las pruebas de Internet Analyzer 

En este artículo se muestra cómo configurar un punto de conexión personalizado de medida como parte de las pruebas de Internet Analyzer. Los puntos de conexión personalizados ayudan a evaluar cargas de trabajo locales, cargas de trabajo que se ejecutan en otros proveedores en la nube y configuraciones personalizadas de Azure.  La comparación de dos puntos de conexión personalizados en una sola prueba es posible si uno de ellos es un recurso de Azure. Para más información acerca de Internet Analyzer, consulte la [información general](internet-analyzer-overview.md). 

> [!IMPORTANT]
> Esta versión preliminar pública se proporciona sin un acuerdo de nivel de servicio y no debe usarse para cargas de trabajo de producción. Puede que algunas características no se admitan, que tengan funcionalidades limitadas o que no estén disponibles en todas las ubicaciones de Azure. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Antes de empezar

Asegúrese de configurar un recurso de Internet Analyzer y seleccione la opción "Custom Endpoint" (Punto de conexión personalizado). Internet Analyzer supone que se puede acceder al punto de conexión personalizado desde Internet. Para más información, consulte [Creación de recursos en Internet Analyzer](internet-analyzer-create-test-portal.md).


## <a name="create-custom-endpoint"></a>Creación de puntos de conexión personalizados

1. Descargue [aquí](https://fpc.msedge.net/apc/trans.gif) una imagen de prueba transparente de un solo píxel. Esta imagen de un solo píxel es el recurso que el cliente JavaScript capturará para medir el rendimiento.
2. En su aplicación web personalizada, implemente la imagen de prueba en una ruta de acceso a la que se pueda acceder de forma pública. La ruta de acceso debe funcionar sobre HTTPS. 
3. Copie la dirección URL completa del punto de conexión personalizado (por ejemplo, `https://contoso.com/test/trans.gif`) en el campo del punto de conexión personalizado al crear la prueba.

## <a name="next-steps"></a>Pasos siguientes

Consulte [Preguntas frecuentes sobre Internet Analyzer](internet-analyzer-faq.md)

