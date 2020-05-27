---
title: Uso de una plantilla de Azure Resource Manager para implementar el servicio
titleSuffix: Azure Cognitive Search
description: Puede implementar rápidamente una instancia del servicio Azure Cognitive Search mediante la plantilla de Azure Resource Manager.
manager: nitinme
author: tchristiani
ms.author: terrychr
ms.service: cognitive-search
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 03/20/2020
ms.openlocfilehash: 670cebe1c0f1c9002e33b729d0db9ee9f9a01283
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83682534"
---
# <a name="quickstart-deploy-cognitive-search-using-a-resource-manager-template"></a>Inicio rápido: Implementación de Cognitive Search con una plantilla de Resource Manager

Este artículo le guiará por el proceso de uso de una plantilla de Resource Manager para implementar un recurso de Azure Cognitive Search en Azure Portal.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Prerrequisitos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-a-cognitive-search-service"></a>Creación de un servicio Cognitive Search

### <a name="review-the-template"></a>Revisión de la plantilla

La plantilla usada en este inicio rápido forma parte de las [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/101-azure-search-create/).

:::code language="json"source="~/quickstart-templates/101-azure-search-create/azuredeploy.json" range="1-86" highlight="4-50,70-85":::

El recurso de Azure definido en esta plantilla:

- [Microsoft. Search/searchServices](https://docs.microsoft.com/azure/templates/Microsoft.Search/2015-08-19/searchServices): creación de un servicio Azure Cognitive Search

### <a name="deploy-the-template"></a>Implementación de la plantilla

Seleccione la imagen siguiente para iniciar sesión en Azure y abrir una plantilla. La plantilla crea un recurso de Azure Cognitive Search.

[![Implementación en Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2F101-azure-search-create%2Fazuredeploy.json)

El portal muestra un formulario que le permite proporcionar fácilmente los valores de parámetros. Algunos parámetros se rellenan previamente con los valores predeterminados de la plantilla. Tendrá que proporcionar la suscripción, el grupo de recursos, la ubicación y el nombre del servicio. Si desea usar Cognitive Services en una canalización de [enriquecimiento con IA](https://docs.microsoft.com/azure/search/cognitive-search-concept-intro), por ejemplo, para analizar archivos de imagen binarios para el texto, elija una ubicación que ofrezca Cognitive Search y Cognitive Services. Ambos servicios deben estar en la misma región para cargas de trabajo de enriquecimiento con inteligencia artificial. Una vez que haya completado el formulario, deberá aceptar los términos y condiciones y, a continuación, seleccionar el botón de compra para completar la implementación.

> [!div class="mx-imgBorder"]
> ![Visualización de la plantilla de Azure Portal](./media/search-get-started-arm/arm-portalscrnsht.png)

## <a name="review-deployed-resources"></a>Revisión de los recursos implementados

Una vez completada la implementación, puede tener acceso al nuevo grupo de recursos y al nuevo servicio de búsqueda en el portal.

## <a name="clean-up-resources"></a>Limpieza de recursos

Otros artículos de inicio rápido y tutoriales de Cognitive Search se basan este. Si tiene pensado seguir trabajando en otros artículos de inicio rápido y tutoriales, considere la posibilidad de dejar este recurso activo. Cuando ya no lo necesite, elimine el grupo de recursos; de este modo, se eliminarán también el servicio Cognitive Search y los recursos relacionados.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha creado un servicio Cognitive Search mediante una plantilla de Azure Resource Manager y ha validado la implementación. Para más información sobre Cognitive Search y Azure Resource Manager, continúe con los artículos siguientes.

 - Lea una [introducción a Azure Cognitive Search](https://docs.microsoft.com/azure/search/search-what-is-azure-search).
 - [Creación de un índice](https://docs.microsoft.com/azure/search/search-get-started-portal) para el servicio de búsqueda
 - [Creación de una aplicación de búsqueda](https://docs.microsoft.com/azure/search/search-create-app-portal) mediante asistente del portal
 - [Creación de un conjunto de aptitudes](https://docs.microsoft.com/azure/search/cognitive-search-quickstart-blob) para extraer información de los datos


