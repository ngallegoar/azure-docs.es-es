---
title: Azure Table Storage | Azure Marketplace
description: Configure la administración de clientes potenciales en Azure Table Storage.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: dsindona
ms.openlocfilehash: e65f2041cdb2e25341bfd63783c70ec09c1216b5
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83124710"
---
# <a name="lead-management-instructions-for-table-storage"></a>Instrucciones de administración de clientes potenciales con Table Storage

En este artículo se describe cómo configurar Azure Table Storage para administrar clientes potenciales. Table Storage le ayuda a almacenar y modificar la información de los clientes.

## <a name="configure-table-storage"></a>Configuración del almacenamiento de tabla

1. Si no tiene una cuenta de Azure, [cree una cuenta de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).
1. Cuando la cuenta esté activa, inicie sesión en [Azure Portal](https://portal.azure.com).
1. En Azure Portal, haga lo siguiente:  
    1. En el panel de la izquierda, seleccione **+Crear un recurso**. Se abrirá el panel **Nuevo**.
    1. En el panel **Nuevo**, seleccione **Storage**. Se abrirá una lista de **Destacados** en el lado derecho.
    1. Seleccione **Cuenta de almacenamiento**. A continuación, siga las instrucciones para [crear una cuenta de almacenamiento](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal).

    ![Creación de una cuenta de Azure Storage](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragecreate.png)

    Para obtener más información sobre las cuentas de almacenamiento, seleccione [Tutorial de inicio rápido](https://docs.microsoft.com/azure/storage/). Para obtener información sobre precios, consulte [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

1. Espere hasta que se aprovisione la cuenta de almacenamiento, que normalmente tarda unos minutos. A continuación, acceda a la cuenta desde la página principal de Azure Portal: Seleccione **Ver todos los recursos** o **Todos los recursos** en el panel de navegación.

    ![Acceso a la cuenta de almacenamiento de Azure](./media/cloud-partner-portal-lead-management-instructions-azure-table/azure-storage-access.png)

1. En el panel de la cuenta de almacenamiento, copie la cadena de conexión de la cuenta de almacenamiento para obtener la clave. Péguelo en el campo **Cadena de conexión** para la cuenta de almacenamiento en Cloud Partner Portal.

    Cadena de conexión de ejemplo:

    ```sql
    DefaultEndpointsProtocol=https;AccountName=myAccountName;AccountKey=myAccountKey;EndpointSuffix=core.windows.net
    ```

      ![Claves de Azure Storage](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragekeys.png)

Puede usar el [Explorador de Azure Storage](https://azurestorageexplorer.codeplex.com/) o una herramienta parecida para ver los datos de Table Storage. También puede exportar datos desde allí.

## <a name="use-microsoft-flow-with-table-storage-optional"></a>Uso de Microsoft Flow con Table Storage (*opcional*)

Puede usar [Microsoft Flow](https://docs.microsoft.com/flow/) para enviar notificaciones automáticamente cuando se agrega un cliente potencial a Table Storage. Si no tiene ninguna cuenta de Microsoft Flow, [regístrese para obtener una cuenta gratuita](https://flow.microsoft.com/).

### <a name="lead-notification-example"></a>Ejemplo de notificación de cliente potencial

En este ejemplo se muestra cómo crear un flujo básico. El flujo envía automáticamente una notificación por correo electrónico cuando se agregan nuevos clientes potenciales a Table Storage.

1. Inicie sesión en la cuenta de Microsoft Flow.
1. En el panel de navegación de la izquierda, seleccione **Mis flujos**.
1. En la barra de navegación de la izquierda, seleccione **+Nuevo**.  
1. En la lista desplegable, seleccione **+Crear desde cero**.
1. En **Crear un flujo a partir de un documento en blanco**, seleccione **Crear desde cero**.

   ![Creación de un flujo a partir de un documento en blanco](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-create-from-blank.png)

1. En la página de búsqueda de los conectores y desencadenadores, escriba **Desencadenadores**.
1. En **Desencadenadores**, seleccione **Periodicidad**.
1. En la ventana **Periodicidad**, mantenga el valor predeterminado de **1** para **Intervalo**. En la lista desplegable **Frecuencia**, seleccione **Hora**.

   >[!NOTE] 
   >En este ejemplo se usa un intervalo de una hora. Sin embargo, puede seleccionar el intervalo y la frecuencia que mejor se adapten a sus necesidades empresariales.

   ![Establecimiento de la frecuencia de 1 hora para la periodicidad](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-recurrence-dropdown.png)

1. Seleccione **+Nuevo paso.**
1. Busque **Obtener la hora pasada** y, a continuación, seleccione **Obtener la hora pasada** en **Elegir una acción**.

    ![Búsqueda y selección de la acción "obtener la hora pasada"](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-search-getpasttime.png)

1. En la ventana **Obtener la hora pasada**, establezca el **Intervalo** en **1**.  En la lista desplegable **Unidad de tiempo**, seleccione **Hora**.
    >[!IMPORTANT] 
    >Asegúrese de que **Intervalo** y **Unidad de tiempo** coincidan con el intervalo y la frecuencia que ha configurado para la periodicidad (paso 8).

    ![Establecimiento del valor para obtener el intervalo de tiempo pasado](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-getpast-time.png)

    >[!TIP] 
    >Puede comprobar el flujo en cualquier momento para comprobar que cada paso esté configurado correctamente: Seleccione **Comprobador de flujo** en la barra de menús Flujo.

En el siguiente conjunto de pasos, se conectará a Table Storage y configurará la lógica de procesamiento para controlar nuevos clientes potenciales.

1. Después del paso **Obtener la hora pasada**, seleccione **+Nuevo paso** y, a continuación, busque **Obtener entidades**.
1. En **Acciones**, seleccione **Obtener entidades** y, a continuación, seleccione **Mostrar opciones avanzadas**.
1. En la ventana **Obtener entidades**, rellene los siguientes campos:

   - **Tabla**: el nombre de Table Storage. En la imagen siguiente se muestra "MarketPlaceLeads":

     ![Elija un valor personalizado para el nombre de la tabla de Azure](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-getentities-table-name.png)

   - **Consulta de filtro**: Al seleccionar este campo, el icono de **Obtener la hora pasada** se mostrará en una ventana emergente. Seleccione **Hora pasada** para usar este valor como marca de tiempo para filtrar la consulta. O bien, puede pegar la función siguiente en el campo:
   
      `CreatedTime Timestamp gt '@{body('Get_past_time')}'` 

     ![Configuración de la función de consulta de filtro](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-getentities-filterquery.png)

1. Seleccione **Nuevo paso** para agregar una condición para examinar Table Storage y comprobar si hay nuevos clientes potenciales.

   ![Uso de "Nuevo paso" para agregar una condición para examinar Table Storage](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-add-filterquery-new-step.png)

1. En la ventana **Elegir una acción**, seleccione **Acciones** y, a continuación, seleccione el **Control de condición**.

     ![Adición de un control de condición](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-action-condition-control.png)

1. En la ventana **Condición**, seleccione el campo **Elegir un valor** y, a continuación, seleccione **Expresión** en la ventana emergente.
1. Pegue `length(body('Get_entities')?['value'])` en el campo ***fx***. Seleccione **Aceptar** para agregar esta función. 



     ![Adición de una función a la condición](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-condition-fx0.png)

1. Configure la acción que se realizará basándose en el resultado de la condición.

    1. Seleccione **es mayor que** en la lista desplegable.
   1. Escriba **0** como valor.

     ![Configuración de una acción según los resultados de la condición](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-condition-pick-action.png)

1. Si la condición se resuelve como "If no", no haga nada.

    Si la condición se resuelve como "En caso positivo", desencadene una acción que se conecte a su cuenta de Office 365 para enviar un correo electrónico:
   1. Seleccione **Add an action**(Agregar una acción).
   1. Seleccione **Enviar un correo electrónico**.
   1. En la ventana **Enviar un correo electrónico**, escriba información en los campos siguientes:

      - **Para**: dirección de correo electrónico para todos los usuarios que recibirán la notificación.
      - **Asunto**: asunto para el correo electrónico. Por ejemplo: *Nuevos clientes potenciales.*
      - **Cuerpo**: texto que quiere incluir en cada correo electrónico (opcional). También puede pegar `body('Get_entities')?['value']` como una función para insertar información de clientes potenciales.

        >[!NOTE] 
        >Puede insertar puntos de datos estáticos o dinámicos adicionales en el cuerpo de este correo electrónico.

      ![Configuración del correo electrónico de notificación de clientes potenciales](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-emailbody-fx.png)

1. Seleccione **Guardar** para guardar el flujo. Microsoft Flow lo probará automáticamente en busca de errores. Si no hay errores, el flujo comienza a ejecutarse después de guardarlo.

    La imagen siguiente muestra un ejemplo de cómo debe ser el último flujo.

    [![Secuencia final de flujo](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-end-to-end-thmb.png)](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-end-to-end.png)

    (*Seleccione la imagen para ampliarla*).

### <a name="manage-your-flow"></a>Administración del flujo

Es fácil administrar el flujo una vez se está ejecutando. Tiene control completo sobre el flujo. Por ejemplo, puede detenerlo, editarlo, ver un historial de ejecución y obtener el análisis. En la imagen siguiente se muestran las opciones de administración de flujo.

 ![Opciones de administración de flujo](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-manage-completed.png)

El flujo sigue ejecutándose hasta que seleccione **Desactivar el flujo**.

Si no recibe notificaciones de correo electrónico de clientes potenciales, no se ha agregado ningún nuevo cliente potencial a Table Storage.
Recibirá un correo electrónico similar al siguiente ejemplo si se produce un error del flujo:

 ![Notificación por correo electrónico de los errores del flujo](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-failure-note.png)

## <a name="next-steps"></a>Pasos siguientes

[Configuración de clientes potenciales](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads)
