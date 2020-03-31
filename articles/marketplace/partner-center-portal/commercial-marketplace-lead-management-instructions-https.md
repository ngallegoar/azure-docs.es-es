---
title: Punto de conexión HTTPS | Azure Marketplace
description: Configure la administración de clientes potenciales para un punto de conexión HTTPS.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 07/31/2019
ms.author: dsindona
ms.openlocfilehash: 6a0131cf94759fc529a52ea33d5392a60c5ef30c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80281602"
---
# <a name="configure-lead-management-using-an-https-endpoint"></a>Configuración de la administración de clientes potenciales mediante un punto de conexión HTTPS

Si el sistema de administración de las relaciones con el cliente (CRM) no se admite explícitamente en el Centro de partners para recibir clientes potenciales de Azure Marketplace y AppSource, puede usar un punto de conexión HTTPS en MS Flow para controlar estos clientes potenciales. Con un punto de conexión HTTPS, estos avisos de clientes potenciales se pueden enviar como notificación por correo electrónico o pueden escribirse en un sistema de administración de relaciones con clientes (CRM) admitido por MS Flow. Las instrucciones de este artículo le guiarán por el proceso básico para crear un nuevo flujo con Microsoft Flow, lo que generará la dirección URL de HTTP POST que escribirá en el portal de publicación para Administración de clientes potenciales > campo **Dirección URL del punto de conexión HTTPS**. Además, se incluyen instrucciones sobre cómo puede probar el flujo con la ayuda de una herramienta denominada [Postman](https://www.getpostman.com/downloads/) que se puede encontrar en línea.

## <a name="create-a-flow-using-microsoft-flow"></a>Creación de un flujo mediante Microsoft Flow

1. Abra la página web de [Flow](https://flow.microsoft.com/). Seleccione **Iniciar sesión** o, si aún no tiene una cuenta, **Regístrese gratis** para crear una cuenta gratuita de Flow.

2. Inicie sesión y seleccione **Mis flujos** en la barra de menús.

3. Seleccione **+ Automatizado: desde cero**.

    ![Mis flujos + Automatizado: desde cero](./media/commercial-marketplace-lead-management-instructions-https/my-flows-automated.png)

4. En la ventana *Crear un flujo automatizado*, seleccione **Omitir**. 

    ![Crear flujo automatizado: Omitir](./media/commercial-marketplace-lead-management-instructions-https/build-automated-flow.png)

5. En el campo **Buscar conectores y desencadenadores**, escriba "solicitud" para encontrar el conector de la solicitud.
6. En *Desencadenadores*, seleccione **Cuando se recibe una solicitud HTTP**. 

    ![Conector de solicitud: Desencadenadores](./media/commercial-marketplace-lead-management-instructions-https/request-connector.png)

7. En la ventana *Cuando se recibe una solicitud HTTP*, copie el siguiente esquema JSON y péguelo en el cuadro de texto **Esquema JSON de cuerpo de solicitud**. Microsoft usa este esquema para contener los datos de los clientes potenciales.

    ![Conector de solicitud: Desencadenadores](./media/commercial-marketplace-lead-management-instructions-https/https-request-received.png)

    **Esquema JSON**

    ```JSON
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

>[!Note]
>En este punto de la configuración, puede seleccionar conectarse a un sistema CRM o configurar una notificación por correo electrónico. Siga las instrucciones restantes en función de su elección.

### <a name="to-connect-to-a-crm-system"></a>Para conectarse a un sistema CRM

1. Seleccione **+ New step**(+ Nuevo paso).
2. Elija el sistema CRM de su elección; para ello, busque el donde dice *Buscar todos los conectores y acciones* y selecciónelo en la sección *Acciones* con la acción para crear un nuevo registro. La siguiente captura de pantalla muestra **Dynamics 365: Crear un nuevo registro** como ejemplo.

    ![Crear un nuevo registro](./media/commercial-marketplace-lead-management-instructions-https/create-new-record.png)

3. Proporcione el **Nombre de la organización** asociado al sistema CRM. Seleccione **Clientes potenciales** en la lista desplegable **Nombre de entidad**.

    ![Selección de clientes potenciales](./media/commercial-marketplace-lead-management-instructions-https/select-leads.png)

4. Flow muestra un formulario para proporcionar información sobre clientes potenciales. Puede asignar elementos de la solicitud de entrada y elige agregar contenido dinámico. La captura de pantalla siguiente muestra **OfferTitle** como ejemplo.

    ![Incorporación de contenido dinámico](./media/commercial-marketplace-lead-management-instructions-https/add-dynamic-content.png)

5. Asigne los campos que quiera y seleccione **Guardar** para guardar el flujo. Se crea una dirección URL de HTTP POST, y se puede acceder a ella en la ventana *Cuando se recibe una solicitud HTTP*. Copie esta dirección URL con el control de copia que se encuentra a la derecha de la dirección URL de HTTP POST (esto es importante para no omitir por error ninguna parte de la dirección URL completa). Guarde esta dirección URL, ya que la necesitará al configurar la administración de clientes potenciales en el portal de publicación.

    ![Cuando se recibe una solicitud HTTP.](./media/commercial-marketplace-lead-management-instructions-https/when-http-request-received.png)

### <a name="to-set-up-email-notification"></a>Para configurar una notificación de correo electrónico

1. Ahora que ha completado el esquema JSON, seleccione **+ Nuevo paso**.
2. En **Elegir una acción**, seleccione **Acciones**.
3. En **Acciones**, seleccione **Enviar un correo electrónico (Office 365 Outlook)** .

    >[!Note]
    >Si quiere usar otro proveedor de correo electrónico, busque y seleccione *Enviarme una notificación por correo electrónico (Correo)* como acción en su lugar.

    ![Adición de una acción de correo electrónico](./media/commercial-marketplace-lead-management-instructions-https/https-request-received-send-email.png)

4. En la ventana **Enviar un correo electrónico**, configure los siguientes campos obligatorios:

   - **Para**: Especifique al menos una dirección de correo electrónico válida, adonde se enviarán los clientes potenciales.
   - **Asunto**: Flow le ofrece la opción de agregar contenido dinámico como, por ejemplo, **LeadSource** en la siguiente captura de pantalla. Para empezar, escriba un nombre de campo y haga clic en la lista desplegable Contenido dinámico en la ventana emergente. 

        >[!Note] 
        > Al agregar nombres de campos, puede seguir cada uno con ":" y, luego, Entrar para crear una nueva fila. Una vez que haya agregado los nombres de campos, puede agregar cada parámetro asociado desde la lista desplegable dinámica.

        ![Agregar una acción de correo electrónico mediante contenido dinámico](./media/commercial-marketplace-lead-management-instructions-https/add-email-using-dynamic-content.png)

   - **Cuerpo**: En la lista desplegable Contenido dinámico, agregue la información que quiera en el cuerpo del correo electrónico. Por ejemplo, nombre, apellidos, correo electrónico y empresa. <br> <br> Cuando haya terminado de configurar la notificación por correo electrónico, tendrá un aspecto similar al ejemplo de la siguiente captura de pantalla.


       ![Adición de una acción de correo electrónico](./media/commercial-marketplace-lead-management-instructions-https/send-an-email.png)

5. Seleccione **Guardar** para finalizar el flujo. Se crea una dirección URL de HTTP POST, y se puede acceder a ella en la ventana *Cuando se recibe una solicitud HTTP*. Copie esta dirección URL con el control de copia que se encuentra a la derecha de la dirección URL de HTTP POST (esto es importante para no omitir por error ninguna parte de la dirección URL completa). Guarde esta dirección URL, ya que la necesitará al configurar la administración de clientes potenciales en el portal de publicación.

   ![Dirección URL de HTTP POST ](./media/commercial-marketplace-lead-management-instructions-https/http-post-url.png)

### <a name="testing"></a>Prueba

Puede probar que todo funcione según lo previsto siguiendo los pasos a continuación con una herramienta denominada [Postman](https://app.getpostman.com/app/download/win64), que se puede descargar en línea. Está disponible para Windows. 

1. Inicie Postman y seleccione **Nuevo** > **Solicitud** para configurar la herramienta de prueba. 

   ![Solicitud de configuración de la herramienta de prueba](./media/commercial-marketplace-lead-management-instructions-https/postman-request.png)

2. Rellene el formulario *Guardar solicitud* y, luego, **guárdelo** en la carpeta que creó.

   ![Guardar solicitud](./media/commercial-marketplace-lead-management-instructions-https/postman-save-to-test.png)

3. Seleccione **POST** en la lista desplegable. 

   ![Probar mi flujo](./media/commercial-marketplace-lead-management-instructions-https/test-my-flow.png)

4. Pegue la dirección URL de HTTP POST del flujo que creó en MS Flow, donde dice *Escribir la dirección URL de la solicitud*.

   ![Pegue la dirección URL de HTTP POST](./media/commercial-marketplace-lead-management-instructions-https/paste-http-post-url.png)

5. Vuelva a [Flow](https://flow.microsoft.com/) y busque el flujo que creó para enviar clientes potenciales; para ello, vaya a **Mis flujos** en la barra de menús de Flow.  Seleccione los tres puntos situados junto al nombre del flujo y seleccione **Editar**.

   ![Mis flujos: Editar](./media/commercial-marketplace-lead-management-instructions-https/my-flows-edit.png)

6. Seleccione **Probar** en la esquina superior derecha, seleccione "Yo realizaré la acción de desencadenamiento" y luego seleccione **Probar**. Verá una indicación en la parte superior de la pantalla que indica que se ha iniciado la prueba.

   ![Probar flujo. Desencadenador](./media/commercial-marketplace-lead-management-instructions-https/test-flow-trigger-action.png)

7. Vuelva a la aplicación Postman y seleccione **Enviar** junto a la ubicación donde pegó la dirección URL HTTPS.

   ![Probar mi flujo: Enviar](./media/commercial-marketplace-lead-management-instructions-https/postman-send.png)

8. Vuelva al flujo y compruebe el resultado. Si todo funciona según lo previsto, verá un mensaje que indica que se ha realizado correctamente.

   ![Flujo: Comprobar resultados](./media/commercial-marketplace-lead-management-instructions-https/my-flow-check-results.png)

9. También debe haber recibido un correo electrónico. Compruebe la bandeja de entrada del correo. 

    >[!Note] 
    >Si no ve un correo electrónico de la prueba, compruebe las carpetas de correo no deseado y spam. A continuación, observará solo las etiquetas de campo que agregó al configurar la notificación por correo electrónico. Si se trata de un cliente potencial real generado a partir de su oferta, también verá la información real del Contacto de cliente potencial en el cuerpo y en la línea de asunto.

   ![Correo electrónico recibido](./media/commercial-marketplace-lead-management-instructions-https/email-received.png)

## <a name="configure-your-offer-to-send-leads-to-the-https-endpoint"></a>Configuración de la oferta para enviar clientes potenciales al punto de conexión HTTPS

Cuando esté listo para configurar la información de administración de clientes potenciales de la oferta en el portal de publicación, siga estos pasos:

1. Vaya a la página **Configuración de la oferta** de su oferta.
2. Seleccione **Conectar** en la sección Administración de clientes potenciales.
3. En la ventana emergente Detalles de conexión, seleccione **Punto de conexión HTTPS** para el **Destino de clientes potenciales** y péguelo en la dirección URL de HTTP POST del flujo que creó siguiendo los pasos anteriores en el campo **Dirección URL del punto de conexión HTTPS**.
4. Seleccione **Guardar**. 

>[!Note] 
>Debe terminar de configurar el resto de las partes de la oferta y publicarla para poder recibir clientes potenciales para la oferta.

Cuando se generan los clientes potenciales, Microsoft los envía a Flow que los enruta al sistema de CRM o dirección de correo electrónico que configuró.

![Administración de clientes potenciales: Conectar](./media/commercial-marketplace-lead-management-instructions-https/lead-management-connect.png)

![Detalles de conexión](./media/commercial-marketplace-lead-management-instructions-https/connection-details.png)

![Detalles de conexión](./media/commercial-marketplace-lead-management-instructions-https/connection-details-1.png)

