---
title: Autenticación de Azure Stream Analytics para Azure Data Lake Storage Gen1
description: En este artículo se describe cómo usar las identidades administradas para autenticar un trabajo de Azure Stream Analytics en la salida de Azure Data Lake Storage Gen1.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 04/08/2019
ms.custom: seodec18
ms.openlocfilehash: 551f0065f1547e94d93993a38795234f455b9eef
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86044403"
---
# <a name="authenticate-stream-analytics-to-azure-data-lake-storage-gen1-using-managed-identities"></a>Autenticación de Stream Analytics en Azure Data Lake Storage Gen1 mediante entidades administradas

Azure Stream Analytics admite la autenticación de identidades administradas con la salida de Azure Data Lake Storage (ADLS) Gen1. La identidad es una aplicación administrada registrada en Azure Active Directory que representa un trabajo de Stream Analytics determinado y que puede usarse para autenticar un recurso de destino. Las identidades administradas eliminan las limitaciones de los métodos de autenticación basada en usuario, como la necesidad de volver a realizar la autenticación debido a los cambios de contraseña o la expiración de tokens de usuario que se produce cada 90 días. Además, las identidades administradas sirven de ayuda en la automatización de las implementaciones de trabajos de Stream Analytics cuya salida es Azure Data Lake Storage Gen1.

En este artículo, se muestran tres maneras de habilitar la identidad administrada para un trabajo de Azure Stream Analytics cuya salida se envía a Azure Data Lake Storage Gen1 mediante Azure Portal, la implementación de una plantilla de Resource Manager y las herramientas de Azure Stream Analytics para Visual Studio.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="azure-portal"></a>Portal de Azure

1. Para empezar, cree un trabajo de Stream Analytics o abra un trabajo existente en Azure Portal. En la barra de menús situada a la izquierda de la pantalla, seleccione **Identidad administrada**, que se encuentra en **Configurar**.

   ![Configuración de la Identidad administrada de Stream Analytics](./media/stream-analytics-managed-identities-adls/stream-analytics-managed-identity-preview.png)

2. Seleccione **Use System-assigned Managed Identity** (Usar identidad administrada asignada por el sistema) en la ventana que aparece a la derecha. Haga clic en **Guardar** para crear una entidad de servicio para la identidad del trabajo de Stream Analytics en Azure Active Directory. El ciclo de vida de la identidad recién creada lo administrará Azure. Cuando se elimina el trabajo de Stream Analytics, Azure elimina automáticamente la identidad asociada (es decir, la entidad de servicio).

   Cuando se guarda la configuración, el Id. de objeto (OID) de la entidad de servicio aparece como Id. de entidad de seguridad, tal como se muestra a continuación:

   ![Identificador de la entidad de servicio de Stream Analytics](./media/stream-analytics-managed-identities-adls/stream-analytics-principal-id.png)
 
   La entidad de servicio se llama igual que el trabajo de Stream Analytics. Por ejemplo, si es el nombre del trabajo es **MyASAJob**, el nombre de la entidad de servicio creada también será **MyASAJob**.

3. En la ventana de propiedades de salida del receptor de salida ADLS Gen1, haga clic en la lista desplegable Modo de autenticación y seleccione **Identidad administrada**.

4. Rellene el resto de las propiedades. Para más información acerca de cómo crear una salida de ADLS, consulte [Creación de una salida de Data Lake Store con Stream Analytics](../data-lake-store/data-lake-store-stream-analytics.md). Cuando haya terminado, haga clic en **Guardar**.

   ![Configurar Azure Data Lake Storage](./media/stream-analytics-managed-identities-adls/stream-analytics-configure-adls.png)
 
5. Vaya a la página de información general de ADLS Gen1 y haga clic en **Data explorer** (Explorador de datos).

   ![Configurar la información general de Azure Data Lake Storage](./media/stream-analytics-managed-identities-adls/stream-analytics-adls-overview.png)

6. En el panel Data explorer (Explorador de datos), seleccione **Access** (Acceso) y haga clic en **Add** (Agregar) en el panel de acceso.

   ![Configurar el acceso de Data Lake Storage](./media/stream-analytics-managed-identities-adls/stream-analytics-adls-access.png)

