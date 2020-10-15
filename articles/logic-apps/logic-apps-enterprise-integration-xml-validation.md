---
title: Validación de documentos XML para la integración empresarial B2B
description: Validación XML mediante esquemas en Azure Logic Apps con Enterprise Integration Pack
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 10/21/2019
ms.openlocfilehash: ff21b059e712489c1914b2d12c6aa6a3d78d66d3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "74792167"
---
# <a name="validate-xml-for-b2b-enterprise-integration-in-azure-logic-apps-with-enterprise-integration-pack"></a>Validación XML para la integración empresarial B2B en Azure Logic Apps con Enterprise Integration Pack

A menudo, en los escenarios B2B las entidades de un acuerdo deben asegurarse de que los mensajes que intercambian son válidos para que pueda comenzar el procesamiento de datos. Con Enterprise Integration Pack puede validar documentos con un esquema predefinido mediante la acción de validación XML disponible.

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si aún no tiene suscripción, [regístrese para obtener una cuenta de Azure gratuita](https://azure.microsoft.com/free/).

* Una aplicación lógica en blanco o existente donde quiera usar la acción de validación XML. Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md) e [Inicio rápido: Creación de la primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Una [cuenta de integración](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) asociada a su suscripción de Azure, se vincula a la aplicación lógica donde vaya usar la acción de validación XML y contiene el esquema que desea usar para validar el contenido XML. Tanto la cuenta de integración como la aplicación de lógica deben existir en la misma ubicación o región de Azure.

## <a name="add-xml-validation-action"></a>Incorporación de la acción de validación XML

1. En [Azure Portal](https://portal.azure.com), abra la aplicación lógica en Diseñador de aplicación lógica.

1. Si tiene una aplicación lógica en blanco, en el Diseñador de aplicación lógica, en el cuadro de búsqueda, escriba `HTTP request` como filtro y seleccione el desencadenador **Cuando se recibe una solicitud HTTP**. De lo contrario, continúe con el paso siguiente.

1. En el último paso del flujo de trabajo, seleccione **Nuevo paso**.

   Para agregar una acción entre pasos existentes, mueva el puntero del mouse sobre la flecha que conecta esos pasos de manera que aparezca el signo más ( **+** ). Haga clic en el signo más y seleccione **Agregar una acción**.

1. En **Elegir una acción**, seleccione **Integrado**. En el cuadro de búsqueda, escriba `xml validation` como filtro. En la lista de acciones, seleccione **Validación XML**.

   ![Búsqueda y selección de la acción "Validación XML"](./media/logic-apps-enterprise-integration-xml-validation/select-xml-validation-action.png)

1. Para especificar el contenido XML que desea validar, haga clic dentro del cuadro **Contenido** para que aparezca la lista de contenido dinámico.

   ![Apertura de la lista de contenido dinámico](./media/logic-apps-enterprise-integration-xml-validation/open-dynamic-content-list.png)

   La lista de contenido dinámico muestra los tokens en propiedad que representan las salidas de los pasos anteriores en el flujo de trabajo. Si la lista no muestra una propiedad que se espera, compruebe el encabezado del desencadenador o la acción para ver si puede seleccionar **Ver más**.

1. En la lista de contenido dinámico, seleccione la propiedad con el contenido que desea validar.

   En este ejemplo se selecciona la salida **Cuerpo** del desencadenador.

   ![Selección del contenido que se va a validar](./media/logic-apps-enterprise-integration-xml-validation/select-content-to-validate.png)

1. Para especificar el esquema que desea usar para la validación, abra la lista **Nombre de esquema** y seleccione el esquema de validación que ha agregado a la cuenta de integración vinculada.

   ![Selección del esquema que se va a usar para la validación](./media/logic-apps-enterprise-integration-xml-validation/select-validation-schema.png)

1. Guarde la aplicación lógica.

   Ya ha terminado de configurar la validación. En una aplicación real, podría almacenar los datos validados en una aplicación de línea de negocio (LOB) como SalesForce. Para enviar la salida validada a Salesforce, agregue una acción.

1. Para probar la acción de validación, puede enviar una solicitud para desencadenar el flujo de trabajo de la aplicación lógica.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información acerca de [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md)