---
title: Acuerdos entre socios comerciales
description: Cree y administre acuerdos entre socios comerciales con Azure Logic Apps y Enterprise Integration Pack
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 06/22/2019
ms.openlocfilehash: c8cbfb619c9eed325161503f705bf5c4c0746265
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "82612340"
---
# <a name="create-and-manage-trading-partner-agreements-in-azure-logic-apps"></a>Crear y administrar acuerdos entre socios comerciales en Azure Logic Apps

Un *acuerdo* entre 
[socios comerciales](../logic-apps/logic-apps-enterprise-integration-partners.md) ayuda a las organizaciones y empresas a comunicarse sin problemas entre sí al definir el protocolo estándar del sector específico que se usará al intercambiar mensajes de negocio a negocio (B2B). Los acuerdos proporcionan ventajas comunes, por ejemplo:

* Permiten a las organizaciones intercambiar información con un formato conocido.
* Mejoran la eficacia al realizar transacciones B2B.
* Son fáciles de crear, administrar y usar al crear aplicaciones de integración empresarial.

En este artículo se muestra cómo crear un acuerdo AS2, EDIFACT o X12 que puede usar al crear soluciones de integración empresarial para escenarios B2B con [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md) y [Azure Logic Apps](../logic-apps/logic-apps-overview.md). Después de crear un acuerdo, puede usar los conectores AS2, EDIFACT o X12 para intercambiar mensajes B2B.

Para crear acuerdos para intercambiar mensajes de RosettaNet, consulte [Intercambio de mensajes de RosettaNet](../logic-apps/logic-apps-enterprise-integration-rosettanet.md).

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si aún no tiene ninguna suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* Una [cuenta de integración](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) para almacenar el contrato y otros artefactos B2B. Esta cuenta de integración debe estar asociada con una suscripción a Azure.

* Al menos dos [socios comerciales](../logic-apps/logic-apps-enterprise-integration-partners.md) que ya haya definido en su cuenta de integración. Un acuerdo requiere un asociado host y un asociado invitado. Ambos asociados deben usar el mismo calificador de "identidad de negocio" que el acuerdo que desea crear, como AS2, X 12 o EDIFACT.