7. En el cuadro de texto del panel **Select user or group** (Seleccionar usuario o grupo), escriba el nombre de la entidad de servicio. Recuerde que el nombre de la entidad de servicio es también el nombre del trabajo de Stream Analytics correspondiente. Cuando empiece a escribir el nombre de la entidad de seguridad, aparecerá debajo del cuadro de texto. Elija el nombre de entidad de seguridad de servicio que desee y haga clic en **Seleccionar**.

   ![Seleccionar un nombre de entidad de seguridad de servicio](./media/stream-analytics-managed-identities-adls/stream-analytics-service-principal-name.png)
 
8. En el panel **Permissions** (Permisos), compruebe los permisos **Read** (Lectura) y **Execute** (Ejecución), y asígnelo a **This Folder and all children** (Esta carpeta y todos los elementos secundarios). Luego, haga clic en **Aceptar**.

   ![Selección de permisos de escritura y ejecución](./media/stream-analytics-managed-identities-adls/stream-analytics-select-permissions.png)
 
9. La entidad de servicio aparece en **Assigned Permissions** (Permisos asignados) en el panel de **acceso**, como se muestra a continuación. Ya puede volver e iniciar el trabajo de Stream Analytics.

   ![Lista de acceso de Stream Analytics en el portal](./media/stream-analytics-managed-identities-adls/stream-analytics-access-list.png)

   Para más información acerca de los permisos del sistema de archivos de Data Lake Storage Gen1, consulte [Control de acceso en Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-access-control.md).

## <a name="stream-analytics-tools-for-visual-studio"></a>Herramientas de Stream Analytics para Visual Studio

1. En JobConfig.json, establezca **Use System-assigned Identity** (Usar identidad asignada por el sistema) en **True**.

   ![Configuración de identidades administradas para trabajos de Stream Analytics](./media/stream-analytics-managed-identities-adls/adls-mi-jobconfig-vs.png)

2. En la ventana de propiedades de salida del receptor de salida ADLS Gen1, haga clic en la lista desplegable Modo de autenticación y seleccione **Identidad administrada**.

   ![Salida de identidades administradas de ADLS](./media/stream-analytics-managed-identities-adls/adls-mi-output-vs.png)

3. Rellene el resto de las propiedades y haga clic en **Guardar**.

4. Haga clic en **Enviar a Azure** en el editor de consultas.

   Al enviar el trabajo, las herramientas lo hacen dos cosas:

   * Crean automáticamente una entidad de servicio para la identidad del trabajo de Stream Analytics en Azure Active Directory. El ciclo de vida de la identidad recién creada lo administrará Azure. Cuando se elimina el trabajo de Stream Analytics, Azure elimina automáticamente la identidad asociada (es decir, la entidad de servicio).

   * Establecen automáticamente los permisos **Escritura** y **Ejecución** para la ruta de acceso del prefijo de ADLS Gen1 usada en el trabajo, y los asignan a esta carpeta y a todos los elementos secundarios.

