---
title: Cifrado de disco mediante claves administradas por el cliente para Azure HDInsight
description: En este artículo se describe cómo puede usar su propia clave de cifrado desde Azure Key Vault a fin de cifrar los datos almacenados en discos administrados en clústeres de Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: c0521f384a333c3054397fb0ec7c2ab907e54f67
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2020
ms.locfileid: "80411748"
---
# <a name="customer-managed-key-disk-encryption"></a>Cifrado de disco mediante claves administradas por el cliente

Azure HDInsight admite el cifrado de claves administradas por el cliente para los datos de los discos administrados y los discos de recursos asociados a las máquinas virtuales del clúster de HDInsight. Esta característica permite usar Azure Key Vault para administrar las claves de cifrado que protegen los datos en reposo en los clústeres de HDInsight.

Todos los discos administrados en HDInsight están protegidos con Azure Storage Service Encryption (SSE). De forma predeterminada, los datos en esos discos se cifran mediante claves administradas por Microsoft. Si habilita las claves administradas por el cliente para HDInsight, debe proporcionar las claves de cifrado para que HDInsight las use y administre mediante Azure Key Vault.

En este documento no se tratan los datos almacenados en la cuenta de Azure Storage. Para más información sobre el cifrado de almacenamiento, consulte [Cifrado de Azure Storage para datos en reposo](../storage/common/storage-service-encryption.md). Los clústeres pueden tener una o varias cuentas de Azure Storage asociadas, en las que el cliente o Microsoft también puede administrar las claves de cifrado, sin embargo, el servicio de cifrado es diferente.

## <a name="introduction"></a>Introducción

El cifrado de claves administradas por el cliente consiste en un proceso de un paso controlado durante la creación del clúster sin ningún costo adicional. Solo necesita registrar HDInsight como una identidad administrada con Azure Key Vault y agregar la clave de cifrado al crear el clúster.

Tanto el disco de recursos como los discos administrados de cada nodo del clúster están cifrados con una clave de cifrado de datos (DEK) simétrica. La DEK está protegida mediante la clave de cifrado de claves (KEK) del almacén de claves. Azure HDInsight controla completamente los procesos de cifrado y descifrado.

Si el firewall del almacén de claves está habilitado en el almacén de claves donde está almacenada la clave de cifrado del disco, las direcciones IP del proveedor de recursos regional de HDInsight de la región donde se va a implementar el clúster se deben agregar a la configuración del firewall del almacén de claves. Esto es necesario porque HDInsight no es un servicio de almacén de claves de Azure de confianza.

Puede usar Azure Portal o la CLI de Azure para rotar las claves en el almacén de claves de forma segura. Cuando se gira una clave, el clúster de HDInsight comienza a usar la clave nueva en cuestión de minutos. Habilite las características de protección de claves [Eliminación temporal](../key-vault/key-vault-ovw-soft-delete.md) para protegerse frente a los escenarios de ransomware y la eliminación accidental. No se admiten almacenes de claves sin esta característica de protección.

