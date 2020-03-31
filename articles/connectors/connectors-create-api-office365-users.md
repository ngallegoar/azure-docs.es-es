---
title: Conexión con Usuarios de Office 365
description: Automatización de tareas y flujos de trabajo que obtienen y administran perfiles en perfiles de Usuarios de Office 365 mediante Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.date: 08/18/2016
ms.topic: article
tags: connectors
ms.openlocfilehash: 179c7538d8072f0e1a4f7d049cf521971efd7a21
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75666863"
---
# <a name="get-and-manage-profiles-in-office-365-users-by-using-azure-logic-apps"></a>Obtención y administración de perfiles en Usuarios de Office 365 mediante Azure Logic Apps

Conéctese a Usuarios de Office 365 para obtener perfiles, buscar usuarios, etc. Con Usuarios de Office 365, puede:

* Compilar el flujo de su negocio en función de los datos que obtiene de Usuarios de Office 365. 
* Usar acciones que obtienen informes directos, obtienen el perfil de usuario de un administrador, etc. Estas acciones obtienen una respuesta y luego dejan el resultado a disposición de otras acciones. Por ejemplo, obtenga informes directos de la persona y, luego, tome esta información y actualice una base de datos de SQL Azure. 

Puede empezar creando una aplicación lógica ahora. Para ello, consulte [Creación de una aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-office-365-users"></a>Creación de una conexión a Usuarios de Office 365

Al agregar este conector a las aplicaciones lógicas, debe iniciar sesión en su cuenta de Usuarios de Office 365 para que Azure Logic Apps pueda conectarse a su cuenta.

> [!INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]
> 
> 

Después de crear la conexión, especifique las propiedades de Usuarios de Office 365, como el identificador de usuario. En la **referencia de la API REST** de este artículo se describen estas propiedades.

## <a name="connector-specific-details"></a>Detalles específicos del conector

Para obtener información técnica acerca de los desencadenadores, las acciones y los límites, que se detallan en la descripción de Swagger del conector, consulte la [página de referencia del conector](/connectors/officeusers/).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](apis-list.md)