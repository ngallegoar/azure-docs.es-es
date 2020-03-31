---
title: Acceder, supervisar y administrar el repositorio de GitHub
description: Supervise los eventos de GitHub y administre el repositorio de GitHub mediante la creación de flujos de trabajo automatizados con Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/02/2018
tags: connectors
ms.openlocfilehash: b67d2e860e8c69f6f2f43441144ef2f60ed3a104
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75378456"
---
# <a name="monitor-and-manage-your-github-repo-by-using-azure-logic-apps"></a>Supervisar y administrar el repositorio de GitHub mediante Azure Logic Apps

GitHub es un servicio de almacenaje de repositorios Git basado en web. Ofrece toda la funcionalidad de control de revisión distribuido y de administración de código fuente (SCM) en Git, además de otras características.

Para empezar a trabajar con el conector de GitHub, [cree primero una aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-github"></a>Creación de una conexión a GitHub

Para usar el conector de GitHub en una aplicación de lógica, debe crear primero una *conexión* y luego especificar los detalles de estas propiedades: 

| Propiedad | Obligatorio | Descripción | 
| -------- | -------- | ----------- | 
| Token | Sí | Proporcione las credenciales de GitHub. |

Después de crear la conexión, puede ejecutar las acciones y escuchar los desencadenadores descritos en este artículo.

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-reference"></a>Referencia de conectores

Para obtener información técnica acerca de los desencadenadores, las acciones y los límites, que se detallan en la descripción de OpenAPI (antes Swagger) del conector, consulte la [página de referencia de los conectores](/connectors/github/).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)