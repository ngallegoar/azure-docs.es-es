---
title: Punto de conexión HTTPS | Azure Marketplace
description: Configure la administración de clientes potenciales para un punto de conexión HTTPS.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/24/2018
ms.author: dsindona
ms.openlocfilehash: cb6ef173e97a7c2bbd7d7cad5e5074b1f2d0f066
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80288604"
---
# <a name="configure-lead-management-using-an-https-endpoint"></a>Configuración de la administración de clientes potenciales mediante un punto de conexión HTTPS

Puede usar un punto de conexión HTTPS para administrar clientes potenciales de AppSource y Azure Marketplace. Estos avisos de clientes potenciales se pueden escribir en un sistema de administración de relaciones con clientes (CRM) o enviarse como una notificación por correo electrónico. En este artículo se describe cómo configurar la administración de clientes potenciales mediante el servicio de automatización [Microsoft Flow](https://powerapps.microsoft.com/automate-processes/).

## <a name="create-a-flow-using-microsoft-flow"></a>Creación de un flujo mediante Microsoft Flow

1. Abra la página web de [Flow](https://flow.microsoft.com/). Seleccione **Iniciar sesión** o **Regístrese gratis** para crear una cuenta gratuita de Flow.

2. Inicie sesión y seleccione **Mis flujos** en la barra de menús.

    ![Mis flujos](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows.png)

3. Seleccione **+ Crear desde cero**.

    ![Crear desde cero](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-create-fromblank.png)

4. Seleccione **Crear desde cero**.

    ![Crear desde cero](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-create-fromblank2.png)

5. En el campo **Buscar conectores y desencadenadores**, escriba "solicitud" para encontrar el conector de la solicitud.
6. En **Desencadenadores**, seleccione **Cuando se recibe una solicitud HTTP**. 

    ![Seleccionar el desencadenador recibido de la solicitud HTTP](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-pick-request-trigger.png)

7. Utilice uno de los pasos siguientes para configurar el **esquema JSON del cuerpo de la solicitud**:

   - Copie el [esquema JSON](#json-schema) que está al final de este artículo en el cuadro de texto **Esquema JSON de cuerpo de solicitud**.
   - Seleccione **Usar una carga de ejemplo para generar el esquema**. En el cuadro de texto **Enter or paste a sample JSON payload** (Escriba o pegue una carga JSON de ejemplo), pegue el [ejemplo JSON](#json-example). Seleccione **Listo** para crear el esquema.

   >[!Note]
   >En este punto del flujo puede conectarse a un sistema CRM o configurar una notificación de correo electrónico.

### <a name="to-connect-to-a-crm-system"></a>Para conectarse a un sistema CRM

1. Seleccione **+ New step**(+ Nuevo paso).
2. Elija el sistema CRM que prefiera con la acción para crear un registro. La siguiente captura de pantalla muestra **Dynamics 365: Crear un nuevo registro** como ejemplo.

    ![Crear un nuevo registro](./media/cloud-partner-portal-lead-management-instructions-https/https-image009.png)

3. Proporcione el **nombre de la organización** que es una de las entradas de conexión para el conector. Seleccione **Clientes potenciales** en la lista desplegable **Nombre de entidad**.

    ![Selección de clientes potenciales](./media/cloud-partner-portal-lead-management-instructions-https/https-image011.png)

4. Flow muestra un formulario para proporcionar información sobre clientes potenciales. Puede asignar elementos de la solicitud de entrada y elige agregar contenido dinámico. La captura de pantalla siguiente muestra **OfferTitle** como ejemplo.

    ![Incorporación de contenido dinámico](./media/cloud-partner-portal-lead-management-instructions-https/https-image013.png)

5. Asigne los campos que quiera y seleccione **Guardar** para guardar el flujo.

6. Se crea una dirección URL de HTTP POST en la solicitud. Copie esta dirección URL y úsela como el punto de conexión HTTPS.

    ![Dirección URL de HTTP POST](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-get-post-url.png)

### <a name="to-set-up-email-notification"></a>Para configurar una notificación de correo electrónico

1. Seleccione **+ New step**(+ Nuevo paso).
2. En **Elegir una acción**, seleccione **Acciones**.
3. En **Acciones**, seleccione **Enviar un correo electrónico**.

    ![Adición de una acción de correo electrónico](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-add-email-action.png)

4. En **Enviar un correo electrónico**, configure los siguientes campos obligatorios:

   - **Para**: escriba al menos una dirección de correo electrónico válida.
   - **Asunto**: Flow le ofrece la opción de agregar contenido dinámico como, por ejemplo, **LeadSource** en la siguiente captura de pantalla.

     ![Agregar una acción de correo electrónico mediante contenido dinámico](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-configure-email-dynamic-content.png)

   - **Cuerpo**: en la lista de contenido dinámico, agregue la información que desee en el cuerpo del correo electrónico. Por ejemplo, nombre, apellidos, correo electrónico y empresa.

   Cuando haya terminado de configurar la notificación por correo electrónico, tendrá un aspecto similar al ejemplo de la siguiente captura de pantalla.

   ![Adición de una acción de correo electrónico](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-configure-email-action.png)

5. Seleccione **Guardar** para finalizar el flujo.
6. Se crea una dirección URL de HTTP POST en la solicitud. Copie esta dirección URL y úsela como el punto de conexión HTTPS.

    ![Dirección URL de HTTP POST](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-get-post-url.png)

## <a name="configure-your-offer-to-send-leads-to-the-https-endpoint"></a>Configuración de la oferta para enviar clientes potenciales al punto de conexión HTTPS

Cuando configure la información de administración de clientes potenciales para la oferta, seleccione **Punto de conexión HTTPS** para el **destino de clientes potenciales** y pegue la dirección URL de HTTPS POST que copió en el paso anterior.  

![Incorporación de contenido dinámico](./media/cloud-partner-portal-lead-management-instructions-https/https-image017.png)

Cuando se generan los clientes potenciales, Microsoft los envía a Flow que los enruta al sistema de CRM o dirección de correo electrónico que configuró.

## <a name="json-schema-and-example"></a>Esquema y ejemplo de JSON

El ejemplo de prueba de JSON usa el esquema siguiente:

### <a name="json-schema"></a>Esquema JSON

``` json
{
  "$schema": "https://json-schema.org/draft-04/schema#",
  "definitions": {},
  "id": "http://example.com/example.json",
  "properties": {
    "ActionCode": {
      "id": "/properties/ActionCode",
      "type": "string"
    },
    "OfferTitle": {
      "id": "/properties/OfferTitle",
      "type": "string"
    },
    "LeadSource": {
      "id": "/properties/LeadSource",
      "type": "string"
    },
    "UserDetails": {
      "id": "/properties/UserDetails",
      "properties": {
        "Company": {
          "id": "/properties/UserDetails/properties/Company",
          "type": "string"
        },
        "Country": {
          "id": "/properties/UserDetails/properties/Country",
          "type": "string"
        },
        "Email": {
          "id": "/properties/UserDetails/properties/Email",
          "type": "string"
        },
        "FirstName": {
          "id": "/properties/UserDetails/properties/FirstName",
          "type": "string"
        },
        "LastName": {
          "id": "/properties/UserDetails/properties/LastName",
          "type": "string"
        },
        "Phone": {
          "id": "/properties/UserDetails/properties/Phone",
          "type": "string"
        },
        "Title": {
          "id": "/properties/UserDetails/properties/Title",
          "type": "string"
        }
      },
      "type": "object"
    }
  },
  "type": "object"
}
```

Puede copiar y editar el siguiente ejemplo de JSON para usarlo como prueba en MS Flow.

### <a name="json-example"></a>Ejemplo JSON

```json
{
"OfferTitle": "Test Microsoft",
"LeadSource": "Test run through MS Flow",
"UserDetails": {
"Company": "Contoso",
"Country": "USA",
"Email": "someone@contoso.com",
"FirstName": "Some",
"LastName": "One",
"Phone": "16175555555",
"Title": "Esquire"
}
}
```

## <a name="next-steps"></a>Pasos siguientes

Si aún no lo ha hecho, configure [clientes potenciales](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads) en Cloud Partner Portal.
