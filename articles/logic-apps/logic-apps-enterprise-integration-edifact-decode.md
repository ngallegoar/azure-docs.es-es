---
title: Descodificación de mensajes EDIFACT
description: Validación de EDI y generación de confirmaciones con el descodificador de mensajes EDIFACT para Azure Logic Apps con Enterprise Integration Pack
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, divswa, logicappspm
ms.topic: article
ms.date: 04/22/2020
ms.openlocfilehash: b36641677dbf36402c7f578b9b1887c52f441afd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91322485"
---
# <a name="decode-edifact-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a>Descodificación de mensajes EDIFACT para Azure Logic Apps con Enterprise Integration Pack

Con el conector de descodificación de mensajes EDIFACT, puede validar propiedades EDI y específicas de asociados, dividir intercambios en transacciones o conservar intercambios completos y originar la confirmación de las transacciones procesadas. Para usar este conector, debe agregarlo a un desencadenador existente en la aplicación lógica.

## <a name="before-you-start"></a>Antes de comenzar

Esto es lo que necesita:

* Una cuenta de Azure; puede crear una [gratuita](https://azure.microsoft.com/free)
* Una [cuenta de integración](logic-apps-enterprise-integration-create-integration-account.md) que ya esté definida y asociada a su suscripción de Azure. Debe tener una cuenta de integración para usar el conector de descodificación de mensajes EDIFACT. 
* Al menos dos [asociados](logic-apps-enterprise-integration-partners.md) que ya estén definidos en su cuenta de integración.
* Un [contrato EDIFACT](logic-apps-enterprise-integration-edifact.md) ya definido en la cuenta de integración.

## <a name="decode-edifact-messages"></a>Descodificación de mensajes EDIFACT

> [!IMPORTANT]
> El conector EDIFACT solo admite caracteres UTF-8.
> Si la salida contiene caracteres inesperados, compruebe que los mensajes EDIFACT utilizan el juego de caracteres UTF-8. 

1. [Crear una aplicación lógica](quickstart-create-first-logic-app-workflow.md).

2. El conector de descodificación de mensajes EDIFACT no tiene desencadenadores, por lo que debe agregar uno para iniciar la aplicación lógica, por ejemplo, un desencadenador de solicitud. En el Diseñador de aplicaciones lógicas, agregue un desencadenador y una acción a la aplicación lógica.

3. En el cuadro de búsqueda, escriba "EDIFACT" para el filtro. Seleccione **Descodificar mensaje EDIFACT**.
   
    ![buscar EDIFACT](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage1.png)

3. Si no había creado ninguna conexión a la cuenta de integración, se le pedirá que lo haga ahora. Asigne un nombre a la conexión y seleccione la cuenta de integración que desee conectar.
   
    ![crear cuenta de integración](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage2.png)

    Aquellas propiedades con un asterisco son obligatorias.

    | Propiedad | Detalles |
    | --- | --- |
    | Nombre de la conexión * |Escriba cualquier nombre para la conexión. |
    | Cuenta de integración* |Escriba un nombre para la cuenta de integración. Asegúrese de que la cuenta de integración y la aplicación lógica se encuentren en la misma ubicación de Azure. |

4. Cuando haya acabado, para terminar de crear la conexión, elija **Crear**. Los detalles de la conexión deberían ser similares a los de este ejemplo:

    ![detalles de la cuenta de integración](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage3.png)  

5. Una vez creada la conexión, como se muestra en este ejemplo, seleccione el mensaje de archivo plano EDIFACT que desee descodificar.

    ![creación de la conexión de la cuenta de integración](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage4.png)  

    Por ejemplo:

    ![Selección del mensaje de archivo plano EDIFACT para descodificar](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage5.png)  

## <a name="edifact-decoder-details"></a>Detalles de descodificador de EDIFACT

El conector de descodificación EDIFACT lleva a cabo estas tareas: 

* Valida el sobre con el acuerdo entre socios comerciales.
* Resuelve el acuerdo, para lo que hace coincidir el calificador e identificador del remitente con el calificador e identificador del receptor.
* Divide un intercambio en varias transacciones cuando el intercambio tiene más de una transacción que se basa en la configuración de recepción del acuerdo.
* Desensambla el intercambio.
* Valida las propiedades de EDI y específicas del asociado, lo que incluye:
  * Validación de la estructura del sobre de intercambio
  * Validación del esquema del sobre con respecto al esquema de control
  * Validación del esquema de los elementos de datos del conjunto de transacciones con respecto al esquema de mensaje
  * Validación de EDI realizada en los elementos de datos del conjunto de transacciones
* Comprueba que los números de control de intercambio, grupo y conjunto de transacciones no están duplicados (si está configurado). 
  * Comprueba el número de control del intercambio en relación con los intercambios recibidos anteriormente. 
  * Comprueba el número de control del grupo en relación con otros números de control de grupo en el intercambio. 
  * Comprueba el número de control del conjunto de transacciones con otros números de control del conjunto de transacciones de dicho grupo.
* Divide el intercambio en conjuntos de transacciones o conserva todo el intercambio:
  * Dividir intercambio como conjuntos de transacciones: suspender conjuntos de transacciones en caso de error. Divide el intercambio en conjuntos de transacciones y analiza cada conjunto de transacciones. 
  La acción de descodificación de EDIFACT solo envía esos conjuntos de transacciones que no superan la validación a `badMessages` y envía el resto de conjuntos de transacciones a `goodMessages`.
  * Dividir intercambio como conjuntos de transacciones: suspender intercambio en caso de error. Divide el intercambio en conjuntos de transacciones y analiza cada conjunto de transacciones. 
  Si uno o varios conjuntos de transacciones del intercambio no superan la validación, la acción de descodificación de EDIFACT envía todos los conjuntos de transacciones de ese intercambio a `badMessages`.
  * Conservar intercambio: suspender conjuntos de transacciones en caso de error. Conserva el intercambio y procesa todo el intercambio por lotes. 
  La acción de descodificación de EDIFACT solo envía esos conjuntos de transacciones que no superan la validación a `badMessages` y envía el resto de conjuntos de transacciones a `goodMessages`.
  * Conservar intercambio: suspender intercambio en caso de error. Conserva el intercambio y procesa todo el intercambio por lotes. 
  Si uno o varios conjuntos de transacciones del intercambio no superan la validación, la acción de descodificación de EDIFACT envía todos los conjuntos de transacciones de ese intercambio a `badMessages`.
* Genera una confirmación técnica (control) o funcional (si esta opción está configurada).
  * Una confirmación técnica o ACK CONTRL informa de los resultados de una comprobación sintáctica de todo el intercambio recibido.
  * Una confirmación funcional confirma la aceptación o el rechazo de un intercambio recibido o un grupo

## <a name="view-swagger-file"></a>Ver el archivo de Swagger
Para ver los detalles de Swagger para el conector EDIFACT, consulte [EDIFACT](/connectors/edifact/).

## <a name="next-steps"></a>Pasos siguientes
[Más información acerca de Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Información sobre Enterprise Integration Pack") 