* Opcional: La aplicación lógica donde quiere usar el acuerdo y un desencadenador que inicie el flujo de trabajo de la aplicación lógica. Para crear su cuenta de integración y los artefactos B2B, no necesita una aplicación lógica. Sin embargo, antes de que la aplicación lógica pueda usar los artefactos B2B en su cuenta de integración, debe vincular la cuenta de integración con la aplicación lógica. Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md) e [Inicio rápido: Creación de la primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-agreements"></a>Crear acuerdos

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
En el menú principal de Azure, seleccione **Todos los servicios**. En el cuadro de búsqueda, escriba "integración" como filtro. En los resultados, seleccione este recurso: **Cuentas de integración**

   ![Búsqueda de la cuenta de integración](./media/logic-apps-enterprise-integration-agreements/find-integration-accounts.png)

1. En **Cuentas de integración**, seleccione la cuenta de integración donde quiere crear el acuerdo.

   ![Selección de la cuenta de integración donde desea crear el contrato](./media/logic-apps-enterprise-integration-agreements/select-integration-account.png)

1. En el panel derecho, bajo **Componentes**, elija el icono **Acuerdos**.

   ![Elija "Acuerdos"](./media/logic-apps-enterprise-integration-agreements/agreement-1.png)

1. En **Contratos**, elija **Agregar**. En el panel **Agregar**, proporcione información sobre su acuerdo, por ejemplo:

   ![Elección de "Agregar"](./media/logic-apps-enterprise-integration-agreements/agreement-2.png)

   | Propiedad | Obligatorio | Value | Descripción |
   |----------|----------|-------|-------------|
   | **Nombre** | Sí | <*agreement-name*> | Escriba el nombre del acuerdo. |
   | **Tipo de contrato** | Sí | **AS2**, **X12** o **EDIFACT** | El tipo de protocolo para el acuerdo. Al crear el archivo de contrato, el contenido del archivo debe coincidir con el tipo de contrato. | |  
   | **Asociado host** | Sí | <*host-partner-name*> | El asociado host representa la organización que especifica el acuerdo. |
   | **Identidad del host** | Sí | <*host-partner-identifier*> | El identificador del asociado host. |
   | **Asociado invitado** | Sí | <*guest-partner-name*> | El asociado invitado representa la organización que está haciendo negocios con el asociado del host. |
   | **Identidad del invitado** | Sí | <*guest-partner-identifier*> | El identificador del asociado invitado. |
   | **Configuración de recepción** | Varía | Varía | Estas propiedades especifican de qué manera el asociado host recibe todos los mensajes entrantes del asociado invitado en el acuerdo. Para más información, consulte el tipo de contrato correspondiente: <p>- [Configuración de mensajes AS2](../logic-apps/logic-apps-enterprise-integration-as2-message-settings.md) <br>- [Configuración de mensajes EDIFACT](logic-apps-enterprise-integration-edifact.md) <br>- [Configuración de mensajes X12](logic-apps-enterprise-integration-x12.md) |
   | **Configuración de envío** | Varía | Varía | Estas propiedades especifican de qué manera el asociado host envía todos los mensajes salientes al asociado invitado en el acuerdo. Para más información, consulte el tipo de contrato correspondiente: <p>- [Configuración de mensajes AS2](../logic-apps/logic-apps-enterprise-integration-as2-message-settings.md) <br>- [Configuración de mensajes EDIFACT](logic-apps-enterprise-integration-edifact.md) <br>- [Configuración de mensajes X12](logic-apps-enterprise-integration-x12.md) |
   |||||

   > [!IMPORTANT]
   > La resolución de un contrato depende de la coincidencia de estos elementos definidos en el asociado y el mensaje entrante:
   >
   > * El calificador y el identificador del remitente
   > * El calificador y el identificador del receptor
   >
   > Si estos valores cambian para el asociado, no olvide actualizar también el contrato.

1. Cuando haya terminado de crear el acuerdo, en la página **Agregar**, elija **Aceptar** y vuelva a la cuenta de integración.

   La lista **Acuerdos** ahora muestra el nuevo acuerdo.

## <a name="edit-agreements"></a>Editar acuerdos

1. En [Azure Portal](https://portal.azure.com), en el menú principal de Azure, seleccione **Todos los servicios**.

1. En el cuadro de búsqueda, escriba "integración" como filtro. En los resultados, seleccione este recurso: **Cuentas de integración**

1. En **Cuentas de integración**, seleccione la cuenta de integración donde se encuentra el acuerdo que quiere editar.

1. En el panel derecho, bajo **Componentes**, elija el icono **Acuerdos**.

1. En **Acuerdos**, seleccione el acuerdo y elija **Editar**.

1. Haga los cambios y luego guárdelos.

## <a name="delete-agreements"></a>Eliminar acuerdos

1. En [Azure Portal](https://portal.azure.com), en el menú principal de Azure, seleccione **Todos los servicios**.

1. En el cuadro de búsqueda, escriba "integración" como filtro. En los resultados, seleccione este recurso: **Cuentas de integración**

1. En **Cuentas de integración**, seleccione la cuenta de integración donde se encuentra el acuerdo que quiere eliminar.

1. En el panel derecho, bajo **Componentes**, elija el icono **Acuerdos**.

1. En **Acuerdos**, seleccione el acuerdo y elija **Eliminar**.

1. Confirme que desea eliminar el contrato seleccionado.

## <a name="next-steps"></a>Pasos siguientes

* [Intercambio de mensajes AS2](logic-apps-enterprise-integration-as2.md)
* [Intercambio de mensajes EDIFACT](logic-apps-enterprise-integration-edifact.md)
* [Intercambio de mensajes X12](logic-apps-enterprise-integration-x12.md)
