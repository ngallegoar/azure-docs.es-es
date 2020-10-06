---
title: Novedades de Custom Vision
titleSuffix: Azure Cognitive Services
description: Este artículo describe las novedades de Custom Vision.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: overview
ms.date: 06/29/2020
ms.author: pafarley
ms.openlocfilehash: 019a9264beddafbd4585810967551e064592e94d
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2020
ms.locfileid: "90602530"
---
# <a name="whats-new-in-custom-vision"></a>Novedades de Custom Vision

Conozca las novedades del servicio. Estos elementos pueden ser notas de la versión, vídeos, entradas de blogs y otros tipos de información. Marque esta página para mantenerse actualizado con el servicio.

## <a name="july-2020"></a>Julio de 2020

### <a name="role-based-access-control"></a>Control de acceso basado en rol

* Custom Vision admite el control de acceso basado en roles de Azure (Azure RBAC), un sistema de autorización que permite administrar el acceso individual a los recursos de Azure. Para aprender a administrar el acceso a los proyectos de Custom Vision, consulte [Control de acceso basado en roles](./role-based-access-control.md).

### <a name="subset-training"></a>Entrenamiento de subconjuntos

* Al entrenar un proyecto de detección de objetos, puede entrenar opcionalmente en solo un subconjunto de las etiquetas aplicadas. Es posible que desee hacer esto si aún no ha aplicado suficientes etiquetas de un tipo determinado, pero tiene bastantes de las otras. Siga el [inicio rápido de la biblioteca cliente](./quickstarts/object-detection.md) para C# o Python para más información.

### <a name="azure-storage-notifications"></a>Notificaciones de almacenamiento de Azure

* Puede integrar el proyecto de Custom Vision con una cola de almacenamiento de blobs de Azure para obtener notificaciones de envío de actividad de entrenamiento/exportación del proyecto y copias de seguridad de los modelos publicados. Esta característica resulta útil para evitar el sondeo continuo del servicio en busca de resultados cuando se ejecutan operaciones largas. En su lugar, puede integrar las notificaciones de la cola de almacenamiento en el flujo de trabajo. Para más información, consulte la guía de [integración del almacenamiento](./storage-integration.md).

### <a name="copy-and-move-projects"></a>Copia y traslado de proyectos

* Ahora puede copiar proyectos de una cuenta de Custom Vision en otras. Puede que desee trasladar un proyecto desde un entorno de desarrollo a un entorno de producción, o realizar una copia de seguridad de un proyecto en una cuenta de otra región de Azure para aumentar la seguridad de los datos. Para más información, consulte la guía [Copia y traslado de proyectos](./copy-move-projects.md).

## <a name="september-2019"></a>Septiembre de 2019

### <a name="suggested-tags"></a>Etiquetas sugeridas

* La herramienta Smart Labeler del [sitio web de Custom Vision](https://www.customvision.ai/) genera etiquetas sugeridas para las imágenes de entrenamiento. Esto le permite etiquetar un gran número de imágenes más rápidamente al entrenar un modelo de Custom Vision. Para obtener instrucciones sobre cómo usar esta característica, consulte [Suggested tags](./suggested-tags.md) (Etiquetas sugeridas).

## <a name="cognitive-service-updates"></a>Actualizaciones de Cognitive Service

[Anuncios de actualización de Azure para Cognitive Services](https://azure.microsoft.com/updates/?product=cognitive-services)