5. Puede generar las plantillas de Resource Manager con la siguiente propiedad mediante el [paquete Nuget Stream Analytics CI.CD](https://www.nuget.org/packages/Microsoft.Azure.StreamAnalytics.CICD/), versión 1.5.0 o posteriores, en una máquina de compilación (fuera de Visual Studio). Siga los pasos de implementación de la plantilla de Resource Manager indicados en la sección siguiente para obtener la entidad de servicio y conceda acceso a la entidad de servicio mediante PowerShell.

## <a name="resource-manager-template-deployment"></a>Implementación de plantillas del Administrador de recursos

1. Puede crear un recurso *Microsoft.StreamAnalytics/streamingjobs* con una identidad administrada mediante la inclusión de la siguiente propiedad en la sección de recursos de la plantilla de Resource Manager:

    ```json
    "Identity": {
      "Type": "SystemAssigned",
    },
    ```

   Esta propiedad indica a Azure Resource Manager que cree y administre la identidad del trabajo de Azure Stream Analytics.

   **Trabajo de ejemplo**
   
   ```json
   {
     "Name": "AsaJobWithIdentity",
     "Type": "Microsoft.StreamAnalytics/streamingjobs",
     "Location": "West US",
     "Identity": {
       "Type": "SystemAssigned",
     },
     "properties": {
       "sku": {
         "name": "standard"
       },
       "outputs": [
         {
           "name": "string",
           "properties":{
             "datasource": {
               "type": "Microsoft.DataLake/Accounts",
               "properties": {
                 "accountName": "myDataLakeAccountName",
                 "filePathPrefix": "cluster1/logs/{date}/{time}",
                 "dateFormat": "YYYY/MM/DD",
                 "timeFormat": "HH",
                 "authenticationMode": "Msi"
             }
           }
         }
       }
     }
   }
   ```
  
   **Respuesta del trabajo de ejemplo**

   ```json
   {
    "Name": "mySAJob",
    "Type": "Microsoft.StreamAnalytics/streamingjobs",
    "Location": "West US",
    "Identity": {
      "Type": "SystemAssigned",
        "principalId": "GUID",
        "tenantId": "GUID",
      },
      "properties": {
        "sku": {
          "name": "standard"
        },
     }
   }
   ```

   Anota el identificador de la entidad de seguridad de la respuesta del trabajo para conceder acceso a los recursos de ADLS necesarios.

   **Id. del inquilino** es el identificador del inquilino de Azure Active Directory en el que se crea la entidad de servicio. La entidad de servicio se crea en el inquilino de Azure AD en el que la suscripción confía.

   **Tipo** indica el tipo de identidad administrada, como se explica en los tipos de identidades administradas. Solo se admite el tipo Asignado por el sistema.

2. Proporcione acceso a la entidad de servicio mediante PowerShell. Para conceder acceso a la entidad de servicio a través de PowerShell, ejecute el siguiente comando:

   ```powershell
   Set-AzDataLakeStoreItemAclEntry -AccountName <accountName> -Path <Path> -AceType User -Id <PrinicpalId> -Permissions <Permissions>
   ```

   **PrincipalId** es el identificador de objeto de la entidad de servicio y se muestra en la pantalla del portal una vez que se crea la entidad de servicio. Si ha creado el trabajo mediante la implementación de una plantilla de Resource Manager, el identificador de objeto aparece en la propiedad Identity de la respuesta del trabajo.

   **Ejemplo**

   ```powershell
   PS > Set-AzDataLakeStoreItemAclEntry -AccountName "adlsmsidemo" -Path / -AceType
   User -Id 14c6fd67-d9f5-4680-a394-cd7df1f9bacf -Permissions WriteExecute
   ```

   Para más información sobre el comandos de PowerShell anterior, consulte la documentación de [Set-AzDataLakeStoreItemAclEntry](/powershell/module/az.datalakestore/set-azdatalakestoreitemaclentry).

## <a name="limitations"></a>Limitaciones
Esta característica no admite lo siguiente:

1. **Acceso multiinquilino**: la entidad de servicio creada para un trabajo determinado de Stream Analytics residirá en el inquilino de Azure Active Directory en el que se creó el trabajo, y no se puede usar en un recurso que resida en un inquilino de Azure Active Directory diferente. Por lo tanto, solo puede usar MSI en recursos de ADLS Gen 1 que se encuentren dentro del mismo inquilino de Azure Active Directory que el trabajo de Azure Stream Analytics. 

2. **[Identidad asignada por el usuario](../active-directory/managed-identities-azure-resources/overview.md)** : no se admite. Esto significa que el usuario no puede especificar el uso de su propia entidad de servicio en su trabajo de Stream Analytics. La entidad de servicio se genera mediante Azure Stream Analytics.

## <a name="next-steps"></a>Pasos siguientes

* [Creación de una salida de Data Lake Store con Stream Analytics](../data-lake-store/data-lake-store-stream-analytics.md)
* [Prueba de las consultas de Stream Analytics localmente con Visual Studio](stream-analytics-vs-tools-local-run.md)
* [Prueba local de datos activos mediante las herramientas de Azure Stream Analytics para Visual Studio](stream-analytics-live-data-local-testing.md) 
