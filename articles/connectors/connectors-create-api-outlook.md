---
title: Conectarse a Outlook.com
description: Automatización de tareas y flujos de trabajo que administran el correo electrónico, los calendarios y los contactos en Outlook.com con Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/18/2016
tags: connectors
ms.openlocfilehash: 8d3b180b6f1e9dc4ec4b09dd81786cc81e8588da
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75707193"
---
# <a name="manage-email-calendars-and-contacts-in-outlookcom-by-using-azure-logic-apps"></a>Administración del correo electrónico, los calendarios y los contactos en Outlook.com con Azure Logic Apps

Con [Azure Logic Apps](../logic-apps/logic-apps-overview.md) y el [conector de Outlook.com](/connectors/outlook/), puede crear tareas y flujos de trabajo automatizados que administren su cuenta de @outlook.com o @hotmail.com mediante la compilación de aplicaciones lógicas. Por ejemplo, puede automatizar estas tareas:

* Recibir, enviar y responder mensajes de correo electrónico.
* Programar reuniones en el calendario.
* Agregar y editar contactos.

Puede usar cualquier desencadenador para iniciar el flujo de trabajo (por ejemplo, cuando llega un nuevo correo electrónico, cuando se actualiza un elemento del calendario o cuando se produce un evento en un servicio diferencial). Puede usar acciones que respondan al evento desencadenador, como enviar un correo electrónico o crear un nuevo evento de calendario.

> [!NOTE]
> Para automatizar las tareas de una cuenta profesional de Microsoft como @fabrikam.onmicrosoft.com, use el [conector de Office 365 Outlook](../connectors/connectors-create-api-office365-outlook.md).

## <a name="prerequisites"></a>Prerrequisitos

* Una [cuenta de Outlook.com](https://outlook.live.com/owa/)

* Suscripción a Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/). 

* La aplicación lógica donde quiere acceder a su cuenta de Outlook.com. Para iniciar el flujo de trabajo con un desencadenador de Outlook.com, necesita una [aplicación lógica en blanco](../logic-apps/quickstart-create-first-logic-app-workflow.md). Para agregar una acción Outlook.com al flujo de trabajo, la aplicación lógica ya debe tener un desencadenador.

## <a name="add-a-trigger"></a>Incorporación de un desencadenador

Un [desencadenador](../logic-apps/logic-apps-overview.md#logic-app-concepts) es un evento que inicia el flujo de trabajo en la aplicación lógica. Esta aplicación lógica de ejemplo usa un desencadenador de "sondeo" que comprueba cualquier correo electrónico nuevo en la cuenta de correo electrónico, según el intervalo y la frecuencia especificados.

1. En [Azure Portal](https://portal.azure.com), abra la aplicación lógica en blanco en el Diseñador de aplicación lógica.

1. En el cuadro de búsqueda, escriba "outlook.com" como filtro. Para este ejemplo, seleccione **Cuando llega un nuevo correo electrónico**.

1. Si se le pide que inicie sesión, proporcione sus credenciales de Outlook.com para que la aplicación lógica pueda conectarse a la cuenta. O bien, si la conexión ya existe, especifique la información de las propiedades del desencadenador:

1. En el desencadenador, establezca los valores de **Frecuencia** e **Intervalo**.

   Por ejemplo, si desea que el desencadenador sondee cada 15 minutos, establezca el valor de **Frecuencia** en **Minuto** y el de **Intervalo** en **15**.

1. En la barra de herramientas del diseñador, seleccione **Guardar** para guardar la aplicación lógica.

Para responder al desencadenador, agregue otra acción. Por ejemplo, puede agregar la acción **Enviar mensaje** de Twilio, que envía un mensaje de texto cuando llega un correo electrónico.

## <a name="add-an-action"></a>Agregar una acción

Una [acción](../logic-apps/logic-apps-overview.md#logic-app-concepts) es una operación que se ejecuta mediante el flujo de trabajo de la aplicación lógica. Esta aplicación lógica de ejemplo envía un correo electrónico desde su cuenta de Outlook.com. Puede utilizar la salida de otro desencadenador para rellenar la acción. Por ejemplo, supongamos que la aplicación lógica utiliza el desencadenador **Cuando se crea un objeto** de SalesForce. Puede agregar la acción **Enviar un correo electrónico** de Outlook.com y usar las salidas del desencadenador de SalesForce en el correo electrónico.

1. En [Azure Portal](https://portal.azure.com), abra la aplicación lógica en Diseñador de aplicación lógica.

1. Para agregar una acción como último paso del flujo de trabajo, seleccione **Nuevo paso**. 

   Para agregar una acción entre un paso y otro, mueva el puntero por encima de la flecha entre ellos. Seleccione el signo más ( **+** ) que aparece y, luego, seleccione **Agregar una acción**.

1. En el cuadro de búsqueda, escriba "outlook.com" como filtro. Para este ejemplo, seleccione **Enviar un correo electrónico**. 

1. Si se le pide que inicie sesión, proporcione sus credenciales de Outlook.com para que la aplicación lógica pueda conectarse a la cuenta. O bien, si la conexión ya existe, especifique la información de las propiedades de la acción.

1. En la barra de herramientas del diseñador, seleccione **Guardar** para guardar la aplicación lógica.

## <a name="connector-reference"></a>Referencia de conectores

Para obtener datos técnica, como los desencadenadores, las acciones y los límites, tal como lo describe el archivo Swagger del conector, consulte la [página de referencia del conector](/connectors/outlook/). 

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)
