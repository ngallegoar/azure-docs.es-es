---
title: Administración de metadatos de artefactos de la cuenta de integración
description: Adición u obtención de metadatos de artefactos de cuentas de integración de Azure Logic Apps con Enterprise Integration Pack
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 01/17/2019
ms.openlocfilehash: bc119f1ce8efb821781dabfb9dd259cc5c8d9c23
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74792479"
---
# <a name="manage-artifact-metadata-in-integration-accounts-with-azure-logic-apps-and-enterprise-integration-pack"></a>Adición u obtención de metadatos de artefactos de cuentas de integración de Azure Logic Apps con Enterprise Integration Pack

Puede definir metadatos personalizados para artefactos en cuentas de integración y obtener metadatos durante el tiempo de ejecución para que los use su aplicación lógica. Por ejemplo, puede proporcionar metadatos para artefactos como asociados, acuerdos, esquemas y asignaciones: todos almacenan metadatos usando pares de clave-valor. 

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si aún no tiene una, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta de Azure gratuita</a>.

* Una [cuenta de integración](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) básica que tiene los artefactos donde desea agregar metadatos; por ejemplo: 

  * [Asociado](logic-apps-enterprise-integration-partners.md)
  * [Acuerdo](logic-apps-enterprise-integration-agreements.md)
  * [Esquema](logic-apps-enterprise-integration-schemas.md)
  * [Map](logic-apps-enterprise-integration-maps.md)

* Una aplicación lógica que está vinculada a los metadatos de artefacto y la cuenta de integración que desea utilizar. Si la aplicación lógica ya no está vinculada, obtenga información sobre [cómo vincular aplicaciones lógicas a cuentas de integración](logic-apps-enterprise-integration-create-integration-account.md#link-account). 

  Si aún no tiene una aplicación lógica, obtenga información sobre [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md). 
  Agregue el desencadenador y las acciones que desea usar para administrar los metadatos de artefactos. O bien, para simplemente probar, agregue un desencadenador como **Request** o **HTTP** a la aplicación lógica.

## <a name="add-metadata-to-artifacts"></a>Adición de metadatos a artefactos

1. Inicie sesión en <a href="https://portal.azure.com" target="_blank">Azure Portal</a> con sus credenciales de su cuenta de Azure. Busque y abra su cuenta de integración.

1. Seleccione el artefacto donde desea agregar los metadatos y elija **Editar**. Escriba los detalles de los metadatos para ese artefacto; por ejemplo:

   ![Introducción de los metadatos](media/logic-apps-enterprise-integration-metadata/add-partner-metadata.png)

1. Cuando termine, elija **Aceptar**.

1. Para ver estos metadatos en la definición de JavaScript Object Notation (JSON) para la cuenta de integración, elija **Editar como JSON** para que se abra el editor de JSON: 

   ![JSON para metadatos de asociados](media/logic-apps-enterprise-integration-metadata/partner-metadata.png)

## <a name="get-artifact-metadata"></a>Obtención de metadatos de artefacto

1. En Azure Portal, abra la aplicación lógica que está vinculada a la cuenta de integración que desea. 

1. En el Diseñador de aplicación lógica, si va a agregar el paso para obtener los metadatos en el desencadenador o la última acción del flujo de trabajo, elija **Nuevo paso** > **Agregar una acción**. 

1. En el cuadro de búsqueda, escriba "cuenta de integración". En el cuadro de búsqueda, elija **Todas**. En la lista de acciones, seleccione esta acción: **Búsqueda de artefactos de la cuenta de integración: cuenta de integración**

   ![Selección de "Búsqueda de artefactos de la cuenta de integración"](media/logic-apps-enterprise-integration-metadata/integration-account-artifact-lookup.png)

1. Proporcione esta información para el artefacto que desea buscar:

   | Propiedad | Obligatorio | Value | Descripción | 
   |----------|---------|-------|-------------| 
   | **Tipo de artefacto** | Sí | **Esquema**, **Asignación**, **Asociado**, **Acuerdo** o un tipo personalizado | El tipo del artefacto que desea | 
   | **Nombre del artefacto** | Sí | <*artifact-name*> | El nombre del artefacto que desea | 
   ||| 

   Por ejemplo, suponga que desea obtener los metadatos del artefacto de un socio comercial:

   ![Selección del tipo de artefacto y especificación del nombre del artefacto](media/logic-apps-enterprise-integration-metadata/artifact-lookup-information.png)

1. Agregue la acción que desee para controlar esos metadatos, por ejemplo:

   1. En la acción **Búsqueda de artefactos de la cuenta de integración**, elija **Siguiente paso** y seleccione **Agregar una acción**. 

   1. En el cuadro de búsqueda, escriba "http". En el cuadro de búsqueda, elija **Integraciones** y seleccione esta acción: **HTTP - HTTP**

      ![Adición de acción HTTP](media/logic-apps-enterprise-integration-metadata/http-action.png)

   1. Proporcione información para los metadatos de artefacto que desea administrar. 

      Por ejemplo, suponga que desea obtener los metadatos de `routingUrl` que se agregaron anteriormente en este tema. Estos son los valores de propiedad que tendría que especificar: 

      | Propiedad | Obligatorio | Value | Descripción | 
      |----------|----------|-------|-------------| 
      | **Método** | Sí | <*operation-to-run*> | La operación HTTP para ejecutar en el artefacto. Por ejemplo, esta acción HTTP utiliza el método **GET**. | 
      | **URI** | Sí | <*metadata-location*> | Para tener acceso al valor de metadatos de `routingUrl` del artefacto que ha recuperado, puede usar una expresión; por ejemplo: <p>`@{outputs('Integration_Account_Artifact_Lookup')['properties']['metadata']['routingUrl']}` | 
      | **Encabezados** | No | <*header-values*> | Cualquier salida de encabezado del desencadenador que desea pasar a la acción de HTTP. Por ejemplo, para pasar el valor de propiedad `headers` del desencadenador: puede utilizar una expresión, por ejemplo: <p>`@triggeroutputs()['headers']` | 
      | **Cuerpo** | No | <*body-content*> | Cualquier otro contenido que desee pasar a través de la propiedad `body` de la acción HTTP. En este ejemplo se pasan los valores `properties` del artefacto en la acción HTTP: <p>1. Haga clic en la propiedad **Body** para que aparezca la lista de contenido dinámico. Si no aparece ninguna propiedad, elija **Ver más**. <br>2. En la lista de contenido dinámico, en **Búsqueda de artefactos de la cuenta de integración**, seleccione **Propiedades**. | 
      |||| 

      Por ejemplo:

      ![Especificación de los valores y expresiones para una acción HTTP](media/logic-apps-enterprise-integration-metadata/add-http-action-values.png)

   1. Para comprobar la información proporcionada para la acción HTTP, vea la definición de JSON de la aplicación lógica. En la barra de herramientas del Diseñador de aplicación lógica, elija la **vista de código** para que aparezca la definición de JSON de la aplicación; por ejemplo:

      ![Definición de JSON de aplicación lógica](media/logic-apps-enterprise-integration-metadata/finished-logic-app-definition.png)

      Después de cambiar de nuevo al Diseñador de aplicación lógica, las expresiones que usó ahora aparecen resueltas, por ejemplo:

      ![Expresiones resueltas en el Diseñador de aplicación lógica](media/logic-apps-enterprise-integration-metadata/resolved-expressions.png)

## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre acuerdos](logic-apps-enterprise-integration-agreements.md)