|Tipo de clúster |Disco del sistema operativo (disco administrado) |Disco de datos (disco administrado) |Disco de datos temporales (SSD local) |
|---|---|---|---|
|Kafka, HBase con escrituras aceleradas|[Cifrado SSE](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview#encryption)|Cifrado SSE + cifrado opcional de CMK|Cifrado opcional de CMK|
|Todos los demás clústeres (Spark, Interactive, Hadoop, HBase sin escrituras aceleradas)|Cifrado SSE|N/D|Cifrado opcional de CMK|

## <a name="get-started-with-customer-managed-keys"></a>Introducción a las claves administradas por el cliente

Para crear un clúster de HDInsight habilitado para claves administradas por el cliente, repasaremos los pasos siguientes:

1. Creación de identidades administradas para los recursos de Azure
1. Crear una instancia de Azure Key Vault
1. Crear clave
1. Creación de directivas de acceso
1. Creación de un clúster de HDInsight con la clave administrada por el cliente habilitada
1. Rotación de la clave de cifrado

## <a name="create-managed-identities-for-azure-resources"></a>Creación de identidades administradas para los recursos de Azure

Cree una identidad administrada asignada por el usuario para autenticarse en Key Vault.

Consulte [Creación de una identidad administrada asignada por el usuario](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md) para conocer los pasos específicos. Para obtener más información sobre cómo funcionan las identidades administradas en Azure HDInsight, vea [Identidades administradas en Azure HDInsight](hdinsight-managed-identities.md). Asegúrese de guardar el identificador de recurso de identidad administrada para cuando lo agregue a la directiva de acceso de Key Vault.

## <a name="create-azure-key-vault"></a>Crear una instancia de Azure Key Vault

Cree un almacén de claves. Consulte [Creación de una instancia de Azure Key Vault](../key-vault/quick-create-portal.md) para conocer los pasos específicos.

HDInsight solo es compatible con Azure Key Vault. Si tiene su propio almacén de claves, puede importar las claves a Azure Key Vault. Recuerde que el almacén de claves debe tener habilitada la **eliminación temporal**. Para obtener más información sobre cómo importar claves existentes, visite [Información acerca de claves, secretos y certificados](../key-vault/about-keys-secrets-and-certificates.md).

## <a name="create-key"></a>Crear clave

1. En el nuevo almacén de claves, vaya a **Configuración** > **Claves** >  **+ Generate/Import** (+ Generar/Importar).

    ![Generación de una clave nueva en Azure Key Vault](./media/disk-encryption/create-new-key.png "Generar una clave nueva en Azure Key Vault")

1. Proporcione un nombre y, luego, seleccione **Crear**. Mantenga el valor de **Tipo de clave** en **RSA**.

    ![generación de un nombre de clave](./media/disk-encryption/create-key.png "Generación de un nombre de clave")

1. Cuando vuelva a la página **Claves**, seleccione la clave que ha creado.

    ![lista de claves de Key Vault](./media/disk-encryption/key-vault-key-list.png)

1. Seleccione la versión para abrir la página **Versión de la clave**. Cuando usa su propia clave para el cifrado del clúster de HDInsight, tiene que proporcionar el URI de la clave. Copie el **identificador de clave** y guárdelo en algún lugar hasta que vaya a crear el clúster.

    ![obtención del identificador de clave](./media/disk-encryption/get-key-identifier.png)

## <a name="create-access-policy"></a>Creación de directivas de acceso

1. En el nuevo almacén de claves, vaya a **Configuración** > **Directivas de acceso** >  **+ Agregar directiva de acceso**.

    ![Creación de una directiva de acceso de Azure Key Vault](./media/disk-encryption/key-vault-access-policy.png)

1. En la página **Agregar directiva de acceso**, proporcione la siguiente información:

    |Propiedad |Descripción|
    |---|---|
    |Permisos de claves|Seleccione **Obtener**, **Desencapsular clave** y **Encapsular clave**.|
    |Permisos de secretos|Seleccione **Obtener**, **Establecer** y **Eliminar**.|
    |Selección de la entidad de seguridad|Seleccione la identidad administrada asignada por el usuario que creó anteriormente.|

    ![Establecimiento de la entidad de seguridad para la directiva de acceso de Azure Key Vault](./media/disk-encryption/azure-portal-add-access-policy.png)

1. Seleccione **Agregar**.

1. Seleccione **Guardar**.

    ![Guardar la directiva de acceso de Azure Key Vault](./media/disk-encryption/add-key-vault-access-policy-save.png)

## <a name="create-cluster-with-customer-managed-key-disk-encryption"></a>Creación de un clúster con el cifrado de disco mediante claves administradas por el cliente

Ya está listo para crear un clúster de HDInsight. La clave administrada por el cliente solo puede aplicarse a los clústeres nuevos durante la creación del clúster. No se puede quitar el cifrado de los clústeres de las claves administradas por el cliente y no se puede agregar la clave administrada por el cliente a los clústeres existentes.

### <a name="using-the-azure-portal"></a>Uso de Azure Portal

Durante la creación del clúster, proporcione el **identificador de clave** completo, incluida la versión de la clave. Por ejemplo, `https://contoso-kv.vault.azure.net/keys/myClusterKey/46ab702136bc4b229f8b10e8c2997fa4`. También deberá asignar la identidad administrada al clúster y proporcionar el URI de la clave.

![Creación de un clúster](./media/disk-encryption/create-cluster-portal.png)

### <a name="using-azure-cli"></a>Uso de la CLI de Azure

En el ejemplo siguiente se muestra cómo usar la CLI de Azure para crear un nuevo clúster de Apache Spark con el cifrado de disco habilitado. Para más información, consulte el comando [az hdinsight create de la CLI de Azure](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-create).

```azurecli
az hdinsight create -t spark -g MyResourceGroup -n MyCluster \
-p "HttpPassword1234!" --workernode-data-disks-per-node 2 \
--storage-account MyStorageAccount \
--encryption-key-name SparkClusterKey \
--encryption-key-version 00000000000000000000000000000000 \
--encryption-vault-uri https://MyKeyVault.vault.azure.net \
--assign-identity MyMSI
```

## <a name="rotating-the-encryption-key"></a>Rotación de la clave de cifrado

Puede haber escenarios en los que quiera cambiar las claves de cifrado que se usan en el clúster de HDInsight una vez creado. Esta tarea se puede realizar fácilmente en el portal. En esta operación, el clúster debe tener acceso a la clave actual y a la clave nueva deseada; si no, se producirá un error en la operación de rotación de la clave.

### <a name="using-the-azure-portal"></a>Uso de Azure Portal

Para rotar la clave, necesita el URI del almacén de claves base. Una vez lo haya hecho, vaya a la sección de propiedades del clúster de HDInsight en el portal y haga clic en **Cambiar clave** en **URL de clave de cifrado de disco**. Escriba la dirección URL de la nueva clave y envíela para rotar la clave.

![rotación de la clave de cifrado de disco](./media/disk-encryption/change-key.png)

### <a name="using-azure-cli"></a>Uso de la CLI de Azure

En el ejemplo siguiente se muestra cómo girar la clave de cifrado de disco de un clúster de HDInsight existente. Para más información, consulte el comando [az hdinsight rotate-disk-encryption-key de la CLI de Azure](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-rotate-disk-encryption-key).

```azurecli
az hdinsight rotate-disk-encryption-key \
--encryption-key-name SparkClusterKey \
--encryption-key-version 00000000000000000000000000000000 \
--encryption-vault-uri https://MyKeyVault.vault.azure.net \
--name MyCluster \
--resource-group MyResourceGroup
```

## <a name="azure-resource-manager-templates"></a>Plantillas del Administrador de recursos de Azure

Para usar las claves administradas por el cliente mediante el uso de una plantilla de Resource Manager, actualice la plantilla con los siguientes cambios:

1. En el archivo **azuredeploy.json**, agregue la siguiente propiedad al objeto "resources":

    ```json
       "diskEncryptionProperties":
         {
                 "vaultUri": "[parameters('diskEncryptionVaultUri')]",
                  "keyName": "[parameters('diskEncryptionKeyName')]",
                  "keyVersion": "[parameters('diskEncryptionKeyVersion')]",
                   "msiResourceId": "[parameters('diskEncryptionMsiResourceId')]"
         }

1. In the **azuredeploy.parameters.json** file, add the following parameters. You can get the values of these parameters from the Key Vault URI and the managed Identity. For example, if you have the following URI and identity values,
    * Sample key vault URI: https://<KeyVault_Name>.vault.azure.net/keys/clusterkey/<Cluster_Key_Value>
    * Sample user-assigned managed identity: "/subscriptions/<subscriptionID>/resourcegroups/<ResourceGroup_Name>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI_Name>

    The parameters in the **azuredeploy.parameters.json** file are:

    ```json
   "diskEncryptionVaultUri": {
            "value": "https://<KeyVault_Name>.vault.azure.net"
        },
        "diskEncryptionKeyName": {
            "value": "clusterkey"
        },
        "diskEncryptionKeyVersion": {
            "value": "<Cluster_Key_Value>"
        },
        "diskEncryptionMsiResourceId": {
            "value": "/subscriptions/<subscriptionID>/resourcegroups/<ResourceGroup_Name>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI_Name>"
        }
    ```

## <a name="faq-for-customer-managed-key-encryption"></a>Preguntas más frecuentes del cifrado de claves administradas por el cliente

**¿Cómo accede el clúster de HDInsight a mi almacén de claves?**

HDInsight accede a su instancia de Azure Key Vault mediante la identidad administrada que se asocia con el clúster de HDInsight. Esta identidad administrada puede crearse antes o durante la creación del clúster. También debe conceder acceso a la identidad administrada para que pueda usar el almacén de claves donde se almacena la clave.

**¿Esta característica está disponible para todos los clústeres de HDInsight?**

El cifrado de claves administradas por el cliente está disponible para todos los tipos de clúster, excepto Spark 2.1 y 2.2.

**¿Puedo usar varias claves para cifrar diferentes discos o carpetas?**

No, todos los discos administrados y los discos de recursos se cifran con la misma clave.

**¿Qué ocurre si el clúster pierde el acceso al almacén de claves o a la clave?**

Si el clúster pierde acceso a la clave, se mostrarán advertencias en el portal de Apache Ambari. En este estado, la operación **Cambiar clave** dará error. Cuando se restaura el acceso a la clave, las advertencias de Ambari desaparecen y operaciones como la rotación de claves se pueden realizar correctamente.

![alerta de Ambari de acceso a la clave](./media/disk-encryption/ambari-alert.png)

**¿Cómo puedo recuperar el clúster si se eliminan las claves?**

Dado que solo se admiten claves habilitadas para eliminación temporal, si las claves se recuperan en el almacén de claves, el clúster debe volver a acceder a ellas. Para recuperar una clave de Azure Key Vault, consulte [Undo-AzKeyVaultKeyRemoval](/powershell/module/az.keyvault/Undo-AzKeyVaultKeyRemoval) o [az-keyvault-key-recover](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-recover).

**¿Qué tipos de disco están cifrados? ¿Los discos de sistema operativo o de recursos también se cifran?**

Los discos de recursos, los datos y los discos administrados están cifrados. Los discos del sistema operativo no están cifrados.

**Si un clúster se escala verticalmente, ¿los nuevos nodos admitirán las claves administradas por el cliente sin problemas?**

Sí. El clúster necesita tener acceso a la clave del almacén de claves durante el escalado vertical. Se usa la misma clave para cifrar los discos administrados y de recursos en el clúster.

**Las claves administradas por el cliente están disponibles en mi ubicación?**

Las claves administradas por el cliente de HDInsight están disponibles en todas las nubes públicas y nubes nacionales.

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre Azure Key Vault, consulte [¿Qué es Azure Key Vault?](../key-vault/key-vault-overview.md).
* [Introducción a la seguridad de la empresa en Azure HDInsight](./domain-joined/hdinsight-security-overview.md).
