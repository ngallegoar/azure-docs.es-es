---
title: Conexión a Dynamics 365
description: Creación y administración de registros de Dynamics 365 mediante Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: jdaly, logicappspm
ms.topic: conceptual
ms.date: 05/09/2020
tags: connectors
ms.openlocfilehash: 00bf8ea2b783e09711a95f203bdfcce0e6b90b2c
ms.sourcegitcommit: 309a9d26f94ab775673fd4c9a0ffc6caa571f598
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2020
ms.locfileid: "82994299"
---
# <a name="create-and-manage-records-in-dynamics-365-by-using-azure-logic-apps"></a>Creación y administración de registros en Dynamics 365 mediante Azure Logic Apps

Dynamics 365 usa [Common Data Service](https://docs.microsoft.com/powerapps/maker/common-data-service/data-platform-intro). Para las conexiones a Dynamics 365, use el [conector de Common Data Service](https://docs.microsoft.com/connectors/commondataservice/).

> [!IMPORTANT]
> El [conector de Dynamics 365](https://docs.microsoft.com/connectors/dynamicscrmonline/) está en desuso, pero seguirá funcionando hasta que su eliminación. No use el conector de Dynamics 365 para las nuevas aplicaciones lógicas. Todavía no se ha anunciado ninguna escala de tiempo para la eliminación del conector de Dynamics 365. No es necesario convertir las aplicaciones lógicas existentes en el conector de Common Data Service ni en otro conector nuevo planeado, pero debe convertir las aplicaciones lógicas cuando se quite el conector. Para obtener más información, consulte [El conector de Dynamics 365 está en desuso](https://docs.microsoft.com/power-platform/important-changes-coming).
>
> El [conector de Common Data Service](https://docs.microsoft.com/connectors/commondataservice/) proporciona las mismas funcionalidades que el conector de Dynamics 365 en desuso, pero incluye mejoras que aumentan la confiabilidad. Para obtener información sobre el uso del conector de Common Data Service en Logic Apps, consulte [Creación y administración de registros de Common Data Service con Azure Logic Apps](../connectors/connect-common-data-service.md).

Para más información sobre Common Data Service, consulte estos temas:

* [Learn: Introducción a Common Data Service](https://docs.microsoft.com/learn/modules/get-started-with-powerapps-common-data-service/)
* [Learn: Conectar y analizar sus datos de Dynamics 365 mediante Power Platform y Common Data Service](https://docs.microsoft.com/learn/wwl/connect-analyze-dynamics-365-data/)