---
title: 'Tutorial: Acceso a Blob Storage mediante el almacén de claves: Azure Databricks'
description: En este tutorial se describe cómo acceder a Azure Blob Storage desde Azure Databricks mediante secretos almacenados en un almacén de claves.
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: tutorial
ms.date: 07/19/2019
ms.openlocfilehash: 026165c7c2052992e8ab485f9ab81c8964f38235
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83647971"
---
# <a name="tutorial-access-azure-blob-storage-from-azure-databricks-using-azure-key-vault"></a>Tutorial: Acceso a Azure Blob Storage desde Azure Databricks mediante el tutorial de Azure Key Vault

En este tutorial se describe cómo acceder a Azure Blob Storage desde Azure Databricks mediante secretos almacenados en un almacén de claves.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear un contenedor de blobs y una cuenta de almacenamiento
> * Crear una instancia de Azure Key Vault y agregar un secreto
> * Crear un área de trabajo de Azure Databricks y agregar un ámbito de secreto
> * Acceder al contenedor de blobs desde Azure Databricks

## <a name="prerequisites"></a>Prerrequisitos

- Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/)

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en [Azure Portal](https://portal.azure.com/).

> [!Note]
> Este tutorial no puede llevarse a cabo mediante una **suscripción de evaluación gratuita de Azure**.
> Si tiene una cuenta gratuita, vaya a su perfil y cambiar la suscripción a **pago por uso**. Para más información consulte el sitio de [cuentas gratuitas de Azure](https://azure.microsoft.com/free/). Después, [quite el límite de gasto](https://docs.microsoft.com/azure/billing/billing-spending-limit#why-you-might-want-to-remove-the-spending-limit) y [solicite un aumento de la cuota](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request) para las vCPU de su región. Cuando crea su área de trabajo de Azure Databricks, puede seleccionar el plan de tarifa de la **Trial (Premium - 14-Days Free DBUs)** para que el área de trabajo acceda a las DBU Premium de Azure Databricks gratis durante 14 días.

## <a name="create-a-storage-account-and-blob-container"></a>Crear un contenedor de blobs y una cuenta de almacenamiento

1. En Azure Portal, seleccione **Crear un recurso** > **Almacenamiento**. Después, seleccione **Cuenta de almacenamiento**.

   ![Búsqueda del recurso de la cuenta de almacenamiento de Azure](./media/store-secrets-azure-key-vault/create-storage-account-resource.png)

2. Seleccione la suscripción y el grupo de recursos o cree uno nuevo. Escriba un nombre para la cuenta de almacenamiento y elija una ubicación. Seleccione **Revisar + crear**.

   ![Definir las propiedades de una cuenta de almacenamiento](./media/store-secrets-azure-key-vault/create-storage-account.png)

3. Si la validación no se realiza correctamente, solucione los problemas e inténtelo de nuevo. Si la validación es correcta, seleccione **Crear** y espere a que se cree la cuenta de almacenamiento.

4. Vaya a la cuenta de almacenamiento recién creada y seleccione **Blobs** en **Servicios** de la página **Información general**. Después, seleccione **+ Contenedor** y escriba un nombre de contenedor. Seleccione **Aceptar**.

   ![Crear un contenedor nuevo](./media/store-secrets-azure-key-vault/create-blob-storage-container.png)

5. Busque un archivo para cargar en el contenedor de almacenamiento de blobs. Si no tiene un archivo, use un editor de texto para crear un archivo de texto con algo de información. En este ejemplo, un archivo denominado **hw.txt** contiene el texto "hello world". Guarde el archivo de texto localmente y cárguelo en el contenedor de almacenamiento de blobs.

   ![Carga del archivo en el contenedor](./media/store-secrets-azure-key-vault/upload-txt-file.png)

6. Vuelva a la cuenta de almacenamiento y seleccione **Claves de acceso** en **Configuración**. Copie el **nombre de la cuenta de almacenamiento** y la **clave 1** en un editor de texto para usarlos más adelante en este tutorial.

   ![Búsqueda de las claves de acceso a la cuenta de almacenamiento](./media/store-secrets-azure-key-vault/storage-access-keys.png)

## <a name="create-an-azure-key-vault-and-add-a-secret"></a>Crear una instancia de Azure Key Vault y agregar un secreto

1. En Azure Portal, seleccione **Crear un recurso** y escriba **Key Vault** en la barra de búsqueda.

   ![Cuadro de búsqueda de creación de un recurso de Azure](./media/store-secrets-azure-key-vault/find-key-vault-resource.png)

2. El recurso de Key Vault se selecciona automáticamente. Seleccione **Crear**.

   ![Creación de un recurso de Key Vault](./media/store-secrets-azure-key-vault/create-key-vault-resource.png)

3. En la página **Crear almacén de claves** escriba la información siguiente y mantenga los valores predeterminados para los campos restantes:

   |Propiedad|Descripción|
   |--------|-----------|
   |Nombre|Nombre único del almacén de claves.|
   |Subscription|Elija una suscripción.|
   |Resource group|Elija un grupo de recursos o cree uno.|
   |Location|Elija una ubicación.|

   ![Propiedades del almacén de claves de Azure](./media/store-secrets-azure-key-vault/create-key-vault-properties.png)

3. Después de proporcionar la información descrita anteriormente, seleccione **Crear**. 

4. Vaya al almacén de claves recién creado en Azure Portal y seleccione **Secretos**. A continuación, seleccione **+ Generate/Import** (+ Generar/Importar). 

   ![Generación del secreto del almacén de claves](./media/store-secrets-azure-key-vault/generate-import-secrets.png)

5. En la página **Crear un secreto** proporcione la información siguiente y mantenga los valores predeterminados para los campos restantes:

   |Propiedad|Value|
   |--------|-----------|
   |Opciones de carga|Manual|
   |Nombre|Nombre descriptivo de la clave de la cuenta de almacenamiento.|
   |Value|key1 de la cuenta de almacenamiento.|

   ![Propiedades del nuevo secreto del almacén de claves](./media/store-secrets-azure-key-vault/create-storage-secret.png)

6. Guarde el nombre de la clave en un editor de texto para usarlo más adelante en este tutorial y seleccione **Crear**. Después, vaya al menú **Propiedades**. Copie el **Nombre de DNS** y el **Id. de recurso** en un editor de texto para usarlos más adelante en el tutorial.

   ![Copia del nombre de DNS y el Id. de recurso de Azure Key Vault](./media/store-secrets-azure-key-vault/copy-dns-resource.png)

## <a name="create-an-azure-databricks-workspace-and-add-a-secret-scope"></a>Crear un área de trabajo de Azure Databricks y agregar un ámbito de secreto

1. En Azure Portal, seleccione **Crear un recurso** > **Análisis** > **Azure Databricks**.

    ![Databricks de Azure Portal](./media/store-secrets-azure-key-vault/azure-databricks-on-portal.png)

2. En el **servicio Azure Databricks**, proporcione los valores siguientes para crear un área de trabajo de Databricks.

   |Propiedad  |Descripción  |
   |---------|---------|
   |Nombre del área de trabajo     | Proporcione un nombre para el área de trabajo de Databricks        |
   |Subscription     | En el cuadro desplegable, seleccione la suscripción de Azure.        |
   |Resource group     | Seleccione el mismo grupo de recursos que contiene el almacén de claves. |
   |Location     | Seleccione la misma ubicación que tiene la instancia de Azure Key Vault. Para todas las regiones disponibles, consulte el artículo de [servicios de Azure disponibles por región](https://azure.microsoft.com/regions/services/).        |
   |Nivel de precios     |  Elija entre **Standard** o **Premium**. Para más información sobre estos planes, consulte la [página de precios de Databricks](https://azure.microsoft.com/pricing/details/databricks/).       |

   ![Propiedades del área de trabajo de Databricks](./media/store-secrets-azure-key-vault/create-databricks-service.png)

   Seleccione **Crear**.

3. En Azure Portal, vaya al recurso de Azure Databricks recién creado y seleccione **Launch Workspace** (Iniciar área de trabajo).

   ![Inicio del área de trabajo de Azure Databricks](./media/store-secrets-azure-key-vault/launch-databricks-workspace.png)

4. Una vez abierta el área de trabajo de Azure Databricks en una ventana independiente, anexe **#secrets/createScope** a la dirección URL. La dirección URL debe tener el siguiente formato: 

   **https://<\location>.azuredatabricks.net/?o=<\orgID>#secrets/createScope**.
   

5. Escriba un nombre de ámbito y el nombre DNS y el identificador de recurso de Azure Key Vault que guardó anteriormente. Guarde el nombre del ámbito en un editor de texto para usarlo más adelante en este tutorial. Seleccione **Crear**.

   ![Creación de ámbito de secreto en el área de trabajo de Azure Databricks](./media/store-secrets-azure-key-vault/create-secret-scope.png)

## <a name="access-your-blob-container-from-azure-databricks"></a>Acceder al contenedor de blobs desde Azure Databricks

1. En la página principal del área de trabajo de Azure Databricks, seleccione **Nuevo clúster** en **Tareas comunes**.

   ![Creación de un cuaderno en Azure Databricks](./media/store-secrets-azure-key-vault/create-new-cluster.png)

2. Escriba un nombre de clúster y seleccione **Create cluster** (Crear clúster). El clúster tarda unos minutos en crearse.

3. Una vez creado el clúster, vaya a la página principal del área de trabajo de Azure Databricks y seleccione **New Notebook** (Nuevo bloc de notas) en **Tareas comunes**.

   ![Creación de un cuaderno en Azure Databricks](./media/store-secrets-azure-key-vault/create-new-notebook.png)

4. Escriba un nombre de cuaderno y establezca el lenguaje en Python. Establezca el nombre del clúster que creó en el paso anterior.

5. Ejecute el siguiente comando para montar el contenedor de almacenamiento de blobs. Recuerde cambiar los valores de las siguientes propiedades:

   * your-container-name
   * your-storage-account-name
   * mount-name
   * config-key
   * scope-name
   * key-name

   ```python
   dbutils.fs.mount(
   source = "wasbs://<your-container-name>@<your-storage-account-name>.blob.core.windows.net",
   mount_point = "/mnt/<mount-name>",
   extra_configs = {"<conf-key>":dbutils.secrets.get(scope = "<scope-name>", key = "<key-name>")})
   ```

   * **mount-name** es una ruta de acceso de DBFS que representa la ubicación en la que se montará el contenedor de Blob Storage o una carpeta dentro del contenedor (especificado en el origen).
   * **conf-key** puede ser `fs.azure.account.key.<\your-storage-account-name>.blob.core.windows.net` o `fs.azure.sas.<\your-container-name>.<\your-storage-account-name>.blob.core.windows.net`.
   * **scope-name** es el nombre del ámbito del secreto que creó en la sección anterior. 
   * **key-name** es el nombre del secreto que creó para la clave de la cuenta de almacenamiento en el almacén de claves.

   ![Creación de un montaje de almacenamiento de blobs en un cuaderno](./media/store-secrets-azure-key-vault/command1.png)

6. Ejecute el siguiente comando para leer el archivo de texto del contenedor de almacenamiento de blobs en una trama de datos. Cambie los valores del comando para que coincidan con el nombre de montaje y el de archivo.

   ```python
   df = spark.read.text("/mnt/<mount-name>/<file-name>")
   ```

   ![Lectura de un archivo en una trama de datos](./media/store-secrets-azure-key-vault/command2.png)

7. Use el siguiente comando para ver el contenido del archivo.

   ```python
   df.show()
   ```
   ![Mostrar la trama de datos](./media/store-secrets-azure-key-vault/command3.png)

8. Para desmontar el contenedor de almacenamiento de blobs, ejecute el siguiente comando:

   ```python
   dbutils.fs.unmount("/mnt/<mount-name>")
   ```

   ![Desmontar la cuenta de almacenamiento](./media/store-secrets-azure-key-vault/command4.png)

9. Observe que una vez que se haya desmontado el montaje, ya no podrá leer de la cuenta de almacenamiento de blobs.

   ![Error al desmontar la cuenta de almacenamiento](./media/store-secrets-azure-key-vault/command5.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no va a seguir usando esta aplicación, elimine el grupo de recursos mediante los siguientes pasos:

1. En el menú izquierdo de Azure Portal, seleccione **Grupos de recursos** y vaya al grupo de recursos en cuestión.

2. Seleccione **Eliminar grupo de recursos** y escriba el nombre del grupo de recursos. A continuación, seleccione **Eliminar**. 

## <a name="next-steps"></a>Pasos siguientes

Continúe con el siguiente artículo para aprender a implementar un entorno de Databricks que tenga una red virtual insertada y un punto de conexión de servicio habilitado para Cosmos DB.
> [!div class="nextstepaction"]
> [Tutorial: Implementar Azure Databricks con un punto de conexión de Cosmos DB](service-endpoint-cosmosdb.md)
