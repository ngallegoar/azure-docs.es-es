---
title: 'Información general: Azure sin servidor para soluciones y aplicaciones basadas en la nube'
description: Aprenda a crear aplicaciones y soluciones basadas en la nube sin preocuparse por la infraestructura mediante el uso de Azure Logic Apps y Azure Functions
services: logic-apps
ms.suite: integration
author: jeffhollan
ms.author: jehollan
ms.reviewer: klam, estfan, logicappspm
ms.topic: article
ms.date: 03/30/2017
ms.openlocfilehash: 0f20bb5fb249ad6bac862afe2b0e8eee4b32e2a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75666557"
---
# <a name="azure-serverless-overview-for-building-cloud-based-apps-and-solutions-with-azure-logic-apps-and-azure-functions"></a>Sin servidor de Azure: Información general para la creación de soluciones y aplicaciones basadas en la nube con Azure Logic Apps y Azure Functions

Las aplicaciones [sin servidor](https://azure.microsoft.com/solutions/serverless/) ofrecen ventajas tales como aumentar la velocidad de desarrollo, reducir el código, la simplicidad y la escala. En este artículo se tratan los diferentes atributos de las soluciones sin servidor y las ofertas de Azure sin servidor.

## <a name="what-is-serverless"></a>¿Qué significa sin servidor?

Sin servidor no significa que no haya ningún servidor, sino que los desarrolladores no tienen que preocuparse por los servidores. Gran parte del desarrollo tradicional de aplicaciones es dar respuesta a preguntas en torno a las soluciones de escalado, hospedaje y supervisión para satisfacer las demandas de la aplicación. Con la tecnología sin servidor, la respuesta a estas cuestiones forma parte de la solución. Además, las aplicaciones sin servidor se facturan según un plan basado en el consumo. Si la aplicación no se usa nunca, no se aplica ningún cargo. Estas características ayudan a los desarrolladores a centrarse únicamente en la lógica de negocios de una solución.

Los principales servicios de Azure para el concepto sin servidor son [Azure Functions](https://azure.microsoft.com/services/logic-apps/) y [Azure Logic Apps](https://azure.microsoft.com/services/functions/). Ambas soluciones siguen los principios descritos anteriormente y ayudan a los desarrolladores a crear sólidas aplicaciones en la nube con muy poco código.

## <a name="what-is-azure-logic-apps"></a>¿Qué es Azure Logic Apps?

[Azure Logic Apps](logic-apps-overview.md) ofrece una manera de simplificar e implementar flujos de trabajo e integraciones escalables en la nube. Este servicio proporciona un diseñador visual para modelar y automatizar el proceso en una serie de pasos denominada flujo de trabajo. Existen muchos [conectores](../connectors/apis-list.md) tanto en los servicio en la nube como en los sistemas locales que conectan rápidamente una aplicación sin servidor a otras API. Cada aplicación lógica comienza con un desencadenador, como "Cuando se agrega una cuenta a Dynamics CRM". Una vez que se activa el desencadenador, el flujo de trabajo puede ejecutar combinaciones de acciones, conversiones y lógica condicional. Logic Apps es una buena opción para orquestar diferentes instancias de Azure Functions en un proceso, en especial cuando el proceso requiere interacción con un sistema o una API externos.

Para empezar a trabajar con Logic Apps, comience por la [creación de su primera aplicación lógica](quickstart-create-first-logic-app-workflow.md). Para obtener información más técnica acerca de Logic Apps, consulte la [referencia para desarrolladores](logic-apps-workflow-definition-language.md).

## <a name="what-is-azure-functions"></a>¿Qué es Azure Functions?

Azure Functions es un servicio para ejecutar fácilmente fragmentos de código o "funciones" en la nube. Solo se puede escribir el código necesario para el problema actual, sin preocuparse de toda una aplicación ni de la infraestructura necesaria. Functions puede hacer que el desarrollo sea aún más productivo y, además, le permite utilizar el lenguaje de desarrollo que prefiera, como C#, F#, Node.js, Python o PHP. Solo se paga por el tiempo durante el que se ejecuta el código y Azure se escala según sea necesario.

Para empezar a usar Azure Functions, comience por consultar [Creación de su primera función en Azure](../azure-functions/functions-create-first-azure-function.md). Si busca información más técnica acerca de las Functions, consulte la [referencia para desarrolladores](../azure-functions/functions-reference.md).

## <a name="how-can-i-build-and-deploy-serverless-apps-in-azure"></a>¿Cómo puedo compilar e implementar aplicaciones sin servidor en Azure?

Azure proporciona un gran número de herramientas para desarrollar, implementar y administrar aplicaciones sin servidor. Puede compilar las aplicaciones directamente en Azure Portal con [herramientas de Visual Studio](logic-apps-serverless-get-started-vs.md) o [Visual Studio Code](quickstart-create-logic-apps-visual-studio-code.md). Después de compilar la aplicación, [puede implementarla rápidamente con las plantillas de Azure Resource Manager](logic-apps-deploy-azure-resource-manager-templates.md). Azure proporciona supervisión, a la que se puede acceder desde Azure Portal, la API o los SDK, o bien con herramientas integradas para los registros de Azure Monitor y Application Insights.

## <a name="next-steps"></a>Pasos siguientes

* [Compilación de una aplicación sin servidor en Visual Studio](logic-apps-serverless-get-started-vs.md)
* [Creación de un panel de streaming de Customer Insights con Azure Logic Apps y Azure Functions](logic-apps-scenario-social-serverless.md)
* [Automatización de la implementación de aplicaciones lógicas](logic-apps-azure-resource-manager-templates-overview.md)
