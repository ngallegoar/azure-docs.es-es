---
title: Incorporación de entidades para integraciones de B2B
description: Crear entidades en la cuenta de integración para usar con Azure Logic Apps
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 06/22/2019
ms.openlocfilehash: 8e3805fae5bf6cc5ad8cf759d3ba75220c6ddbd8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91565078"
---
# <a name="add-trading-partners-to-integration-accounts-for-azure-logic-apps"></a>Agregar entidades a la cuenta de integración para Azure Logic Apps

En [Azure Logic Apps](../logic-apps/logic-apps-overview.md), puede crear flujos de trabajo de integración automatizados de negocio a negocio (B2B) mediante un [cuenta de integración](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) con sus aplicaciones lógicas. Para representar a su organización y a otros usuarios, cree entidades como artefactos y agréguelos a la cuenta de integración. Los asociados son entidades que participan en transacciones B2B e intercambian mensajes entre sí.

Antes de crear a estos asociados, asegúrese de conversar y compartir información con sus asociados acerca de cómo identificar y validar los mensajes que el otro envía. Después de acordar estos detalles, estará listo para crear asociados en la cuenta de integración.

## <a name="partner-roles-in-integration-accounts"></a>Roles de asociado en la cuenta de integración

Para definir los detalles acerca de los mensajes intercambiados con los asociados, cree [acuerdos](../logic-apps/logic-apps-enterprise-integration-agreements.md) como artefactos y agréguelos a la cuenta de integración. Los acuerdos requieren al menos dos asociados en la cuenta de integración Su organización es siempre el *asociado host* en el acuerdo. La organización que intercambia mensajes con su organización es el *asociado invitado*. El asociado invitado puede ser otra compañía o incluso un departamento de su organización. Después de agregar a estos asociados, puede crear un contrato.

En un acuerdo, especifica los detalles para el tratamiento de mensajes entrantes y salientes desde la perspectiva del asociado host. Para los mensajes entrantes, la **Configuración de recepción** especifica de qué manera el asociado host recibe los mensajes del asociado invitado en el acuerdo. Para los mensajes salientes, la **Configuración de envío** especifica de qué manera el asociado host envía los mensajes al asociado invitado.

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si aún no tiene ninguna suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* Una [cuenta de integración](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) para almacenar los asociados, el acuerdo y otros artefactos B2B. Esta cuenta de integración debe estar asociada con una suscripción a Azure.

## <a name="create-partner"></a>Creación de un asociado

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. En el menú principal de Azure, seleccione **Todos los servicios**. En el cuadro de búsqueda, escriba "integración" y seleccione **Cuentas de integración**.

   ![Seleccione "Cuentas de integración".](./media/logic-apps-enterprise-integration-partners/find-integration-accounts.png)

1. En **Cuentas de integración**, seleccione la cuenta de integración en la que va a agregar los asociados.

   ![Seleccionar cuenta de integración](./media/logic-apps-enterprise-integration-partners/select-integration-account.png)

1. Elija el icono de **Asociados**.

   ![Captura de pantalla que muestra el icono Asociados.](./media/logic-apps-enterprise-integration-partners/choose-partners.png)

1. En **Asociados**, elija **Agregar**. En **Agregar asociado**, proporcione los detalles del asociado como se describe en la tabla siguiente.

   ![Elija "Agregar" y proporcione los detalles del asociado](./media/logic-apps-enterprise-integration-partners/add-partners.png)

   | Propiedad | Obligatorio | Descripción |
   |----------|----------|-------------|
   | **Nombre** | Sí | El nombre del asociado |
   | **Calificador** | Sí | El cuerpo de autenticación que proporciona identidades de negocio únicas a las organizaciones, por ejemplo, **D-U-N-S (Dun & Bradstreet)** . <p>Los asociados pueden optar por una identidad de negocio definida mutuamente. Para estos escenarios, seleccione **Definidos mutuamente** para EDIFACT o **Definidos mutuamente (X12)** para X12. <p>Para RosettaNet, seleccione solo **DUNS**, que es el estándar. |
   | **Valor** | Sí | Un valor que identifica los documentos que reciben sus aplicaciones lógicas. <p>Para RosettaNet, este valor debe ser un número de nueve dígitos que se corresponde con el número DUNS. |
   ||||

   > [!NOTE]
   > Para los asociados que usan RosettaNet, puede especificar información adicional creando primero estos asociados y [editándolos más adelante](#edit-partner).

1. Cuando termine, elija **Aceptar**.

   El nuevo asociado ahora aparece en la lista **Asociados**. Además, el icono **Asociados** actualiza el número actual de asociados.

   ![Nuevo asociado](./media/logic-apps-enterprise-integration-partners/new-partner.png)

<a name="edit-partner"></a>

## <a name="edit-partner"></a>Edición del asociado

1. En [Azure Portal](https://portal.azure.com), busque y seleccione la cuenta de integración.
Elija el icono de **Asociados**.

   ![Elección del icono "Asociados"](./media/logic-apps-enterprise-integration-partners/edit.png)

1. En **Asociados**, seleccione el asociado que quiera editar y, luego elija **Editar**. En **Editar**, haga los cambios.

   ![Haga los cambios y guárdelos](./media/logic-apps-enterprise-integration-partners/edit-partner.png)

   Para RosettaNet, bajo **Propiedades de asociado de RosettaNet**, puede especificar esta información adicional:

   | Propiedad | Obligatorio | Descripción |
   |----------|----------|-------------|
   | **Clasificación de asociados** | No | Tipo de organización del asociado |
   | **Código de la cadena de suministro** | No | Código de cadena de suministro del asociado, por ejemplo, "Tecnologías de la información" o "Componentes electrónicos" |
   | **Nombre de contacto** | No | Nombre de contacto del asociado |
   | **Correo electrónico** | No | Dirección de correo electrónico del asociado |
   | **Fax** | No | Número de fax del asociado |
   | **Teléfono** | No | Número de teléfono del asociado |
   ||||

1. Cuando haya terminado, elija **Aceptar** para guardar los cambios.

## <a name="delete-partner"></a>Eliminación del asociado

1. En [Azure Portal](https://portal.azure.com), busque y seleccione la cuenta de integración. Elija el icono de **Asociados**.

   ![Captura de pantalla que muestra el icono Asociados que se selecciona cuando quiere eliminar un asociado.](./media/logic-apps-enterprise-integration-partners/choose-partners-to-delete.png)

1. En **Asociados**, seleccione el asociado que desee eliminar. Elija **Eliminar**.

   ![Eliminación del asociado](./media/logic-apps-enterprise-integration-partners/delete-partner.png)

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre [acuerdos](../logic-apps/logic-apps-enterprise-integration-agreements.md)