---
title: Mensajes XML y archivos planos
description: Procesamiento, validación y transformación de los mensajes XML en Azure Logic Apps con Enterprise Integration Pack
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 02/27/2017
ms.openlocfilehash: 3cb3472572abbd891f8d36ea036b371c1224f38c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74792150"
---
# <a name="xml-messages-and-flat-files-in-azure-logic-apps-with-enterprise-integration-pack"></a>Mensajes XML y archivos planos en Azure Logic Apps con Enterprise Integration Pack

En [Azure Logic Apps](logic-apps-overview.md) puede procesar los mensajes XML que envíe y reciba con Enterprise Integration Pack. Si ha usado BizTalk Server, Enterprise Integration Pack ofrece funcionalidades similares para transformar y validar los mensajes, trabajar con archivos planos e incluso usar XPath para enriquecer o extraer propiedades específicas de un mensaje. Si no conoce nada de este espacio, estas características expanden la forma de procesar los mensajes dentro del flujo de trabajo de la aplicación lógica. Por ejemplo, si se encuentra en un escenario de negocio a negocio (B2B) y trabaja con esquemas XML específicos, puede usar Enterprise Integration Pack para mejorar cómo su empresa procesa estos mensajes.

Por ejemplo, Enterprise Integration Pack incluye estas funcionalidades:

* [Validación XML](logic-apps-enterprise-integration-xml-validation.md): permite validar un mensaje XML entrante o saliente con respecto a un esquema específico.

* [Transformación XML](logic-apps-enterprise-integration-transform.md): permite convertir o personalizar un mensaje XML en función de sus requisitos o de los de un asociado mediante asignaciones.

* [Codificación y descodificación de archivos planos](logic-apps-enterprise-integration-flatfile.md): permite codificar o descodificar un archivo plano.

  Por ejemplo, SAP acepta y envía archivos IDOC en formato de archivo plano. Muchas plataformas de integración crean mensajes XML, incluida Logic Apps. Por lo tanto, puede crear una aplicación lógica que use el codificador de archivo plano para "convertir" archivos XML en archivos planos.

* [XPath](workflow-definition-language-functions-reference.md#xpath): permite enriquecer un mensaje y extraer propiedades específicas de él. Las propiedades extraídas se pueden utilizar después para redirigir el mensaje a un destino o un punto de conexión intermediario.

## <a name="sample"></a>Muestra

[Implemente una aplicación lógica totalmente operativa](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-veter-pipeline) (ejemplo de GitHub) con las características XML de Azure Logic Apps.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información acerca de [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md)
