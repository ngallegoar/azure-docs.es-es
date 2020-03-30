---
title: 'Configuración del servicio: QnA Maker'
description: Comprenda cómo y dónde configurar los recursos.
ms.topic: reference
ms.date: 02/21/2020
ms.openlocfilehash: 8ef6fecbfeb119d0c68ec5bc3bbc90ec449dbb7d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77651806"
---
# <a name="service-configuration"></a>Configuración del servicio

QnA Maker usa varios recursos de Azure (servicios), como Cognitive Search, App Service, plan de App Service y Application Insights.

A continuación se enumeran todas las personalizaciones de esta configuración admitidas por QnA Maker.

## <a name="app-service"></a>App Service

QnA Maker usa una instancia de App Service para proporcionar el tiempo de ejecución de la consulta que usa [generateAnswer API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer).


Esta configuración está disponible en Azure Portal para la instancia de App Service. La configuración está disponible seleccionando **Configuración** y **Configuración**.

Puede establecer una configuración individual a través de la lista de configuración de la aplicación o modificar varias opciones de configuración seleccionando **Edición avanzada**.

|Resource|Configuración|
|--|--|
|AzureSearchAdminKey|Cognitive Search: se usa para el almacenamiento de conjuntos de QnA y clasificador n.º 1|
|AzureSearchName|Cognitive Search: se usa para el almacenamiento de conjuntos de QnA y clasificador n.º 1|
|DefaultAnswer|Texto de respuesta cuando no se encuentra ninguna coincidencia|
|UserAppInsightsAppId|Registro de chat y telemetría|
|UserAppInsightsKey|Registro de chat y telemetría|
|UserAppInsightsName|Registro de chat y telemetría|

Aprenda a [agregar el servicio Cognitive Search](./how-to/set-up-qnamaker-service-azure.md#configure-qna-maker-to-use-different-cognitive-search-resource) al servicio.

Debe **reiniciar** el servicio en la página **Información general** de Azure Portal, una vez que haya terminado de realizar los cambios.

## <a name="qna-maker-service"></a>Servicio QnA Maker

El servicio QnA Maker proporciona la configuración para que los siguientes usuarios colaboren en un único servicio QnA Maker y en todas sus bases de conocimiento.

Aprenda a [agregar colaboradores](./how-to/collaborate-knowledge-base.md) al servicio.

## <a name="application-insights"></a>Application Insights

Application Insights no tiene valores de configuración específicos de QnA Maker.

## <a name="app-service-plan"></a>Plan de servicio de aplicación

El plan de App Service no tiene valores de configuración específicos de QnA Maker.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre los [formatos](reference-document-format-guidelines.md) para los documentos y las direcciones URL que desea importar en una base de conocimiento.