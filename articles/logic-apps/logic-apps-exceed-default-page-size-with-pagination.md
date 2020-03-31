---
title: Obtención de más elementos o registros con la paginación
description: Configuración de la paginación para superar el límite de tamaño de página predeterminado para las acciones de conector en Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 04/11/2019
ms.openlocfilehash: 75d9660eb35b5d7ddc644d177c11ae489e2853dc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74792113"
---
# <a name="get-more-data-items-or-records-by-using-pagination-in-azure-logic-apps"></a>Obtención de más datos, elementos o registros con la paginación en Azure Logic Apps

Al recuperar datos, elementos o registros mediante una acción de conector en [Azure Logic Apps](../logic-apps/logic-apps-overview.md), puede obtener conjuntos de resultados tan grandes que la acción no devuelva todos los resultados al mismo tiempo. Con algunas acciones, el número de resultados puede superar el tamaño de página predeterminado del conector. En este caso, la acción devolverá solo la primera página de resultados. Por ejemplo, el tamaño de página predeterminado para la acción **Obtener filas** del conector de SQL Server es 2048, pero puede ser diferente según la configuración.

Algunas acciones permiten activar una opción de *paginación* para que la aplicación lógica pueda recuperar más resultados hasta el límite de paginación, pero devuelve los resultados como un mensaje único cuando finaliza la acción. Cuando utilice la paginación, debe especificar un valor de *umbral*, que es el número de resultados objetivo que quiere que devuelva la acción. La acción recupera los resultados hasta alcanzar el umbral especificado. Cuando el número total de elementos es menor que el umbral especificado, la acción recupera todos los resultados.

Al activar la opción de paginación se recuperan las páginas de resultados según el tamaño de página de un conector. Este comportamiento significa que, en ocasiones, podría obtener más resultados que el umbral especificado. Por ejemplo, al usar la acción **Obtener filas** de SQL Server, que admite la opción de paginación:

* El tamaño de página predeterminado de la acción es de 2048 registros por página.
* Supongamos que tiene 10 000 registros y que especifica un mínimo de 5000 registros.
* La paginación obtiene páginas de registros, por lo que para obtener al menos el mínimo especificado, la acción devuelve 6144 registros a (3 páginas x 2048), no 5000 registros.

Esta es una lista con solo algunos de los conectores donde puede superar el tamaño de página predeterminado para acciones específicas:

* [Azure Blob Storage](https://docs.microsoft.com/connectors/azureblob/)
* [Dynamics 365](https://docs.microsoft.com/connectors/dynamicscrmonline/)
* [Excel](https://docs.microsoft.com/connectors/excel/)
* [HTTP](https://docs.microsoft.com/azure/connectors/connectors-native-http)
* [IBM DB2](https://docs.microsoft.com/connectors/db2/)
* [Microsoft Teams](https://docs.microsoft.com/connectors/teams/)
* [Base de datos de Oracle](https://docs.microsoft.com/connectors/oracle/)
* [Salesforce](https://docs.microsoft.com/connectors/salesforce/)
* [SharePoint](https://docs.microsoft.com/connectors/sharepointonline/)
* [SQL Server](https://docs.microsoft.com/connectors/sql/)

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si aún no tiene ninguna suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* La aplicación lógica y la acción donde quiere activar la paginación. Si no tiene una aplicación lógica, consulte [Inicio rápido: Creación de la primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="turn-on-pagination"></a>Activar la paginación

Para determinar si una acción admite la paginación en el Diseñador de aplicación lógica, compruebe la configuración de la acción para la opción de **paginación**. En este ejemplo se muestra cómo activar la paginación en la acción **Obtener filas** de SQL Server.

1. En la esquina superior derecha de la acción, elija el botón de puntos suspensivos ( **...** ) y, después, seleccione **Configuración**.

   ![Abrir la configuración de la acción](./media/logic-apps-exceed-default-page-size-with-pagination/sql-action-settings.png)

   Si la acción admite la paginación, la acción muestra la opción **Paginación**.

1. Cambie la opción **Paginación** de **Desactivado** a **Activado**. En la propiedad **Umbral**, especifique un valor entero para el número de resultados objetivo que quiere que devuelva la acción.

   ![Especificar el número mínimo de resultados que se devolverán](./media/logic-apps-exceed-default-page-size-with-pagination/sql-action-settings-pagination.png)

1. Cuando esté listo, elija **Hecho**.

## <a name="workflow-definition---pagination"></a>Definición de flujo de trabajo: paginación

Al activar la paginación para una acción que admite esta funcionalidad, la definición de flujo de trabajo de la aplicación lógica incluye la propiedad `"paginationPolicy"` junto con la propiedad `"minimumItemCount"` en la propiedad `"runtimeConfiguration"` de esa acción; por ejemplo:

```json
"actions": {
   "HTTP": {
      "inputs": {
         "method": "GET",
         "uri": "https://www.testuri.com"
      },
      "runAfter": {},
      "runtimeConfiguration": {
         "paginationPolicy": {
            "minimumItemCount": 1000
         }
      },
      "type": "Http"
   }
},
```

## <a name="get-support"></a>Obtención de soporte técnico

Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
