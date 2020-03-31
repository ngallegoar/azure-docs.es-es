---
title: Tabla de Azure | Azure Marketplace
description: Configure la administración de clientes potenciales para Azure Table.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: dsindona
ms.openlocfilehash: f511a60b533d6d1e0b1ae8847d0ee0fb6be3500c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80288842"
---
# <a name="configure-lead-management-using-an-azure-table"></a>Configuración de la administración de clientes potenciales mediante una tabla de Azure

Si el sistema de administración de las relaciones con el cliente (CRM) no se admite explícitamente en el Centro de partners para recibir clientes potenciales de Azure Marketplace y AppSource, puede usar una tabla de Azure para controlar estos clientes potenciales. Después, puede exportar los datos e importarlos en el sistema CRM. Las instrucciones de este artículo le guiarán por el proceso de creación de una cuenta de Azure Storage y una tabla de Azure en esa cuenta. Además, puede crear un nuevo flujo mediante Microsoft Flow para enviar una notificación por correo electrónico cuando su oferta reciba un cliente potencial.

## <a name="configure-azure-table"></a>Configuración de una tabla de Azure

1. Si no tiene una cuenta de Azure, puede [crear una cuenta de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).
2. Cuando la cuenta de Azure esté activa, inicie sesión en [Azure Portal](https://portal.azure.com).
3. En Azure Portal, cree una cuenta de almacenamiento mediante el siguiente procedimiento.  
    1. En la barra de menús de la izquierda, seleccione **+ Crear un recurso**.  El panel **Nuevo** (hoja) se mostrará a la derecha.
    2. Seleccione **Almacenamiento** en el panel **Nuevo**.  En la derecha se mostrará una lista de **Destacados**.
    3. Seleccione **Cuenta de almacenamiento** para iniciar la creación de la cuenta.  Siga las instrucciones que aparecen en el artículo [Crear una cuenta de almacenamiento](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal).

        ![Pasos para crear una cuenta de Azure Storage](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-storage-create.png)

        Para más información sobre las cuentas de almacenamiento, seleccione [Tutorial de inicio rápido](https://docs.microsoft.com/azure/storage/).  Para más información sobre los precios de almacenamiento, consulte [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

4. Espere hasta que se aprovisione la cuenta de almacenamiento, proceso que normalmente tarda unos minutos.  Luego, acceda a la cuenta de almacenamiento desde la página de **Inicio** de Azure Portal tras seleccionar **Ver todos los recursos** o **Todos los recursos** en la barra de menús de navegación de la izquierda.

    ![Acceso a la cuenta de almacenamiento de Azure](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-storage-access.png)

5. En el panel de la cuenta de almacenamiento, seleccione **Claves de acceso** y copie el valor de la *Cadena de conexión* de la clave. Guarde este valor, ya que es el valor de la *cadena de conexión de cuenta de almacenamiento* que tendrá que proporcionar en el portal de publicación para recibir clientes potenciales para su oferta de Marketplace. 

    Este es un ejemplo de una cadena de conexión:

    ```sql
    DefaultEndpointsProtocol=https;AccountName=myAccountName;AccountKey=myAccountKey;EndpointSuffix=core.windows.net
    ```

    ![Claves de Azure Storage](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-storage-keys.png)

6. En el panel de la cuenta de almacenamiento, seleccione **Tablas** y **+Tabla** para crear una tabla. Escriba el nombre de la tabla y seleccione **Aceptar**. Guarde este valor, ya que lo necesitará si quiere configurar una instancia de MS Flow para recibir notificaciones por correo electrónico cuando reciba clientes potenciales.

    ![Tablas de Azure](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-tables.png)

    Puede usar el [Explorador de Azure Storage](https://archive.codeplex.com/?p=azurestorageexplorer) o cualquier otra herramienta para ver los datos en la tabla de almacenamiento. También puede exportar los datos en la tabla de Azure. 

## <a name="optional-use-microsoft-flow-with-an-azure-table"></a>(Opcional) Uso de Microsoft Flow con una tabla de Azure  

Puede usar [Microsoft Flow](https://docs.microsoft.com/flow/) para automatizar las notificaciones cada vez que un cliente potencial se agregue a la tabla de Azure. Si aún no tiene una cuenta, puede [registrarse para obtener una gratuita](https://flow.microsoft.com/).

### <a name="lead-notification-example"></a>Ejemplo de notificación de cliente potencial

Utilice este ejemplo como guía para crear un flujo simple que envíe automáticamente una notificación por correo electrónico cuando se agregue un nuevo cliente potencial a una tabla de Azure. En este ejemplo se configura una periodicidad para enviar información de clientes potenciales cada hora si se actualiza el almacenamiento en la tabla.

1. Inicie sesión en la cuenta de Microsoft Flow.
2. En la barra de navegación de la izquierda, seleccione **My flows** (Mis flujos).
3. En la barra de navegación de la izquierda, seleccione **+ New** (+ Nuevo).  
4. En la lista desplegable, seleccione **+ Programado: desde cero**.

   ![Mis flujos **+ Programado: desde cero**](./media/commercial-marketplace-lead-management-instructions-azure-table/ms-flow-scheduled-from-blank.png)

5.  En la ventana *Crear un flujo programado* en *Repetir cada* seleccione "1" para el intervalo y "hora" para la frecuencia. Además, asigne un nombre al flujo si lo desea. Seleccione **Crear**.

    >[!Note]
    >Aunque en este ejemplo se usa un intervalo de 1 hora, puede seleccionar el intervalo y la frecuencia más apropiada para sus necesidades empresariales.

    ![Cree un flujo programado.](./media/commercial-marketplace-lead-management-instructions-azure-table/build-scheduled-flow.png)

6. Seleccione **+ New step**(+ Nuevo paso).
7. En la ventana *Elegir una acción*, busque "obtener la hora pasada" y, luego, seleccione **Obtener la hora pasada** en Acciones.

   ![Elija una acción.](./media/commercial-marketplace-lead-management-instructions-azure-table/choose-an-action.png)

8. En la ventana **Obtener la hora pasada**, establezca el **Intervalo** en 1. Desde la lista desplegable **Unidad de tiempo**, seleccione **Hora**.

    >[!Important]
    >Asegúrese de que este intervalo y unidad de tiempo coincidan con el intervalo y la frecuencia que ha configurado para la periodicidad en el paso 5.

    ![Establecimiento del valor para obtener el intervalo de tiempo pasado](./media/commercial-marketplace-lead-management-instructions-azure-table/ms-flow-getpast-time.png)

>[!TIP] 
>Puede comprobar el flujo en cualquier momento para comprobar que cada paso está configurado correctamente. Para comprobar el flujo, seleccione **Comprobador de flujo** en la barra de menús Flujo.

En el siguiente conjunto de pasos, se conectará a la tabla de Azure y configurará la lógica de procesamiento para controlar nuevos clientes potenciales.

9. Después del paso de obtener la hora pasada, seleccione **+Nuevo paso** y, luego, busque "Obtener entidades" en la ventana *Elegir una acción*.
10. En **Acciones**,seleccione **Obtener entidades (Almacenamiento de tablas de Azure)** .
11. En la ventana de **Almacenamiento de tablas de Azure**, proporcione información para los campos siguientes y seleccione **Crear**:

    * *Nombre de conexión*: Proporcione un nombre descriptivo para la conexión que va a establecer entre este flujo y la tabla de Azure.
    * *Nombre de cuenta de almacenamiento*: Proporcione el nombre de la cuenta de almacenamiento para la tabla de Azure. Puede encontrarlo en la página **Claves de acceso** de la cuenta de almacenamiento.
    * *Clave de almacenamiento compartida*: Proporcione el valor de clave de la cuenta de almacenamiento de la tabla de Azure. Puede encontrarlo en la página **Claves de acceso** de la cuenta de almacenamiento.

        ![Azure Table Storage](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-storage.png)

    Después de hacer clic en Crear, verá una ventana *Obtener entidades*. Aquí, seleccione **Mostrar opciones avanzadas** y proporcione información para los campos siguientes:

       * *Tabla*: Seleccione el nombre del almacenamiento de tablas de Azure (del paso 6 de las instrucciones sobre cómo configurar una tabla de Azure). La captura de pantalla siguiente muestra el símbolo del sistema cuando se selecciona "marketplaceleads" en este ejemplo.

            ![Obtener entidades de tabla de Azure.](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-get-entities.png)

        * *Consulta de filtro*: Seleccione este campo y pegue esta función en el campo: `Timestamp gt datetime'@{body('Get_past_time')}'`

            ![Obtener entidades de tabla de Azure: consulta de filtro.](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-get-entities-filter-query.png)

12. Ahora que ha completado la configuración de la conexión a la tabla de Azure, seleccione **Nuevo paso** para agregar una condición para examinar la tabla de Azure para los nuevos clientes potenciales. 

13. En la ventana **Elegir una acción**, seleccione **Acciones** y, luego, seleccione el **control Condición**.

    ![Tabla de Azure: Elegir una acción.](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-choose-an-action.png)

14. En la ventana **Condición**, seleccione el campo **Elegir un valor** y, a continuación, seleccione **Expresión** en la ventana emergente.

15. Pegue `length(body('Get_entities')?['value'])` en el campo ***fx***. Seleccione **Aceptar** para agregar esta función. 

16. Para finalizar la configuración de la condición:
    1. Seleccione "es mayor que" en la lista desplegable.
    2. Escriba 0 como valor.

        ![Tabla de Azure: Condición.](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-condition.png)

En los pasos siguientes, va a configurar la acción que se realizará según el resultado de la condición.

* Si la condición se resuelve como **En caso negativo**, no haga nada.
* Si la condición se resuelve como **En caso positivo**, desencadene una acción que se conecte a su cuenta de Office 365 para enviar un correo electrónico. 

17. Seleccione **Agregar una acción** en **En caso positivo**.

    ![Tabla de Azure: Condición, **En caso positivo**.](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-condition-if-yes.png)

18. Seleccione **Enviar un correo electrónico (Office 365 Outlook)** .

    ![Tabla de Azure: Condición, **En caso positivo**, enviar correo electrónico.](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-condition-if-yes-send-email.png)

    >[!Note]
    >Si quiere usar otro proveedor de correo electrónico, busque y seleccione Enviarme una notificación por correo electrónico (Correo) como acción en su lugar. En las instrucciones se muestra cómo configurar con Office 365 Outlook, pero las instrucciones son similares para otro proveedor de correo electrónico.

19. En la ventana **Office 365 Outlook**, proporcione la información para los campos siguientes:

    1. **Para**: escriba una dirección de correo electrónico para todos los usuarios que reciban esta notificación.
    1. **Asunto**: proporcione un asunto para el correo electrónico. Por ejemplo: Nuevos clientes potenciales.
    1. **Cuerpo**: Agregue el texto que quiera incluir en cada correo electrónico (opcional) y, luego, pegue en el cuerpo `body('Get_entities')?['value']`.

    >[!Note]
    >Puede insertar puntos de datos estáticos o dinámicos adicionales en el cuerpo de este correo electrónico.

    ![Tabla de Azure: Condición, **En caso positivo**, ventana Office 365 Outlook.](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-condition-if-yes-outlook.png)

20. Seleccione **Guardar** para guardar el flujo. Microsoft Flow probará automáticamente el flujo de errores. Si no hay errores, el flujo comienza a ejecutarse después de guardarlo.

La captura de pantalla siguiente muestra un ejemplo de cómo debe ser el último flujo.

![Un ejemplo del flujo final.](./media/commercial-marketplace-lead-management-instructions-azure-table/final-flow.png)

### <a name="manage-your-flow"></a>Administración del flujo

Es fácil administrar el flujo una vez se está ejecutando. Tiene control completo sobre el flujo. Por ejemplo, puede detenerlo, editarlo, ver un historial de ejecución y obtener el análisis. La captura de pantalla siguiente muestra las opciones disponibles para administrar un flujo. 

 ![Administración de un flujo](./media/commercial-marketplace-lead-management-instructions-azure-table/ms-flow-manage-completed.png)

El flujo seguirá ejecutándose hasta que lo detenga mediante la opción **Desactivar el flujo**.

Si no recibe notificaciones de correo electrónico de clientes potenciales, significa que no se han agregado nuevos clientes potenciales a la tabla de Azure. Si se produce un error de flujo, recibirá un correo electrónico similar al del ejemplo de la siguiente captura de pantalla.

 ![Notificación por correo electrónico de los errores del flujo](./media/commercial-marketplace-lead-management-instructions-azure-table/ms-flow-failure-note.png)

## <a name="configure-your-offer-to-send-leads-to-the-azure-table"></a>Configuración de la oferta para enviar clientes potenciales a la tabla de Azure

Cuando esté listo para configurar la información de administración de clientes potenciales de la oferta en el portal de publicación, siga los pasos a continuación:

1. Vaya a la página **Configuración de la oferta** de su oferta.
2. Seleccione **Conectar** en la sección Administración de clientes potenciales.
3. En la ventana emergente Detalles de conexión, seleccione **Tabla de Azure** para el **Destino de clientes potenciales** y pegue la cadena de conexión de la cuenta de Azure Storage que creó siguiendo los pasos anteriores en el campo **Cadena de conexión de cuenta de almacenamiento**.
4. Seleccione **Guardar**. 

>[!Note]
>Debe terminar de configurar el resto de las partes de la oferta y publicarla para poder recibir clientes potenciales para la oferta.

Cuando se generan clientes potenciales, Microsoft envía los clientes potenciales a la tabla de Azure. Si ha configurado un flujo, también se enviará un correo electrónico a la dirección de correo electrónico configurada.

![Administración de clientes potenciales](./media/commercial-marketplace-lead-management-instructions-azure-table/lead-management.png)

![Administración de clientes potenciales: detalles de conexión](./media/commercial-marketplace-lead-management-instructions-azure-table/connection-details.png)

![Administración de clientes potenciales: detalles de conexión de la cuenta de almacenamiento](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-connection-details.png)

